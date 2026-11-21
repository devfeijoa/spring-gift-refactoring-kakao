# 주문 생성 흐름

`POST /api/orders`는 옵션 하나를 선택한 수량만큼 구매하고 회원의 포인트로 결제한다. **옵션 재고, 회원 포인트, 주문을 이 순서로 각각 저장한다.** 세 번의 저장을 묶는 `@Transactional`이 없어 뒤 단계에서 실패해도 앞에서 커밋한 값은 남는다. [주문 생성 코드](../../../src/main/java/gift/order/OrderController.java#L69)

## 요청부터 응답까지

클라이언트는 `Authorization: Bearer <서비스 JWT>` 헤더와 JSON 본문을 보낸다. 예를 들어 `{"optionId":7,"quantity":2,"message":"생일 축하해요"}`는 옵션 7을 2개 주문하는 요청이다. 아래 단계가 성공하면 마지막 단계까지 HTTP 응답을 보류한다.

| 단계 | 확인하거나 실행하는 일 | 저장하는 값 또는 응답 |
| --- | --- | --- |
| 1. 요청 검증 | Spring MVC가 필수 헤더와 JSON 본문을 읽고, `optionId`가 `null`이 아닌지, `quantity`가 1 이상인지 검사한다. `message`는 생략할 수 있다. | DB 접근 없이 검증을 마치고 `createOrder`를 호출한다. [요청 필드](../../../src/main/java/gift/order/OrderRequest.java#L6) |
| 2. 회원 인증 | `extractMember`가 헤더에서 JWT를 읽고 서명과 만료 여부를 확인한다. 토큰의 이메일로 회원을 찾는다. | 회원을 조회한다. [인증 처리](../../../src/main/java/gift/auth/AuthenticationResolver.java#L25) · [JWT 검증](../../../src/main/java/gift/auth/JwtProvider.java) |
| 3. 옵션 조회 | `optionRepository.findById(optionId)`로 구매할 옵션을 찾는다. | 옵션과 연결된 상품을 이후 계산에 사용한다. [옵션 조회](../../../src/main/java/gift/order/OrderController.java#L81) |
| 4. 재고 차감 | `subtractQuantity`가 요청 수량과 옵션 재고를 비교한다. 재고가 충분하면 요청 수량만큼 뺀다. | `optionRepository.save`가 줄어든 재고를 `options.quantity`에 저장한다. [재고 검사](../../../src/main/java/gift/option/Option.java#L39) · [재고 저장](../../../src/main/java/gift/order/OrderController.java#L87) |
| 5. 포인트 결제 | 상품 단가 × 주문 수량으로 결제 금액을 계산한다. `deductPoint`는 금액이 1 이상이고 회원 잔액 이하일 때 그만큼 뺀다. | `memberRepository.save`가 결제 후 잔액을 `member.point`에 저장한다. [잔액 검사](../../../src/main/java/gift/member/Member.java#L57) · [금액 계산과 저장](../../../src/main/java/gift/order/OrderController.java#L91) |
| 6. 주문 저장 | 인증한 회원, 선택한 옵션, 요청 수량·메시지로 `Order`를 만든다. 주문 시각은 생성 시점의 서버 시각이다. | `orderRepository.save`가 `orders`에 한 행을 추가한다. DB가 주문 ID를 부여하고 옵션 ID, 회원 ID, 수량, 메시지, 주문 시각을 보관한다. [주문 엔티티](../../../src/main/java/gift/order/Order.java#L15) · [주문 저장](../../../src/main/java/gift/order/OrderController.java#L96) |
| 7. 카카오 알림 | 회원의 카카오 액세스 토큰이 `null`이면 건너뛴다. 값이 있으면 구매자 본인의 카카오톡으로 상품명·옵션명·수량·금액을 보낸다. 주문 메시지는 공백이 아닌 경우에 덧붙인다. | 전송을 동기로 시도하고 전송 결과는 DB에 기록하지 않는다. 예외가 발생해도 응답 단계로 간다. [전송 조건과 예외 처리](../../../src/main/java/gift/order/OrderController.java#L104) · [나에게 보내기 요청](../../../src/main/java/gift/order/KakaoMessageClient.java#L16) |
| 8. 생성 응답 | 저장한 주문을 `OrderResponse`로 변환한다. | **`201 Created`**, `Location: /api/orders/{id}`. JSON 본문에는 `id`, `optionId`, `quantity`, `orderDateTime`, `message`가 담긴다. [응답 생성](../../../src/main/java/gift/order/OrderController.java#L100) · [응답 필드](../../../src/main/java/gift/order/OrderResponse.java#L5) |

**찜은 주문 전 상태를 유지한다.** 컨트롤러 주석에는 주문 저장 뒤 찜을 정리한다고 적혀 있지만, `createOrder`는 `WishRepository`를 호출하지 않고 바로 카카오 알림으로 넘어간다. 이미 찜한 상품도 주문 후 목록에 남는다. [컨트롤러 주석과 구현](../../../src/main/java/gift/order/OrderController.java#L61) · [주문 전후 조회 기록](../../analysis.md#찜과-주문)

## 실패 시 응답과 남는 상태

아래 표는 이번 요청 한 건이 DB에 남긴 결과다. ‘유지’는 요청 직전 값, ‘차감’은 해당 단계에서 저장을 마친 값이다. 주문 저장에 이르기 전에 실패하거나 주문 저장 자체가 롤백되면 카카오 알림도 호출하지 않는다.

| 실패 시점과 조건 | 클라이언트가 받는 응답 | 옵션 재고 | 회원 포인트 | 새 주문 |
| --- | --- | --- | --- | --- |
| 필수 `Authorization` 헤더 누락, 본문 누락·JSON 해석 실패, `optionId` 누락·`null`, `quantity` 누락·1 미만 | `400` 기본 오류 JSON | 유지 | 유지 | 없음 |
| 본문이 지원하지 않는 미디어 타입으로 전송됨 (예: `text/plain`) | `415` 기본 오류 JSON | 유지 | 유지 | 없음 |
| JWT 해석·서명·만료 검증 실패, 이메일에 맞는 회원 없음, 인증 중 회원 조회 예외 | `401` 빈 본문 | 유지 | 유지 | 없음 |
| 요청한 ID의 옵션이 없음 | `404` 빈 본문 | 유지 | 유지 | 없음 |
| 옵션 조회 중 DB 예외 | `500` 기본 오류 JSON | 유지 | 유지 | 없음 |
| 요청 수량이 재고보다 많음 | `500` 기본 오류 JSON | 유지 | 유지 | 없음 |
| 재고 저장이 롤백됨 | `500` 기본 오류 JSON | 유지 | 유지 | 없음 |
| 계산한 결제 금액이 0 이하이거나 회원 포인트가 부족함 | `500` 기본 오류 JSON | **차감** | 유지 | 없음 |
| 포인트 저장이 롤백됨 | `500` 기본 오류 JSON | **차감** | 유지 | 없음 |
| 주문 저장이 롤백됨. 예를 들어 요청에서 길이를 검사하지 않은 `message`가 DB의 `varchar(255)` 한도를 넘어 거절된 경우 | `500` 기본 오류 JSON | **차감** | **차감** | 없음 |
| 주문 저장 후 카카오 전송에서 예외 발생 | **`201 Created`**, 생성된 주문 본문과 `Location` | 차감 | 차감 | **1건** |

재고 부족과 포인트 부족은 모두 `IllegalArgumentException`을 던진다. 주문 컨트롤러에는 이 예외를 처리할 핸들러가 없어 두 경우 모두 `500`을 반환한다. 기본 오류 JSON에는 `timestamp`, `status`, `error`, `path`가 담긴다. 기존 실행 기록에서 잔액이 0인 회원이 재고 100개인 옵션을 1개 주문했을 때, 주문은 생기지 않았고 재고는 99개로 줄었다. 저장 롤백 시의 상태는 각 저장 호출의 순서와 DB 제약을 기준으로 정리했다. [실패 요청 실행 기록](../../analysis.md#실패예외-시나리오) · [주문 테이블 제약](../../../src/main/resources/db/migration/V1__Initialize_project_tables.sql#L47)

# 용어 사전

| 한글명 | 영문명 | 설명 |
| --- | --- | --- |
| 상품 | Product | 회원이 위시에 담거나 주문할 수 있는 판매 대상. 이름, 가격, 이미지 주소를 가진다. |
| 카테고리 | Category | 상품을 묶는 분류. 기본 카테고리는 전자기기, 패션, 식품이다. |
| 옵션 | Option | 같은 상품을 살 때 고를 수 있는 구성. 예를 들어 운동화 한 상품에서 ‘270mm’를 고를 수 있다. |
| 재고 | Stock | 한 옵션에서 앞으로 주문할 수 있는 개수. 그 옵션을 2개 주문하면 재고도 2개 줄어든다. |
| 위시(찜) | Wish | 회원이 나중에 다시 보려고 저장한 상품 한 건. 같은 상품을 두 번 찜해도 목록에는 한 번만 나온다. |
| 주문 | Order | 회원이 원하는 옵션과 수량을 골라 포인트로 구매한 기록. 어떤 옵션을 몇 개, 언제 주문했는지 확인할 수 있다. |
| 주문 수량 | Order Quantity | 한 번의 주문에서 구매한 개수. 감귤 한 상자를 파는 상품을 2개 주문하면 주문 수량은 2다. |
| 상품 가격 | Unit Price | 상품 한 개의 가격. ‘제주 감귤 5kg’이 25,000원이면 한 상자의 가격이 25,000원이다. |
| 주문 금액 | Order Total | 한 주문에 쓰는 포인트를 계산하는 기준 금액. 25,000원짜리 상품 2개를 주문하면 총 50,000원이다. |
| 포인트 | Point | 계정에 충전해 두고 주문에 쓰는 잔액. 75,000포인트에서 50,000원어치를 구매하면 25,000포인트가 남는다. |
| 주문 메시지 | Order Message | 주문하는 사람이 선택해서 남기는 짧은 글. 예를 들어 ‘생일 축하해요’라고 적을 수 있다. |
| 카카오톡 주문 알림 | KakaoTalk Order Notification | 카카오로 로그인한 구매자 본인의 대화방에 보내는 주문 요약. 상품·옵션·수량·금액이 담기고, 구매자가 글을 남겼으면 그 글도 함께 보낸다. |
| 비회원 | Guest | 현재 이 서비스에 로그인하지 않은 사람. 이미 계정이 있어도 로그인하기 전에는 비회원이다. |
| 회원 | Member | 현재 이 서비스에 로그인한 사람. 자기 계정으로 상품을 찜하고 주문할 수 있다. |
| 사용자 | User | 회원과 비회원을 함께 가리키는 말. 상품 보기처럼 양쪽 모두 이용할 수 있는 기능의 주체다. |
| 계정 | Account | 이메일, 포인트처럼 한 사람에게 연결해 보관하는 정보. 로그인하지 않은 동안에도 남아 있다. |
| 서비스 로그인 토큰 | Service Login Token (JWT) | 가입이나 로그인에 성공했을 때 이 서비스가 발급하는 확인표. 찜과 주문을 요청하면 이 표로 누구의 목록인지 확인한다. |
| 카카오 액세스 토큰 | Kakao Access Token | 카카오가 발급하는 이용 허가증. 서비스가 동의받은 카카오 계정 정보를 확인하거나 구매자 본인에게 카카오톡 알림을 보낼 때 쓴다. |

## 같은 개념을 다르게 부르는 곳

| 가리키는 것 | 실제로 쓰인 이름 | 어디에서 확인할 수 있나 |
| --- | --- | --- |
| 관심 상품 저장 | 이 사전의 **위시**, 사용자 스토리의 **찜**, 코드의 `Wish`와 `addWish`, 요청 주소의 `/api/wishes`는 모두 상품을 내 목록에 저장하는 기능을 가리킨다. | [찜 기능](../src/main/java/gift/wish/WishController.java#L52) · [사용자 스토리](user-stories.md) |
| 상품 이름 | 상품과 찜 응답에서는 `name`, 관리자 입력란에서는 **상품명**, 입력 오류에서는 **상품 이름**이라고 한다. 찜 응답의 이름도 저장한 상품에서 가져온다. | [찜 응답](../src/main/java/gift/wish/WishResponse.java#L14) · [상품 입력란](../src/main/resources/templates/product/new.html#L19) · [오류 문구](../src/main/java/gift/product/ProductNameValidator.java#L23) |
| 옵션 이름 | 옵션 응답의 `name`을 이름 검사에서는 **옵션 이름**, 같은 이름을 다시 등록했을 때는 **옵션명**이라고 부른다. | [옵션 응답](../src/main/java/gift/option/OptionResponse.java#L5) · [이름 검사](../src/main/java/gift/option/OptionNameValidator.java#L25) · [중복 안내](../src/main/java/gift/option/OptionController.java#L60) |
| 옵션별 재고 | 옵션을 조회하면 `quantity`로 표시한다. 재고를 줄이는 코드 이름은 `subtractQuantity`이고, 수량이 부족할 때의 문구는 **“차감할 수량이 현재 재고보다 많습니다.”**이다. | [옵션 응답](../src/main/java/gift/option/OptionResponse.java#L6) · [재고 차감과 문구](../src/main/java/gift/option/Option.java#L39) |
| 카카오가 발급한 이용 허가증 | 카카오 응답은 `access_token`, 이를 읽는 코드는 `accessToken`, 서비스 계정에 보관할 때는 `kakaoAccessToken`이라고 쓴다. 이름은 세 가지지만 카카오가 발급한 같은 값이다. | [카카오 응답 읽기](../src/main/java/gift/auth/KakaoLoginClient.java#L44) · [계정에 저장](../src/main/java/gift/auth/KakaoAuthController.java#L63) |

## 같은 이름이 두 가지 이상 뜻으로 쓰이는 곳

| 겹치는 이름 | 문맥에 따른 뜻 | 실제로 구별하는 방법 |
| --- | --- | --- |
| 회원 / Member | 사용자 스토리에서는 **지금 로그인한 사람**이다. 관리자 화면의 ‘회원 관리’와 코드의 `Member`는 **등록된 계정**을 가리킨다. | 관리자 목록에 있다고 해서 지금 로그인 중이라는 뜻은 아니다. [회원 목록을 불러오는 코드](../src/main/java/gift/member/AdminMemberController.java#L28) · [사용자 스토리](user-stories.md) |
| 수량 / `quantity` | 옵션 응답에서는 **남은 재고**, 주문 응답과 카카오 알림에서는 **이번에 산 개수**다. | 재고 50개 중 2개를 주문한 뒤 옵션 응답의 `quantity`는 48, 주문 응답의 `quantity`는 2였다. [주문 전후 재고와 주문 내역](user-stories.md#상품을-고르고-주문하는-사람) · [알림의 수량](../src/main/java/gift/order/KakaoMessageClient.java#L46) |
| 가격 / `price` | 상품·찜 응답의 `price`는 **상품 한 개 가격**이다. 주문 처리 코드 안의 `price`는 **한 개 가격에 주문 수량을 곱한 합계**다. | 감귤 2개를 살 때 상품 응답에는 25,000이 오고 포인트 차감에는 50,000이 쓰인다. 주문 응답에는 금액 항목이 없다. [상품 응답](../src/main/java/gift/product/ProductResponse.java#L6) · [차감 금액 계산](../src/main/java/gift/order/OrderController.java#L91) · [주문 응답](../src/main/java/gift/order/OrderResponse.java#L5) |
| 메시지 / Message | 주문의 `message`는 **구매자가 쓴 한마디**다. 카카오 ‘메시지 보내기’에서의 메시지는 **서비스가 작성한 주문 알림 전체**다. | ‘생일 축하해요’는 구매자의 글이다. 카카오 알림에는 상품과 금액도 들어가고, 작성한 글이 있으면 💌 뒤에 붙는다. [주문 응답](../src/main/java/gift/order/OrderResponse.java#L10) · [알림 조립](../src/main/java/gift/order/KakaoMessageClient.java#L31) |
| 토큰 / Token | **서비스 로그인 토큰**과 **카카오 액세스 토큰**을 모두 토큰이라고 부른다. | 카카오 로그인 한 번에도 둘 다 등장한다. 앱은 카카오가 준 값을 계정에 저장한 뒤, 찜·주문에 쓸 서비스 토큰을 새로 발급해 `token`으로 돌려준다. [카카오 로그인 처리](../src/main/java/gift/auth/KakaoAuthController.java#L55) · [로그인 응답](../src/main/java/gift/auth/TokenResponse.java#L9) |
| 번호 / `id` | 상품 응답의 `id`는 **상품 번호**, 찜 응답의 `id`는 **저장한 찜 한 건의 번호**다. | 찜 응답에는 `id`와 `productId`가 함께 온다. 찜 해제에는 `id`를, 상품 상세 조회에는 `productId`를 쓴다. [찜 응답](../src/main/java/gift/wish/WishResponse.java#L11) · [찜 해제](../src/main/java/gift/wish/WishController.java#L79) |
| 이름 / `name` | 상품·찜 응답에서는 **상품명**, 옵션 응답에서는 **옵션명**, 카테고리 응답에서는 **분류 이름**이다. | 감귤 예시에서 상품명은 ‘제주 감귤 5kg’, 옵션명은 ‘일반 감귤’, 카테고리명은 ‘식품’이다. [상품 응답](../src/main/java/gift/product/ProductResponse.java#L5) · [옵션 응답](../src/main/java/gift/option/OptionResponse.java#L5) · [카테고리 응답](../src/main/java/gift/category/CategoryResponse.java#L5) |
| 이미지 주소 / `imageUrl` | 카테고리 응답에서는 **분류를 대표하는 그림**, 상품·찜 응답에서는 **개별 상품의 사진**을 가리킨다. | 같은 식품 카테고리에서도 감귤을 찜하면 찜 목록에 들어가는 주소는 감귤 상품의 사진 주소다. [카테고리 이미지](../src/main/java/gift/category/CategoryResponse.java#L15) · [찜 이미지](../src/main/java/gift/wish/WishResponse.java#L16) |
| 변동량 / `amount` | 포인트 충전·차감에서는 **더하거나 뺄 포인트**, 옵션의 재고 차감에서는 **뺄 상품 개수**다. | 구매 한 번에도 단위가 다르다. 감귤 2개를 사면 재고에서 2개를 빼고, 잔액에서 50,000포인트를 뺀다. [재고 계산](../src/main/java/gift/option/Option.java#L39) · [포인트 계산](../src/main/java/gift/member/Member.java#L57) |

## 같은 기능을 코드·응답·메시지에서 대조한 사례

| 기능 | 코드에서 부르는 이름 | 요청한 사람이 받는 응답 | 화면이나 메시지에 쓰인 표현 |
| --- | --- | --- | --- |
| 계정 만들기 | 직접 가입은 `register`, 관리자의 계정 추가는 `create` | 직접 가입하면 `201`과 로그인용 `token`을 받는다. 관리자가 추가하면 `302`로 회원 목록으로 이동한다. | 관리자 폼 제목은 **회원 추가**, 버튼은 **추가**다. 같은 이메일을 다시 쓰면 두 경로 모두 `Email is already registered.`라고 안내한다. [가입 처리](../src/main/java/gift/member/MemberController.java#L33) · [관리자 처리](../src/main/java/gift/member/AdminMemberController.java#L39) · [회원 추가 화면](../src/main/resources/templates/member/new.html#L11) |
| 상품 등록 | 상품 API는 `createProduct`, 관리자 폼은 `create` | API는 `201`과 새 상품 정보(`id`, `name` 등)를 돌려준다. 관리자 폼은 `302`로 상품 목록으로 이동한다. | 관리자 화면의 제목은 **상품 추가**, 제출 버튼은 **추가**다. [API 처리](../src/main/java/gift/product/ProductController.java#L49) · [상품 추가 화면](../src/main/resources/templates/product/new.html#L11) · [관리자 폼 제출 결과](user-stories.md#관리자-화면에서-상품과-계정을-관리하는-사람) |
| 옵션 이름 등록과 중복 확인 | `createOption`이 같은 상품에 같은 이름이 있는지 `existsByProductIdAndName`으로 확인한다. | 새 이름이면 `201` 응답의 `name`에 담긴다. 같은 이름을 다시 보내면 `400`과 안내 문장이 온다. | 입력 규칙에는 **“옵션 이름”**, 중복 안내에는 **“이미 존재하는 옵션명입니다.”**라고 쓰여 있다. [등록과 중복 안내](../src/main/java/gift/option/OptionController.java#L59) · [입력 규칙 문구](../src/main/java/gift/option/OptionNameValidator.java#L25) |
| 포인트 충전 | `chargePoint(amount)`에서 `amount`는 이번에 더할 양, `point`는 계정 잔액이다. | 관리자 폼은 `302`로 회원 목록으로 이동한다. 목록에서 충전 후 잔액을 확인한다. | 목록의 열 제목은 **포인트 충전**, 버튼은 **충전**, 결과가 표시되는 열은 **포인트**다. [충전 계산](../src/main/java/gift/member/Member.java#L49) · [폼 처리](../src/main/java/gift/member/AdminMemberController.java#L75) · [목록 화면](../src/main/resources/templates/member/list.html#L18) |
| 주문 후 카카오톡 알림 | `createOrder`가 주문을 저장한 뒤 `sendToMe`로 구매자 본인에게 알림 전송을 시도한다. | `201`과 주문 번호·옵션 번호·구매 수량·주문 시각·구매자의 메시지가 온다. `201`은 주문이 생성됐다는 뜻이다. | 카카오 문구는 **“🎁 선물이 도착했어요!”**, 버튼은 **“선물 확인하기”**다. 여기서 ‘도착’은 구매자 본인의 대화방에 주문 내용을 알릴 때 쓰는 표현이다. [주문 처리](../src/main/java/gift/order/OrderController.java#L95) · [나에게 보내기](../src/main/java/gift/order/KakaoMessageClient.java#L16) · [알림 문구](../src/main/java/gift/order/KakaoMessageClient.java#L39) |
| 주문 금액 안내 | 주문 계산에서는 `price`, 카카오 알림을 만들 때는 `totalPrice`라고 부른다. | 주문 응답에 포함되는 숫자는 주문 번호·옵션 번호·수량이다. 총액은 응답 본문에 따로 실리지 않는다. | 같은 합계가 카카오 알림에서는 **금액**으로 표시되고 숫자 뒤에 **원**이 붙는다. [주문 계산](../src/main/java/gift/order/OrderController.java#L91) · [주문 응답](../src/main/java/gift/order/OrderResponse.java#L5) · [금액 표시](../src/main/java/gift/order/KakaoMessageClient.java#L32) |

카카오 알림 문구는 프로젝트의 메시지 양식과 이전 실행에서 카카오를 대신한 로컬 서버가 받은 요청을 기준으로 적었다. [카카오 연동 검증 기록](user-stories.md#상품을-고르고-주문하는-사람)

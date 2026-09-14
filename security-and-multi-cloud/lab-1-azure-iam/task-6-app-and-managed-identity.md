# Task 6 - 앱 등록 · 서비스 주체 · 관리 ID

사람이 아닌 **프로그램**도 Azure에 로그인해야 합니다. 그 신원을 만드는 두 가지 방법을 확인합니다.

## 6-1. 앱 등록과 서비스 주체

**앱 등록(Application)** 은 애플리케이션의 **설계도**로 홈 테넌트에 딱 하나 만듭니다. **서비스 주체(Service Principal)** 는 그 설계도로 특정 테넌트에 실제로 만들어진 **'앱의 계정'** 이고, **역할 할당·관리자 동의가 붙는 대상**이 바로 이것입니다.

1. Entra ID → \[앱 등록] → \[새 등록].

<figure><img src="../../.gitbook/assets/image (1042).png" alt="" width="563"><figcaption></figcaption></figure>



2. 이름 `app-iam-demo-user**`, 지원 계정 유형은 **이 조직 디렉터리의 계정만** 선택 → 등록.

<figure><img src="../../.gitbook/assets/image (1043).png" alt="" width="542"><figcaption></figcaption></figure>



3. \[개요] 에서 **애플리케이션(클라이언트) ID** 와 **디렉터리(테넌트) ID** 를 확인합니다.

<figure><img src="../../.gitbook/assets/image (1044).png" alt=""><figcaption></figcaption></figure>



2. 개요 화면의 **'관리되는 애플리케이션'** 링크를 클릭합니다 → **엔터프라이즈 애플리케이션** 목록의 항목으로 이동합니다. **이것이 서비스 주체**입니다.

<figure><img src="../../.gitbook/assets/image (1045).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1046).png" alt=""><figcaption></figcaption></figure>

2. Entra ID → \[엔터프라이즈 애플리케이션] 목록에서도 `app-iam-demo-user**` 이 보이는지 확인합니다.

> 💡 **앱 등록 = 설계도, 엔터프라이즈 애플리케이션 = 그 설계도로 찍어낸 계정.** 포털에서 두 메뉴에 같은 이름이 보이는 이유가 이것입니다.

<figure><img src="../../.gitbook/assets/image (1047).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1048).png" alt=""><figcaption></figcaption></figure>

### 인증 방법의 안전 순서

1. Entra ID → \[앱 등록] -> 해당 App -> \[인증서 및 비밀] 메뉴를 열어 **클라이언트 비밀 / 인증서 / 연합 자격 증명** 세 탭이 있는 것만 확인합니다.

<figure><img src="../../.gitbook/assets/image (1049).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1050).png" alt=""><figcaption></figcaption></figure>



> ⚠️ **안전 순서: 클라이언트 시크릿(암호 — 만료·유출 위험) < 인증서 < 연합 자격 증명(시크릿 없이 토큰 교환).** 실습에서는 시크릿을 만들지 않습니다. 만들었다면 **반드시 메모장이 아니라 Key Vault에 보관**하고, 실습 후 삭제하세요.

##

##

## 6-2. 서비스 주체에 RBAC 역할 주기

1. `rg-iam-user**` → \[액세스 제어(IAM)] → \[역할 할당 추가]

<figure><img src="../../.gitbook/assets/image (1051).png" alt="" width="527"><figcaption></figcaption></figure>





2. 역할 **독자**, 구성원 할당 대상에서 **사용자, 그룹 또는 서비스 주체** 를 고르고 `app-iam-demo-user**` 을 선택합니다.

<figure><img src="../../.gitbook/assets/image (1052).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1053).png" alt=""><figcaption></figcaption></figure>



2. 역할 할당 목록에 **서비스 주체**로 표시되는 것을 확인합니다.

> 💡 Task 3의 「① 누구에게」에는 사용자·그룹뿐 아니라 **서비스 주체와 관리 ID**도 들어갈 수 있습니다. 같은 3요소 구조가 그대로 적용됩니다.

<figure><img src="../../.gitbook/assets/image (1054).png" alt=""><figcaption></figcaption></figure>

##

* [ ] 앱 등록을 만들고 대응하는 **엔터프라이즈 애플리케이션(서비스 주체)** 을 찾았는가
* [ ] 서비스 주체에 RBAC 역할을 할당했는가





## 실습 정리

이 Lab에서 만든 리소스는 이후 Lab에서 쓰지 않습니다. 과정 종료 후 아래를 정리하세요.

* 역할 할당 제거 → 스토리지 계정 → `rg-iam-user**` 삭제
  * app-iam-demo-user00
  * sec-dev-user00
  * sec-view-user00
* 앱 등록 `app-iam-demo-user**` 삭제
* 사용자 `dev-user**`·`view-user**`, 그룹 `sec-dev-user**`·`sec-view-user**` 삭제

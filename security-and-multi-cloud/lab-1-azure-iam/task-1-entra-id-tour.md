# Task 1 - Entra ID 테넌트 둘러보기

Microsoft Entra ID는 구 **Azure Active Directory(Azure AD)** 입니다 (2023년 개명). 현장에서는 아직 옛 이름을 더 많이 쓰고, 일부 API·SDK에도 옛 이름이 남아 있으니 **두 이름이 같은 것**임을 기억하세요.

## 1-1. 테넌트 정보 확인

1. 포털 상단 검색창에 `Microsoft Entra ID` 입력 → 서비스로 이동.

<figure><img src="../../.gitbook/assets/image (32).png" alt="" width="458"><figcaption></figcaption></figure>



2. \[개요] 에서 다음을 확인하고 메모합니다.

* **테넌트 이름** 과 **테넌트 ID**
* **기본 도메인** (`xxxx.onmicrosoft.com`)
* **라이선스** 등급 (Free / P1 / P2)

<figure><img src="../../.gitbook/assets/image (82).png" alt=""><figcaption></figcaption></figure>



3. 왼쪽 메뉴에서 **사용자 · 그룹 · 앱 등록 · 엔터프라이즈 애플리케이션** 메뉴의 위치만 확인합니다.

<figure><img src="../../.gitbook/assets/image (209).png" alt="" width="498"><figcaption></figcaption></figure>

##





## 1-2. 테넌트와 구독의 관계 확인

1. 포털에서 `구독(Subscriptions)` 으로 이동 → 실습용 구독 클릭

<figure><img src="../../.gitbook/assets/image (261).png" alt="" width="509"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (271).png" alt=""><figcaption></figcaption></figure>

2. \[개요] 에서 **디렉터리(Directory)** 항목이 1-1에서 본 테넌트를 가리키는지 확인합니다.

<figure><img src="../../.gitbook/assets/image (337).png" alt=""><figcaption></figcaption></figure>

> 💡 **AWS와 가장 다른 지점입니다.** AWS는 자격 증명이 계정(Account)에 내장되어 있지만, Azure는 **테넌트(디렉터리)에 분리**되어 있고 여러 구독이 그 테넌트를 **공유**합니다. 구독을 지워도 사용자 계정은 테넌트에 그대로 남습니다.

<table><thead><tr><th width="179.272705078125">개념</th><th width="233.54541015625">AWS</th><th>Azure</th></tr></thead><tbody><tr><td>자격 증명이 사는 곳</td><td>계정(Account)에 내장</td><td>테넌트(Tenant)에 분리 — 구독들이 공유</td></tr><tr><td>리소스 권한 부여</td><td>정책(JSON)을 주체에 연결</td><td>역할(Role)을 범위(Scope)에 할당</td></tr><tr><td>서비스·앱 자격 증명</td><td>IAM Role + STS</td><td>서비스 주체 + 관리 ID</td></tr><tr><td>최상위 신원</td><td>루트 사용자</td><td>전역 관리자(Global Administrator)</td></tr></tbody></table>



##

## 1-3. 라이선스 계층 확인 — 기능 게이트

Entra ID는 **라이선스 등급에 따라 메뉴 자체가 열리고 닫힙니다.** 이걸 모르면 "왜 우리는 조건부 액세스 메뉴가 없지?" 하고 헤매게 됩니다.

1. Entra ID → \[보안] → **조건부 액세스** 메뉴를 열어봅니다.

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>





2. 라이선스가 Free라면 기능이 잠겨 있거나 평가판 안내가 뜨는 것을 확인합니다.

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

| 계층          | 대표 기능                                  |
| ----------- | -------------------------------------- |
| Free        | 사용자·그룹 관리, 기본 SSO, 보안 기본값, 셀프서비스 암호 변경 |
| Entra ID P1 | 조건부 액세스, 동적 그룹, 그룹 기반 라이선싱, SSPR       |
| Entra ID P2 | PIM(시간제한 권한), ID 보호(위험 기반), 액세스 검토     |

* [ ] 테넌트 ID와 기본 도메인을 메모했는가
* [ ] 구독의 디렉터리가 그 테넌트를 가리키는가
* [ ] 우리 테넌트의 라이선스 등급을 확인했는가

> 💡 **Entra ID ≠ 온프레미스 AD.** 이름은 비슷하지만 완전히 다른 제품입니다. AD DS는 사내 도메인 컨트롤러에서 **LDAP·Kerberos**로 사내 PC·파일 공유를 인증하고, Entra ID는 Microsoft 클라우드에서 **OAuth·OIDC·SAML**로 웹·모바일·SaaS를 인증합니다. 도메인 컨트롤러를 클라우드에 올린 것이 아닙니다.


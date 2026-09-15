# Task 2 - 사용자와 그룹 만들기

**사용자**는 사람 계정, **그룹**은 그 사람들을 묶는 상자입니다. 권한은 **개인이 아니라 그룹에 주는 것이 원칙**입니다.

## 2-1. 멤버 사용자 2명 만들기

1. Entra ID → \[사용자] → \[새 사용자] → **새 사용자 만들기**.

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (434).png" alt="" width="563"><figcaption></figcaption></figure>





2. 아래 두 계정을 만듭니다.

<table><thead><tr><th width="131.09088134765625">표시 이름</th><th width="367.8179931640625">사용자 계정 이름(UPN)</th><th>암호</th><th>용도</th></tr></thead><tbody><tr><td>dev-user**</td><td>dev-user**@테넌트도메인.onmicrosoft.com</td><td>lbit123!</td><td>Task 3~4에서 기여자 역할 부여</td></tr><tr><td>view-user**</td><td>view-user**@테넌트도메인.onmicrosoft.com</td><td>lbit123!</td><td>Task 4~5에서 읽기 권한자 역할 부여</td></tr></tbody></table>

{% columns %}
{% column %}
<figure><img src="../../.gitbook/assets/image (447).png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}

{% column %}
<figure><img src="../../.gitbook/assets/image (491).png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}
{% endcolumns %}





3. 만든 사용자를 클릭해 **개체 ID**와 **사용자 유형(멤버)** 을 확인합니다.

> 💡 **멤버 사용자 vs 게스트 사용자** — 멤버는 우리 조직 소속 직원 계정입니다. 게스트는 협력사 등 외부인을 초대한 계정(B2B)으로, 로그인은 **상대 회사 테넌트에서** 처리되고 UPN에 `#EXT#` 가 붙습니다.
>
> 💡 **동기화 계정과 클라우드 전용 계정** — 실무 대부분은 사내 AD 계정을 `Entra Connect Sync` 로 복제해 씁니다. 그렇게 온 계정에는 `onPremisesSyncEnabled = true` 가 붙고, 포털에서 일부 항목을 수정할 수 없습니다. 지금 만든 것은 **클라우드 전용** 계정입니다.

<figure><img src="../../.gitbook/assets/image (560).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (561).png" alt=""><figcaption></figcaption></figure>

##

##

## 2-2. (선택) 게스트 사용자 초대해 보기

1. \[사용자] → \[새 사용자] → **외부 사용자 초대**.

<figure><img src="../../.gitbook/assets/image (736).png" alt=""><figcaption></figcaption></figure>



2. 본인의 다른 메일 주소를 넣고 초대합니다.

<figure><img src="../../.gitbook/assets/image (737).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (817).png" alt=""><figcaption></figcaption></figure>





3. 목록에서 **사용자 유형이 '게스트'**, UPN에 `#EXT#` 가 붙은 것을 확인합니다.

<figure><img src="../../.gitbook/assets/image (1011).png" alt=""><figcaption></figcaption></figure>

##

##

## 2-3. 보안 그룹 만들고 멤버 추가

1. Entra ID → \[그룹] → \[새 그룹].

<figure><img src="../../.gitbook/assets/image (1012).png" alt="" width="329"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1013).png" alt="" width="563"><figcaption></figcaption></figure>





2. 아래와 같이 입력하고 만듭니다.

| 항목       | 값                |
| -------- | ---------------- |
| 그룹 유형    | 보안(Security)     |
| 그룹 이름    | sec-dev-user\*\* |
| 멤버 자격 유형 | 할당됨(Assigned)    |
| 멤버       | dev-user\*\*     |

<figure><img src="../../.gitbook/assets/image (1014).png" alt=""><figcaption></figcaption></figure>





3. 같은 방법으로 `sec-view-user**` 그룹을 만들고 `view-user**` 을 멤버로 넣습니다.

> 💡 **보안 그룹은 AWS의 IAM 그룹에 해당**합니다 — 권한·앱 접근·라이선스를 한꺼번에 부여하는 상자입니다.

* [ ] 멤버 사용자 2명이 생성되었는가
* [ ] 보안 그룹 2개가 만들어지고 각각 멤버가 들어갔는가
* [ ] (선택) 게스트 계정 UPN에 `#EXT#` 가 붙은 것을 확인했는가


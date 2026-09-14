# Task 4 - 역할 차이와 상속 검증

Task 3에서 준 권한이 **실제로 어떻게 동작하는지** 직접 로그인해서 확인합니다.

## 4-1. 내 액세스 확인

1. `rg-iam-user**` → \[액세스 제어(IAM)] → **\[내 액세스 확인]** 탭.
2. 드롭다운에서 `dev-user**`, `view-user**` 을 각각 골라 어떤 역할이 보이는지 확인합니다.
3. **\[역할 할당]** 탭에서 각 할당의 **'범위' 열**을 봅니다 — `이 리소스` 와 `상속됨(구독)` 이 구분되어 표시됩니다.





## 4-2. 읽기 권한자로 로그인해 보기

1. 브라우저 **시크릿 창(또는 다른 종류의 브라우저)**&#xC5D0;서 `https://portal.azure.com` 접속 → `view-user**` 으로 로그인 (첫 로그인 시 암호 변경 -> 암호에  ! 추가)

<figure><img src="../../.gitbook/assets/image (1016).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
* 계정은 이메일 형식으로 로그인하여야합니다.&#x20;
* 로그인 시 MFA 등록이 반복됩니다.
{% endhint %}



2. `rg-iam-user**` 이 보이는지 확인합니다.

<figure><img src="../../.gitbook/assets/image (1017).png" alt=""><figcaption></figcaption></figure>







3. Vnet 을 해당 리소스그룹에 만들어 봅니다 → **권한 오류로 차단**되는 것을 확인합니다.

<figure><img src="../../.gitbook/assets/image (1020).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1021).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1022).png" alt="" width="319"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1023).png" alt=""><figcaption></figcaption></figure>





4. \[액세스 제어(IAM)] → \[추가] 버튼이 **비활성**인 것을 확인합니다.

<figure><img src="../../.gitbook/assets/image (1024).png" alt="" width="563"><figcaption></figcaption></figure>



## 4-3. 기여자로 로그인해 보기

1. 시크릿 창(또는 다른 브라우저) 에서 `dev-user**` 으로 로그인

<figure><img src="../../.gitbook/assets/image (1025).png" alt=""><figcaption></figcaption></figure>



2. Vnet 을 해당 리소스그룹에 만들어 봅니다 → **성공**합니다.

<figure><img src="../../.gitbook/assets/image (1026).png" alt=""><figcaption></figcaption></figure>



3. \[액세스 제어(IAM)] → \[역할 할당 추가] 를 시도합니다 → **비활성 / 거부**됩니다.

<figure><img src="../../.gitbook/assets/image (1027).png" alt=""><figcaption></figcaption></figure>

> ❗ **소유자와 기여자를 가르는 결정적 차이가 여기입니다.** 기여자는 리소스를 다 만들고 바꾸지만 **권한은 못 줍니다.** 권한 위임까지 하려면 소유자 또는 사용자 액세스 관리자가 필요합니다.



* [ ] 읽기 권한자로 리소스 생성이 차단되는가
* [ ] 기여자로 리소스 생성은 되지만 **역할 할당은 안 되는가**


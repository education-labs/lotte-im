# Task 3 - 경로 테이블 생성과 연결

**이 Lab의 핵심 단계입니다.**

1. "경로 테이블"을 검색해 \[만들기]

<figure><img src="../../.gitbook/assets/image (1104).png" alt="" width="563"><figcaption></figcaption></figure>



1. 리소스 그룹 : rg-user\*\* 이름 : `rt-spoke-user**`, 게이트웨이 경로 전파 : **사용 안 함**

<figure><img src="../../.gitbook/assets/image (1106).png" alt="" width="453"><figcaption></figcaption></figure>



2. 생성 완료 되면, \[경로] → \[추가]

* 경로 이름 : `udr-default`,&#x20;
* 대상유형 :  IP주소&#x20;
* 대상 IP 주소 : 0.0.0.0/0
* 다음 홉 형식 : **가상 어플라이언스**
* 다음 홉 주소 : Task 1에서 메모한 방화벽 사설 IP (10.\*\*.0.4)

<figure><img src="../../.gitbook/assets/image (1107).png" alt=""><figcaption></figcaption></figure>





3. ⭐ **SSH 예외 경로를 하나 더 추가합니다** — 아래 ⚠️를 먼저 읽으세요. \[경로] → \[추가] → 이름 `udr-ssh-exception`, 대상 IP  주소 `내 공인 IP/32`, 다음 홉 유형 **인터넷**.

{% hint style="info" %}
내 공인 IP는 [whatismyip.com](https://www.whatismyip.com/) 에서 확인합니다 (예: `1.2.3.4` → `1.2.3.4/32`).
{% endhint %}

<figure><img src="../../.gitbook/assets/image (1108).png" alt=""><figcaption></figcaption></figure>

> ⚠️ **이 예외 경로를 빼면 Task 5와 Lab 6의 SSH가 끊깁니다.**
>
> VM1은 공인 IP를 가지고 `snet-web`에 있습니다. `0.0.0.0/0 → 방화벽` UDR을 걸면 들어오는 SSH는 공인 IP로 직행하지만 **나가는 응답은 방화벽을 거쳐 다른 IP로 나갑니다.** 내 PC는 보낸 곳과 다른 주소에서 온 응답을 버리므로 접속이 안 됩니다. 이것이 **비대칭 라우팅**입니다.
>
> `내IP/32 → 인터넷` 경로를 넣으면 내 PC로 가는 응답만 방화벽을 우회해 경로가 대칭이 됩니다.
>
> 💡 **수강생마다 공인 IP가 다릅니다.** 같은 사무실에서 나가면 대개 하나지만, 다르면 각자 추가하거나 공통 출구 대역을 `/24`로 묶어 넣으세요.



* [ ] 경로 테이블이 3개 서브넷에 연결되었는가
* [ ] 경로 전파가 "사용 안 함"인가
* [ ] 다음 홉이 사설 IP인가
* [ ] ⭐ **SSH 예외 경로(내IP/32 → 인터넷)를 추가했는가**


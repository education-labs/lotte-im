# Task 2 - AWS Site-to-Site VPN 연결 생성

> 🟧 **계속 AWS 콘솔**입니다.

1. VPC → \[Site-to-Site VPN 연결] → \[VPN 연결 생성]

<figure><img src="../../.gitbook/assets/image (1132).png" alt=""><figcaption></figcaption></figure>





2.  이름 `vpn-to-azure-user**` 입력, 대상 게이트웨이 유형 **가상 프라이빗 게이트웨이** → `vgw-aws-user**` 선택.<br>

    <figure><img src="../../.gitbook/assets/image (1131).png" alt="" width="479"><figcaption></figcaption></figure>





2. 고객 게이트웨이 **기존** → `cgw-azure-user**` 선택.
3. 라우팅 옵션 **정적** 선택 → 정적 IP 접두사에 **`10.**.0.0/16`**(Azure 대역) 입력.

<figure><img src="../../.gitbook/assets/image (1134).png" alt="" width="339"><figcaption></figcaption></figure>



2. \[터널 옵션]을 펼칩니다. 터널 1의 **사전 공유 키를 직접 지정**합니다 (영숫자 8자 이상, 아래④번 값으로 메모)\
   \
   \[VPN 연결 생성] 클릭.

<figure><img src="../../.gitbook/assets/image (1135).png" alt="" width="425"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1136).png" alt=""><figcaption></figcaption></figure>

2. 생성 후 \[터널] 탭에서 **터널 1의 외부 IP 주소를 메모**합니다 (③번 값).

<figure><img src="../../.gitbook/assets/image (1138).png" alt=""><figcaption></figcaption></figure>



3. 이 시점의 터널 상태가 **Down(아래로)** 인 것을 확인합니다.

<figure><img src="../../.gitbook/assets/image (1139).png" alt=""><figcaption></figcaption></figure>

| 항목               | 값              | 비고         |
| ---------------- | -------------- | ---------- |
| 라우팅              | 정적             | BGP 미사용    |
| 정적 IP 접두사        | 10.\*\*.0.0/16 | Azure 쪽 대역 |
| 터널 1 PSK(사전 공유키) | awstoazure     | ④ 메모 ★     |
| 확인할 값            | 터널 1 외부 IP     | ③ 메모 ★     |

> ⚠️ **PSK를 직접 지정하세요.** 자동 생성된 키는 특수문자가 섞여 Azure에서 거부되거나 복사 중 오류가 나기 쉽습니다. 영숫자만으로 8자 이상 지어서 메모장에 적어두세요.
>
> ⚠️ **지금 터널 상태가 Down인 것이 정상입니다.** Azure 쪽 설정이 아직 없으니까요. Task 3을 마치면 UP으로 바뀝니다.
>
> 💡 **AWS는 터널을 항상 2개 줍니다.** 실습에서는 시간 관계상 터널 1개만 구성하지만, 운영에서는 두 개를 다 씁니다. AWS가 유지 관리로 한 터널을 내리는 날, 하나만 쓰고 있으면 그대로 장애가 됩니다.

* [ ] VPN 연결이 생성되었는가
* [ ] **터널 1 외부 IP를 메모(③)** 했는가
* [ ] **PSK를 메모(④)** 했는가
* [ ] 터널 상태가 Down인 것을 확인했는가 (이 시점엔 정상)

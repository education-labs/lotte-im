# Task 4 - VM 2대 생성

1. "가상 머신"을 검색해 \[만들기] → \[Azure 가상 머신]

<figure><img src="../../.gitbook/assets/image (1074).png" alt=""><figcaption></figcaption></figure>



2. 아래와 같이 입력한 뒤 다음을 클릭합니다.

* 리소스 그룹 : rg-user\*\*
* 이름 : vm1-user\*\*

<figure><img src="../../.gitbook/assets/image (1075).png" alt=""><figcaption></figcaption></figure>

* 이미지 : Ubuntu Server 24.04 LTS
* 크기 : Standard\_B2s (안 보인다면 모든 크기 보기에서 선택)

<figure><img src="../../.gitbook/assets/image (1076).png" alt=""><figcaption></figcaption></figure>

* 인증 유형 : 암호
* 사용자 이름 : azureuser
* 암호 : azureuser!234

<figure><img src="../../.gitbook/assets/image (1077).png" alt=""><figcaption></figcaption></figure>



3. 네트워킹 탭에서 vnet 과 subnet (`snet-web`) 을 선택한 뒤 검토 + 만들기 를 클릭합니다.

<figure><img src="../../.gitbook/assets/image (1078).png" alt=""><figcaption></figcaption></figure>





4. 아래 정보와 같이 2번 VM을 생성합니다.  &#x20;

<table><thead><tr><th width="180.81817626953125">항목</th><th>값</th></tr></thead><tbody><tr><td>리소스 그룹</td><td>rg-user**</td></tr><tr><td>가상 머신 이름</td><td>vm2-user**</td></tr><tr><td>이미지</td><td>Ubuntu Server 24.04 LTS</td></tr><tr><td>크기</td><td>Standard_B2s</td></tr><tr><td>인증</td><td>암호</td></tr><tr><td>사용자 이름 </td><td>azureuser</td></tr><tr><td>암호 </td><td>azureuser!234</td></tr><tr><td>네트워킹 -> 서브넷</td><td>snet-app</td></tr><tr><td>공인 IP ★</td><td>없음</td></tr></tbody></table>





5. **VM1의 공인 IP와 VM2의 사설 IP를 포털에서 확인해 메모합니다.** (각 VM → \[개요])

<figure><img src="../../.gitbook/assets/image (1079).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1080).png" alt=""><figcaption></figcaption></figure>





6. Windows Powershell 실행 한뒤 아래 명령 수행하여 VM으로 접속합니다.

```bash
ssh azureuser@<vm1 IP>
```

<figure><img src="../../.gitbook/assets/image (1081).png" alt=""><figcaption></figcaption></figure>



7. vm2의 사설 IP로 ping 테스트를 시도해봅니다.

```bash
ping -c 4 10.**.9.4
```

{% hint style="info" %}
보통 vm의 ip 의 할당순서는 4번부터 진행됩니다.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (1082).png" alt=""><figcaption></figcaption></figure>

> ⚠️ **ping이 안 되면** Task 3에서 각 NSG에 **ICMP 허용 규칙**을 넣었는지 확인하세요. 기본 규칙만으로는 VNet 내부 통신이 열려 있어도 ICMP가 막혀 있을 수 있습니다.

* [ ] VM 2대가 실행 중인가
* [ ] VM1에 SSH 접속이 되는가
* [ ] VM1 → VM2 ping이 통과하는가
* [ ] VM2에는 공인 IP가 없는가

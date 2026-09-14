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



3. 네트워킹 탭에서 vnet 과 subnet (`snet-web` ) 을 선택한 뒤 검토 + 만들기 를 클릭합니다.

<figure><img src="../../.gitbook/assets/image (1078).png" alt=""><figcaption></figcaption></figure>





4. 아래 정보와 같이 2번 VM을 생성합니다.  &#x20;

<table><thead><tr><th width="180.81817626953125">항목</th><th>값</th></tr></thead><tbody><tr><td>리소스 그룹</td><td>rg-user**</td></tr><tr><td>가상 머신 이름</td><td>vm2-user**</td></tr><tr><td>이미지</td><td>Ubuntu Server 24.04 LTS</td></tr><tr><td>크기</td><td>Standard_B2s</td></tr><tr><td>인증</td><td>암호</td></tr><tr><td>사용자 이름 </td><td>azureuser</td></tr><tr><td>암호 </td><td>azureuser!234</td></tr><tr><td>네트워킹 -> 서브넷</td><td>snet-app</td></tr></tbody></table>



4. 1번 VM에 SSH로 접속합니다.

**입력값**

7. **VM1의 공인 IP와 VM2의 사설 IP를 포털에서 확인해 메모합니다.** (각 VM → \[개요])

> 💡 `snet-app`(`10.**.9.0/24`)의 첫 VM이면 대개 **`10.**.9.4`** 가 할당됩니다. Azure가 각 서브넷의 앞 4개 주소(네트워크·게이트웨이·DNS×2)를 예약하기 때문입니다. **다를 수 있으니 아래 명령에는 실제로 확인한 IP를 넣으세요.**

```bash
chmod 400 <키파일>.pem
ssh-add <키파일>.pem                                  # 에이전트에 키 등록
ssh -A -i <키파일>.pem azureuser@<VM1 공인 IP>       # -A: 키 전달

# VM1 에서 VM2 로 통신 확인 (10.**.9.4 = 메모해 둔 VM2 사설 IP)
ping -c 4 10.**.9.4
ssh azureuser@10.**.9.4                              # -A 접속이어야 성공
```

> ⚠️ **ping이 안 되면** Task 3에서 각 NSG에 **ICMP 허용 규칙**을 넣었는지 확인하세요. 기본 규칙만으로는 VNet 내부 통신이 열려 있어도 ICMP가 막혀 있을 수 있습니다.

* [ ] VM 2대가 실행 중인가
* [ ] VM1에 SSH 접속이 되는가
* [ ] VM1 → VM2 ping이 통과하는가
* [ ] VM2에는 공인 IP가 없는가

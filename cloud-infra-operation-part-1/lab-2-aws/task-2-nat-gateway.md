# Task 2 - NAT Gateway



1. 좌측 NAT 게이트웨이 > NAT 게이트웨이 생성 클릭

<figure><img src="../../.gitbook/assets/image (396).png" alt=""><figcaption></figcaption></figure>



2. 아래와 같이 설정 후 탄력적 IP 할당 클릭

* 이름 : user\*\*-natgw
* 가용성 모드 : 영역별
* 서브넷 : user\*\*-subnet-1
* 연결 유형 : 퍼블릭&#x20;

<figure><img src="../../.gitbook/assets/image (397).png" alt=""><figcaption></figcaption></figure>





3. 탄력적 IP 할당 ID가 할당되면 NAT 게이트웨이 생성 클릭

<figure><img src="../../.gitbook/assets/image (398).png" alt=""><figcaption></figcaption></figure>





4. 상태가 Pending 에서 Available 로 변경될때까지 대기 (약 1\~2분 후 새로고침하여 확인)

<figure><img src="../../.gitbook/assets/image (399).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../.gitbook/assets/image (400).png" alt=""><figcaption></figcaption></figure>





5. 해당 화면에서 확인되는 네트워크 인터페이스 ID를 메모장에 저장

<figure><img src="../../.gitbook/assets/image (409).png" alt=""><figcaption></figcaption></figure>

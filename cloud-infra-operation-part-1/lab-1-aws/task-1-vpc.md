# Task 1 - VPC



1. VPC 서비스로 이동

<figure><img src="../../.gitbook/assets/image (450).png" alt=""><figcaption></figcaption></figure>





2. VPC > VPC 생성 클릭

<figure><img src="../../.gitbook/assets/image (451).png" alt=""><figcaption></figcaption></figure>





3. 아래와 같이 설정 후 VPC 생성 클릭



* 생성할 리소스 : VPC만
* 이름 태그 : user\*\*–vpc
* IPv4 CIDR 블록 : 수동입력 선택
* VPC IPv4 CIDR : 10.0.0.0/16으로 입력

<figure><img src="../../.gitbook/assets/image (452).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (453).png" alt=""><figcaption></figcaption></figure>



4. 생성 완료 안내를 확인하고 좌측 VPC 클릭하면, 전체 VPC 목록이 확인되는데, \
   필터링 검색창에 user\*\* 를 입력하여 본인이 생성한 VPC만 하단에 출력되게 설정

<figure><img src="../../.gitbook/assets/image (454).png" alt=""><figcaption></figcaption></figure>





5. VPC ID 값을 클릭하여 내 VPC 의 상세페이지로 이동

<figure><img src="../../.gitbook/assets/image (455).png" alt=""><figcaption></figcaption></figure>





6. 작업 > CIDR 편집 클릭

<figure><img src="../../.gitbook/assets/image (456).png" alt=""><figcaption></figcaption></figure>





7. 새 IPv4 CIDR 추가를 클릭하여 CIDR를 추가확장 가능 (추가는 하지 않고 취소)

<figure><img src="../../.gitbook/assets/image (458).png" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
다만 이때, 확장할수 있는 CIDR은 기존 VPC CIDR의 클래스와 같아야합니다.

10.0.0.0/16 이라면,&#x20;

`10.1.0.0/16` 또는 `10.2.0.0/16`
{% endhint %}







8. 다시 VPC 상세 페이지로 이동하여 작업 > VPC 설정 편집 클릭

<figure><img src="../../.gitbook/assets/image (459).png" alt=""><figcaption></figcaption></figure>





9. DNS 설정에서 DNS 호스트 이름 활성화를 체크한 후 저장 버튼을 클릭

{% hint style="info" %}
향후 VPC 내부에 생성될 EC2 인스턴스에 DNS 이름을 할당하고자 할 경우 필요한 옵션
{% endhint %}

<figure><img src="../../.gitbook/assets/image (460).png" alt=""><figcaption></figcaption></figure>

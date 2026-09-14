# Task 2. 태그를 명시한 EC2 인스턴스 생성

1\. ec2 서비스로 이동한 뒤, 인스턴스 생성 클릭

<figure><img src="../../.gitbook/assets/image (561).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (564).png" alt=""><figcaption></figcaption></figure>





2. 아래와 같이 이름을 입력하고 추가 태그 추가 클릭

* 이름 : prod-instance-user\*\*

<figure><img src="../../.gitbook/assets/image (565).png" alt="" width="563"><figcaption></figcaption></figure>



3. 키 : Env   값 : prod-user\*\*    을 입력

<figure><img src="../../.gitbook/assets/image (566).png" alt="" width="563"><figcaption></figcaption></figure>





4. 나머지는 아래와 같이 설정한 뒤 인스턴스 생성

* 애플리케이션 및 OS 이미지 : Amazon Linux
* Amazon Machine Image : Amazon Linux 2023 kernel-6.1 AMI
* 인스턴스 유형 : t3.small
* 키페어 생성 : user\*\*-key
* VPC : user\*\*-vpc
* 서브넷 : user\*\*-pub-subnet1
* 퍼블릭 IP 자동할당 : 활성화
* 방화벽(보안그룹) : 보안 그룹 생성
* 보안그룹 이름 : user\*\*-tagec2-sg



5. 아래설정과 같이 구성하여 두 번째 인스턴스 생성

* 이름 및 태그

<table><thead><tr><th width="183.5999755859375">키</th><th width="253.2000732421875">값</th></tr></thead><tbody><tr><td>Name</td><td>dev-instance-user**</td></tr><tr><td>Env</td><td>dev-user**</td></tr></tbody></table>

* 애플리케이션 및 OS 이미지 : Amazon Linux
* Amazon Machine Image : Amazon Linux 2023 kernel-6.1 AMI
* 인스턴스 유형 : t3.small
* 키페어 생성 : user\*\*-key
* VPC : user\*\*-vpc
* 서브넷 : user\*\*-pub-subnet1
* 퍼블릭 IP 자동할당 : 활성화
* 방화벽(보안그룹) : 기존 보안 그룹
* 보안그룹 이름 : user\*\*-tagec2-sg



<figure><img src="../../.gitbook/assets/image (567).png" alt=""><figcaption></figcaption></figure>


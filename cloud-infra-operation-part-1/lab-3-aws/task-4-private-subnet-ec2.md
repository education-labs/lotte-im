# Task 4 - Private Subnet EC2 구축



1. Lab3 > Task2 를 참고하여 아래와 같은 ec2 2개 생성

첫번째 추가 인스턴스 1

* 이름 : user\*\*-linux-was
* 애플리케이션 및 OS 이미지 : Amazon Linux
* Amazon Machine Image : Amazon Linux 2023 kernel-6.1 AMI
* 인스턴스 유형 : t3.small
* 키페어 : user\*\*-key
* VPC : user\*\*-vpc&#x20;
* 서브넷 : user\*\*-pri-subnet-1(WAS)
* 퍼블릭 IP 자동할당 : 비활성화
* 방화벽(보안그룹) : 보안 그룹 생성
* 보안그룹 이름 : user\*\*-wasserver-sg
* 설명 : user\*\*-wasserver-sg
* 보안 그룹 규칙 1&#x20;
  * 유형 : 모든 트래픽&#x20;
  * 소스 유형 : 사용자 지정&#x20;
  * 원본 : 10.0.1.0/24
* 보안 그룹 규칙 2&#x20;
  * 유형 : 모든 트래픽&#x20;
  * 소스 유형 : 사용자 지정&#x20;
  * 원본 : 10.0.2.0/24





두번째 추가 인스턴스 2

* 이름 : user\*\*-linux-db
* 애플리케이션 및 OS 이미지 : Amazon Linux
* Amazon Machine Image : Amazon Linux 2023 kernel-6.1 AMI
* 인스턴스 유형 : t3.small
* 키페어 : user\*\*-key
* VPC : user\*\*-vpc&#x20;
* 서브넷 : user\*\*-pri-subnet-2(DB)
* 퍼블릭 IP 자동할당 : 비활성화
* 방화벽(보안그룹) : 기존 보안 그룹
* 보안그룹 이름 : user\*\*-dbserver-sg



2. user\*\*-dbserver-sg 보안그룹 규칙 수정

* 보안 그룹 규칙 1&#x20;
  * 유형 : 모든 트래픽&#x20;
  * 소스 유형 : 사용자 지정&#x20;
  * 원본 : 10.0.10.0/24


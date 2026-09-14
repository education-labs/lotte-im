# Task 6 - Securiy Group



1. 좌측 보안그룹 > 보안그룹 생성 클릭

<figure><img src="../../.gitbook/assets/image (417).png" alt=""><figcaption></figcaption></figure>





4. 아래와 같이 설정

* 보안 그룹 이름 : user\*\*-dbserver-sg
* 설명 : using dbserver
* VPC : user\*\*-vpc

<figure><img src="../../.gitbook/assets/image (481).png" alt=""><figcaption></figcaption></figure>





5. 아래와 같은 두 인바운드 규칙을 추가 한 뒤 보안그룹 생성 클릭

| 유형         | 포트범위 | 소스           |
| ---------- | ---- | ------------ |
| 사용자 지정 TCP | 3306 | 10.0.10.0/24 |



6. 다시 보안그룹 생성화면으로 이동하여 아래 설정대로 보안그룹 생성

* 보안 그룹 이름 : user\*\*-cacheserver-sg
* 설명 : using cacheserver
* VPC : user\*\*-vpc
* 인바운드 규칙&#x20;

| 유형         | 포트범위 | 소스           |
| ---------- | ---- | ------------ |
| 사용자 지정 TCP | 6379 | 10.0.10.0/24 |


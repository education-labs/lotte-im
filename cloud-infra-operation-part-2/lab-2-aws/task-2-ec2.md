# Task 2 - EC2 구성

1. 첫번째EC2 인스턴스를 생성(로드밸런서의 백엔드풀에 등록할 서버)

{% code overflow="wrap" %}
```
기본 정보

이름 : user**-lb-vm-1
AMI : Amazon Linux 2023 
인스턴스 유형 : t3.small
키페어 생성: user**-key
```
{% endcode %}

{% code overflow="wrap" %}
```
네트워크 설정

vpc : user**-vpc
subnet : user**-pri-subnet-1
보안 그룹 생성 선택
보안 그룹 이름 : user**-lb-vm-sg
인바운드 보안 그룹 규칙 
유형 : http
소스 유형 : 위치 무관
```
{% endcode %}

{% code overflow="wrap" %}
```
고급 세부정보

사용자 데이터 코드 :

#!/bin/bash
sudo yum update -y
sudo yum install -y httpd
sudo echo hello lb backend server 1 > /var/www/html/index.html
sudo systemctl start httpd
sudo systemctl enable httpd
```
{% endcode %}



2. 두번째 EC2 인스턴스를 생성(로드밸런서의 백엔드풀에 등록할 서버)

{% code overflow="wrap" %}
```
기본 정보

이름 : user**-lb-vm-2
AMI : Amazon Linux 2023 
인스턴스 유형 : t3.small
키페어 : user**-key
```
{% endcode %}

{% code overflow="wrap" %}
```
네트워크 설정

vpc : user**-vpc
subnet : user**-pri-subnet-2
기존 보안그룹 선택
보안 그룹 이름 : user**-lb-vm-sg
```
{% endcode %}

{% code overflow="wrap" %}
```
고급 세부정보

사용자 데이터 코드 :

#!/bin/bash
sudo yum update -y
sudo yum install -y httpd
sudo echo hello lb backend server 2 > /var/www/html/index.html
sudo systemctl start httpd
sudo systemctl enable httpd
```
{% endcode %}


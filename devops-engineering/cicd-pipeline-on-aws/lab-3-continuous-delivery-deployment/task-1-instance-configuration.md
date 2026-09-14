# Task 1 - Instance Configuration

## 실습 소개

인스턴스를 생성하고 해당 인스턴스로 웹 서버를 배포하기 위한 구성을 해봅니다.



1. IAM 서비스로 이동

<div align="left"><figure><img src="../../../.gitbook/assets/image (827).png" alt="" width="563"><figcaption></figcaption></figure></div>



2. 좌측 역할 클릭하고, 우측 역할 만들기 클릭

<figure><img src="../../../.gitbook/assets/image (828).png" alt=""><figcaption></figcaption></figure>



3. EC2 선택 후 다음 클릭

<figure><img src="../../../.gitbook/assets/image (829).png" alt=""><figcaption></figcaption></figure>



4\. AmazonEC2RoleforAWSCodeDeploy 를 검색후 정책을 선택한 뒤 다음 클릭

<figure><img src="../../../.gitbook/assets/image (830).png" alt=""><figcaption></figcaption></figure>

5. 역할 이름 : user##-WebServerRole 입력 후 하단의 역할 생성 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (831).png" alt="" width="375"><figcaption></figcaption></figure></div>



6. EC2 서비스로 이동한 뒤 인스턴스 시작 버튼 클릭

<figure><img src="../../../.gitbook/assets/image (832).png" alt=""><figcaption></figcaption></figure>



7. 아래와 같이 설정한 뒤 인스턴스 시작 클릭

* 이름 : user##-AngularProject
* 애플리케이션 및 OS 이미지 (AMI) : Amazon Linux 2023
* 인스턴스 유형 : t2.micro
* 키페어 생성 :  user##-Angular-key
* 네트워크 설정 : <mark style="background-color:green;">SSH 트래픽 허용 체크, 인터넷에서 HTTPS 트래픽 허용 체크, 인터넷에서 HTTP 트래픽 허용 체크</mark>

나머지는 기본값으로 설정

![](<../../../.gitbook/assets/image (833).png>)



8. 인스턴스 시작 성공화면에서 인스턴스 id 값 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (834).png" alt="" width="375"><figcaption></figcaption></figure></div>



9. 좌측 체크박스 체크, 우측 작업 클릭, 보안 클릭, IAM 역할 수정 클릭

<figure><img src="../../../.gitbook/assets/image (835).png" alt=""><figcaption></figcaption></figure>



10. IAM 역할 선택을 클릭하고 user## 을 검색하여 이전에 생성했던 역할을 선택한 뒤 IAM 역할 업데이트 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (836).png" alt="" width="563"><figcaption></figcaption></figure></div>



11. 인스턴스 목록 상단에 user##-AngularProject 를 검색하여 출력되는 인스턴스를 선택하고, 연결 클릭

<figure><img src="../../../.gitbook/assets/image (837).png" alt=""><figcaption></figcaption></figure>



12. EC2 인스턴스 연결 탭의 우측 하단 연결 클릭

<figure><img src="../../../.gitbook/assets/image (838).png" alt=""><figcaption></figcaption></figure>



13. 패키지 업데이트 및 wget, ruby 설치

```
sudo yum update -y
sudo yum install -y wget ruby
```

<figure><img src="../../../.gitbook/assets/image (839).png" alt=""><figcaption></figcaption></figure>



14. Codedeploy 의 install 패키지 설치

```
wget https://aws-codedeploy-ap-northeast-2.s3.ap-northeast-2.amazonaws.com/latest/install
```

<div align="left"><figure><img src="../../../.gitbook/assets/image (840).png" alt="" width="563"><figcaption></figcaption></figure></div>



15. 권한 변경 및 실행

```
chmod +x ./install
sudo ./install auto
```

<div align="left"><figure><img src="../../../.gitbook/assets/image (841).png" alt="" width="563"><figcaption></figcaption></figure></div>

정상동작중인지 확인&#x20;

```
sudo systemctl status codedeploy-agent.service
```

<figure><img src="../../../.gitbook/assets/image (842).png" alt=""><figcaption></figcaption></figure>

* 비정상일시 재시작

```
sudo systemctl restart codedeploy-agent.service
```

* 계속 비정상일시 삭제 후 재설치

```
sudo yum erase codedeploy-agent
```

```
sudo ./install auto
```

16. nginx (웹서버구동 패키지) 설치

```
sudo yum install -y nginx
```

<div align="left"><figure><img src="../../../.gitbook/assets/image (843).png" alt="" width="563"><figcaption></figcaption></figure></div>



17. 실행 및 확인

```
sudo service nginx start 
sudo service nginx status
```

<div align="left"><figure><img src="../../../.gitbook/assets/image (844).png" alt="" width="563"><figcaption></figcaption></figure></div>



18. 재부팅되도 nginx 실행되게 설정

```
sudo systemctl enable nginx.service
```

<figure><img src="../../../.gitbook/assets/image (845).png" alt=""><figcaption></figcaption></figure>



19. 디렉토리 생성

```
sudo mkdir -p /var/www/my-angular-project
```



20. nginx 구성파일 수정

```
sudo nano /etc/nginx/nginx.conf
```

```
/var/www/my-angular-project;
```

<figure><img src="../../../.gitbook/assets/image (846).png" alt=""><figcaption></figcaption></figure>



21. 수정을 한뒤 Ctrl +  x 입력 후 y 입력, 그리고 Enter 키 입력후 수정이 되었는지 확인

```
sudo cat /etc/nginx/nginx.conf
```

<figure><img src="../../../.gitbook/assets/image (847).png" alt=""><figcaption></figcaption></figure>



22. nginx 재시작

```
sudo service nginx restart
```

<figure><img src="../../../.gitbook/assets/image (848).png" alt=""><figcaption></figcaption></figure>



23. 인스턴스 목록으로 이동 후 해당 인스턴스의 DNS 확인 및 복사

<figure><img src="../../../.gitbook/assets/image (849).png" alt=""><figcaption></figcaption></figure>



24. 복사한 링크를 웹브라우저에서 접속

<figure><img src="../../../.gitbook/assets/image (850).png" alt=""><figcaption></figcaption></figure>

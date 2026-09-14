# Task 2 - Web 서버 구축(Linux)

1\. 인스턴스 > 인스턴스 시작 클릭

<figure><img src="../../.gitbook/assets/image (380).png" alt=""><figcaption></figcaption></figure>



2. 아래와 같이 설정

* 이름 : user\*\*-linux-web
* 애플리케이션 및 OS 이미지 : Amazon Linux
* Amazon Machine Image : Amazon Linux 2023 kernel-6.1 AMI
* 인스턴스 유형 : t3.small

<figure><img src="../../.gitbook/assets/image (381).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (382).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (383).png" alt=""><figcaption></figcaption></figure>





3\. 키 페어(로그인) 은 이전에 생성한 user\*\*-key 선택

<figure><img src="../../.gitbook/assets/image (384).png" alt=""><figcaption></figcaption></figure>





4\. 네트워크 설정 섹션의 편집 버튼을 클릭합니다.

<figure><img src="../../.gitbook/assets/image (362).png" alt=""><figcaption></figcaption></figure>





5. 네트워크 설정에서 다음과 같이 설정을 진행합니다.

* VPC : user\*\*-vpc&#x20;
* 서브넷 : user\*\*-subnet-1
* 퍼블릭 IP 자동할당 : 활성화 선택
* 방화벽(보안그룹) : 보안 그룹 생성
* 보안그룹 이름 : user\*\*-web-sg
* 설명 : user\*\*-web-sg

<figure><img src="../../.gitbook/assets/image (386).png" alt=""><figcaption></figcaption></figure>



&#x20;6\. 인바운드 보안 그룹 규칙에서 아래와 같이 1 규칙을 설정하고 보안그룹 규칙 추가 클릭

* 유형 : ssh
* 소스 유형 : 내 IP

<figure><img src="../../.gitbook/assets/image (387).png" alt=""><figcaption></figcaption></figure>



7. 아래와 같이 설정

* 유형 : HTTP
* 소스 유형 : 위치 무관

<figure><img src="../../.gitbook/assets/image (388).png" alt=""><figcaption></figcaption></figure>





8. 나머지 설정은 기본값으로 그대로 둔채 우측의 인스턴스 시작을 클릭

<figure><img src="../../.gitbook/assets/image (389).png" alt="" width="470"><figcaption></figcaption></figure>





9. 생성이 되면 확인되는 인스턴스 id값을 클릭

<figure><img src="../../.gitbook/assets/image (368).png" alt=""><figcaption></figcaption></figure>







10. 연결 버튼 클릭

<figure><img src="../../.gitbook/assets/image (369).png" alt=""><figcaption></figcaption></figure>





11. ssh 클라이언트 탭을 클릭하고 하단의 예에 출력되어있는 명령을 복사하여 메모장에 저장

<figure><img src="../../.gitbook/assets/image (370).png" alt=""><figcaption></figcaption></figure>





12. 윈도우즈 검색창에 powershell 을 검색하고 해당 프로그램을 실행

<figure><img src="../../.gitbook/assets/image (371).png" alt=""><figcaption></figcaption></figure>







13. 아래 명령을 사용하여 키페어를 다운로드 받았던 Downloads 폴더로 이동

```
cd .\Downloads\
```

<figure><img src="../../.gitbook/assets/image (372).png" alt=""><figcaption></figcaption></figure>





14. 11번에서 복사한 명령어로 인스턴스에 접속합니다. (key fingerprint 저장하는 질문에는 yes로 입력)

<figure><img src="../../.gitbook/assets/image (373).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (374).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
download 폴더에서도 권한 에러가 나는 경우

권한 변경 명령 사용

```
icacls .\user**-key.pem /inheritance:r
icacls .\user**-key.pem /grant:r "$($env:USERNAME):(R)"
```

Removing .. username 이 나오는경우&#x20;

```
icacls .\user**-key.pem /remove "username"
```
{% endhint %}





15. 웹 서버 환경 구성을 위한 업데이트, 설치 및 실행을 진행합니다. 먼저 업데이트를 실행합니다.

```
sudo yum update -y
```

<figure><img src="../../.gitbook/assets/image (375).png" alt=""><figcaption></figcaption></figure>





16. 업데이트가 완료되면 아파치 웹서버를 설치합니다.

```
sudo yum install httpd -y
```

<figure><img src="../../.gitbook/assets/image (376).png" alt=""><figcaption></figcaption></figure>





17\. 웹서버 설치 후를 입력하여 웹서버를 실행합니다.

```
sudo service httpd start
```

<figure><img src="../../.gitbook/assets/image (377).png" alt=""><figcaption></figcaption></figure>





18. 웹서버가 설치된 인스턴스의 퍼블릭 IP 주소를 복사합니다.

<figure><img src="../../.gitbook/assets/image (378).png" alt=""><figcaption></figcaption></figure>



19. 인스턴스의 퍼블릭 IP 주소를 웹 브라우저에서 입력하여 확인해보면 Test Page가 보이며 정상적으로 웹서버가 설치된 것을 알 수 있습니다.

```
http://<퍼블릭 ip 주소>
```

&#x20;

<figure><img src="../../.gitbook/assets/image (379).png" alt=""><figcaption></figcaption></figure>

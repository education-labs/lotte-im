# Task 1 - Windows 서버 구축

1. EC2 검색후 서비스로 이동

<figure><img src="../../.gitbook/assets/image (337).png" alt=""><figcaption></figcaption></figure>



2. 인스턴스 > 인스턴스 시작 클릭

<figure><img src="../../.gitbook/assets/image (338).png" alt=""><figcaption></figcaption></figure>





3. 아래와 같이 설정

* 이름 : user\*\*-win-ec2
* 애플리케이션 및 OS 이미지 : Windows&#x20;
* Amazon Machine Image : Microsoft Windows Server 2025 Base
* 인스턴스 유형 : t3.small

<figure><img src="../../.gitbook/assets/image (339).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (340).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (341).png" alt=""><figcaption></figcaption></figure>





4. 새 키 페어 생성 클릭

<figure><img src="../../.gitbook/assets/image (342).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (343).png" alt="" width="563"><figcaption></figcaption></figure>

{% hint style="info" %}
이때 생성되어 다운로드 되는 .pem 파일은 1회만 다운로드 됩니다.&#x20;

따라서 보관에 주의하여야 합니다.
{% endhint %}





5. 네트워크 설정 편집 클릭후 아래와 같이 설정

<figure><img src="../../.gitbook/assets/image (344).png" alt=""><figcaption></figcaption></figure>



* VPC : user\*\*-vpc
* 서브넷 : user\*\*-subnet-1
* 퍼블릭 IP 자동 할당 : 활성화
* 방화벽 : 보안 그룹 생성
* 보안 그룹 이름 : user\*\*-win-sg
* 설명 : user\*\*-win-sg

<figure><img src="../../.gitbook/assets/image (345).png" alt=""><figcaption></figcaption></figure>



6. 인바운드 규칙을 확인하고 인스턴스 시작 클릭

<figure><img src="../../.gitbook/assets/image (346).png" alt=""><figcaption></figcaption></figure>





7. 인스턴스 목록으로 이동하고, user\*\* 필터 검색 한뒤 아래 출력되는 인스턴스를 선택한 후 연결 클릭

<figure><img src="../../.gitbook/assets/image (347).png" alt=""><figcaption></figcaption></figure>





8. RDP 클라이언트 > 원격 데스크톱 파일 다운로드 클릭

<figure><img src="../../.gitbook/assets/image (348).png" alt=""><figcaption></figcaption></figure>





9. 아래 암호가져오기 클릭

<figure><img src="../../.gitbook/assets/image (350).png" alt=""><figcaption></figcaption></figure>





10. 이전에 다운로드받았던 키페어 업로드 및 암호해독 클릭

<figure><img src="../../.gitbook/assets/image (351).png" alt=""><figcaption></figcaption></figure>





11. 암호 확인

<figure><img src="../../.gitbook/assets/image (352).png" alt=""><figcaption></figcaption></figure>





12. 8에서 다운받은해당 파일 실행

<figure><img src="../../.gitbook/assets/image (349).png" alt=""><figcaption></figcaption></figure>





13. 아래와 같이 원격 접속&#x20;

<figure><img src="../../.gitbook/assets/image (353).png" alt=""><figcaption></figcaption></figure>





14. 접속 성공 화면

<figure><img src="../../.gitbook/assets/image (354).png" alt=""><figcaption></figcaption></figure>


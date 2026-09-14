# Task 1 - 인스턴스 SSM 접속

{% hint style="info" %}
해당 실습에서 적용하고자 하는 인스턴스는 Pub 서브넷에 배치되어있어야하며,&#x20;

Amazon Linux로 배포된 SSM Agent 가 설치된 인스턴스여야합니다.
{% endhint %}



1. 보안 그룹의 인바운드 규칙중 ssh(22포트) 설정을 삭제



2. IAM 서비스로 이동하고 역할 생성&#x20;

<figure><img src="../../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>





3. AWS 서비스 > EC2 > EC2 > 다음 클릭

<figure><img src="../../.gitbook/assets/image (38).png" alt="" width="563"><figcaption></figcaption></figure>





4. AmazonSSMManagedInstanceCore 정책을 검색 및 선택하고 다음 클릭

<figure><img src="../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>



5. 역할 이름에 ec2-ssm-role-user\*\* 를 입력하고 역할 생성 클릭

<figure><img src="../../.gitbook/assets/image (40).png" alt="" width="536"><figcaption></figcaption></figure>



6. 해당 인스턴스에 IAM 역할을 부여

<figure><img src="../../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>



7. 방금 만든 역할을 선택하고 업데이트 클릭

<figure><img src="../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

8. 해당 인스턴스를 선택하고 연결 클릭

<figure><img src="../../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>



9. SSM Session Manager 탭을 클릭하고 Ping Status 가 온라인이 되면, 연결 클릭

{% hint style="info" %}
인스턴스 내 SSM Agent 가 IAM 적용 설정을 가져오기까지 2\~10분 정도 소요될 수 있습니다.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>



10. 접속 성공 화면

<figure><img src="../../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>


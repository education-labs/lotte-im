# Task 3 - Cloudwatch Agent

1. IAM 서비스로 이동한 뒤 역할 생성을 클릭 한뒤, AWS 서비스, EC2 선택, 다음 클릭

<figure><img src="../../.gitbook/assets/image (23).png" alt="" width="361"><figcaption></figcaption></figure>

2. 검색란에 CloudWatchAgentServerPolicy 를 검색하여 선택 한 뒤, AmazonSSMFullAccess 권한도 검색하여 선택한 뒤 다음을 클릭

<figure><img src="../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

3. 역할 이름란에 user\*\*-CloudWatchAgentServerPolicy 를 입력한 뒤 역할 생성을 클릭

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

4. EC2 콘솔로 이동하여 CloudWatch Agent 를 설치할 실행중인 인스턴스를 선택한 뒤, 작업 > 보안 > IAM 역할 수정 클릭

<figure><img src="../../.gitbook/assets/image (27).png" alt="" width="375"><figcaption></figcaption></figure>

5. 역할에 방금 만든 CloudwatchAgentServerPolicy를 선택하고 IAM 역할 업데이트를 클릭

<figure><img src="../../.gitbook/assets/image (28).png" alt="" width="563"><figcaption></figcaption></figure>

6. 해당 인스턴스로 접속 (Powershell OR 연결 콘솔에서 직접 연결)
7. 아래 명령으로 Cloudwatch Agent 설치

{% code overflow="wrap" lineNumbers="true" %}
```
sudo yum install -y amazon-cloudwatch-agent
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

8. 에이전트 구성 마법사 실행

<pre><code><strong>sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
</strong></code></pre>

9. Enter 키를 연속으로 입력하여 Default 설정으로 선택하다가 아래와 같이 Log file path 를 입력하라는 메시지가 나오면 /var/log/messages 를 입력

<figure><img src="../../.gitbook/assets/image (30).png" alt="" width="563"><figcaption></figcaption></figure>

10. 그리고 다시 Enter 키를 연속으로 입력하여 Default 설정으로 선택을 하다가 모니터링할 추가 로그 파일을 지정하시겠습니까? 라는 질문 이후 Log file path 에 /var/log/secure 를 입력

<figure><img src="../../.gitbook/assets/image (31).png" alt="" width="563"><figcaption></figcaption></figure>

11. 그리고 다시 Enter 키를 연속으로 입력하여 Default 설정으로 선택을 하다가 모니터링할 추가 로그 파일을 지정하시겠습니까? 라는 질문이 다시 나오면 2를 입력하고, CloudWatch 에이전트가 X-Ray 추적도 검색하도록 하시겠습니까? 라는 질문에도 2를 입력

<figure><img src="../../.gitbook/assets/image (33).png" alt="" width="563"><figcaption></figcaption></figure>

12. 그리고 다시 Enter 키를 연속으로 입력하다가 SSM Parameter store 메시지에는 2입력

<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

13. Collectd 설치

{% code overflow="wrap" lineNumbers="true" %}
```
sudo amazon-linux-extras install -y collectd
```
{% endcode %}

또는

{% code overflow="wrap" lineNumbers="true" %}
```
sudo yum install -y collectd
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

14. CloudWatch Agent 실행

{% code overflow="wrap" lineNumbers="true" %}
```
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s -c \
file:/opt/aws/amazon-cloudwatch-agent/bin/config.json
```
{% endcode %}

15. CloudWatch 로 이동하여 지표 > 모든지표 > CWAgent 네임스페이스 > InstanceID > 메모리 지표와 디스크 지표가 추가된것을 확인

{% hint style="info" %}
CWAgent 가 추가 되기까지 1\~2분 소요
{% endhint %}

<figure><img src="../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

# Task 5 - Streaming Deployment Logs to CloudWatch Logs

실습 소개

배포 인스턴스에 CloudWatch Agent를 설치하고 로그들을 확인해봅니다.



1. EC2 로 이동하여 기존 생성했던 user##-AngularProject 인스턴스의 IAM 역할 클릭

<figure><img src="../../../.gitbook/assets/image (875).png" alt=""><figcaption></figcaption></figure>



2. 권한탭 클릭, 권한 추가 클릭, 정책 연결 클릭

<figure><img src="../../../.gitbook/assets/image (876).png" alt=""><figcaption></figcaption></figure>



3. 검색 란에 CloudWatchAgentServerPolicy 를 검색하고 선택한 뒤 권한 추가 클릭

<figure><img src="../../../.gitbook/assets/image (877).png" alt=""><figcaption></figcaption></figure>



4. EC2 인스턴스에 접속

<figure><img src="../../../.gitbook/assets/image (878).png" alt=""><figcaption></figcaption></figure>



5. CloudWatch agent 다운로드

```
wget https://s3.amazonaws.com/amazoncloudwatch-\
agent/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm
```

<figure><img src="../../../.gitbook/assets/image (879).png" alt=""><figcaption></figcaption></figure>



6. CloudWatch agent 설치

```
sudo rpm -U ./amazon-cloudwatch-agent.rpm
```

<figure><img src="../../../.gitbook/assets/image (880).png" alt=""><figcaption></figcaption></figure>



7. 구성 파일 생성

```
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
```



8. OS : Linux / EC2 / cwagent / StatsD : no / CollectD : no / Monitor : no / 순서로 선택

<figure><img src="../../../.gitbook/assets/image (881).png" alt=""><figcaption></figcaption></figure>



9. Log Agent : no / Monitor Log file : yes /

Path :

```
/opt/codedeploy-agent/deployment-root/deployment-logs/codedeploy-agent-deployments.log
```

Log group name :&#x20;

```
user##codedeploy-agent-deployments.log
```

Log group class : Standard \
Log stream name : {instance\_id} \
Log Group Retention in days : Enter 순서로 입력





10. additional log files to monitor : 2 / X-ray traces : 2 / SSM : 2 선택

<figure><img src="../../../.gitbook/assets/image (882).png" alt=""><figcaption></figcaption></figure>

11. 구성파일 편집 (vi 편집기 사용)

```
sudo vi /opt/aws/amazon-cloudwatch-agent/bin/config.json
```

-1 우측에 <mark style="background-color:red;">쉼표</mark>를 추가하고 하단에 아래와 같이입력

```
"timestamp_format": "[%Y-%m-%d %H:%M:%S.%f]"
```

<figure><img src="../../../.gitbook/assets/image (883).png" alt=""><figcaption></figcaption></figure>

수정한 뒤 저장하고 나가기





12. 7\~12 과정을 파일생성 방식으로 진행하는 방법

{% hint style="info" %}
위 wizard 로 config 파일 구성이 아닌 config 파일 생성방식
{% endhint %}

* 아래 명령 사용

{% code lineNumbers="true" %}
```
sudo vi /opt/aws/amazon-cloudwatch-agent/bin/config.json
```
{% endcode %}



* 파일 내용을 아래와 같이 입력한 뒤 저장

{% code lineNumbers="true" %}
```
{
        "agent": {
                "run_as_user": "cwagent"
        },
        "logs": {
                "logs_collected": {
                        "files": {
                                "collect_list": [
                                        {
                                                "file_path": "/opt/codedeploy-agent/deployment-root/deployment-logs/codedeploy-agent-deployments.log",
                                                "log_group_class": "STANDARD",
                                                "log_group_name": "user##codedeploy-agent-deployments.log",
                                                "log_stream_name": "{instance_id}",
                                                "retention_in_days": -1,
                                                "timestamp_format": "[%Y-%m-%d %H:%M:%S.%f]"
                                        }
                                ]
                        }
                }
        }
}
```
{% endcode %}







12. Agent 실행

```
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a \
fetch-config -m ec2 -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json -s
```

<figure><img src="../../../.gitbook/assets/image (884).png" alt=""><figcaption></figcaption></figure>



13. Agent 상태 확인

```
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status
```

<figure><img src="../../../.gitbook/assets/image (885).png" alt=""><figcaption></figcaption></figure>



14. AWS 콘솔에서 Cloudwatch 로 이동&#x20;

<figure><img src="../../../.gitbook/assets/image (886).png" alt=""><figcaption></figcaption></figure>



좌측 로그, 로그 관리, 로그그룹 선택, user## 검색, 이전에 지정했던 log 파일을 선택

<figure><img src="../../../.gitbook/assets/image (988).png" alt=""><figcaption></figcaption></figure>



인스턴스 id 선택하여 로그 이벤트 확인가능

<figure><img src="../../../.gitbook/assets/image (989).png" alt=""><figcaption></figcaption></figure>

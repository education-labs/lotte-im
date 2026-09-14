# Task 5 - 접속 Test

1. Lab 3 > Task 2 를 참고하여 WEB 서버에 접속

<figure><img src="../../.gitbook/assets/image (329).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Web 서버는 사설 IP 10.0.1.X 를 갖습니다.
{% endhint %}



2. 파워쉘을 하나 더 실행하여, Local 환경에서 WEB 서버에게 키페어 파일 전송

{% code overflow="wrap" %}
```
cd Download
```
{% endcode %}

{% code overflow="wrap" %}
```
scp -i user**-key.pem user**-key.pem ec2-user@<WebServerPubIP>:/home/ec2-user/
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (330).png" alt=""><figcaption></figcaption></figure>







3. Web 서버에서 KeyPair 파일 권한 변경

{% code overflow="wrap" %}
```
chmod 600 user**-key.pem
```
{% endcode %}





4. WEB 서버에서 WAS 접속

{% hint style="info" %}
WAS 서버는 Private 서브넷에 배치가 되었기에 Public IP가 없고, 외부에서 직접 연결할 방법이 없습니다.
{% endhint %}

{% code overflow="wrap" %}
```
ssh -i user**-key.pem ec2-user@<WasIP>
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (331).png" alt=""><figcaption></figcaption></figure>





5. DB NACL 설정에서 인바운드규칙 편집&#x20;

<figure><img src="../../.gitbook/assets/image (327).png" alt=""><figcaption></figcaption></figure>



6. WEB 에서 WAS로 KeyPair 전송 (WEB 서버에 접속한 터미널에서)

{% code overflow="wrap" %}
```
scp -i user**-key.pem user**-key.pem ec2-user@<WASIP>:/home/ec2-user/
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (326).png" alt=""><figcaption></figcaption></figure>





7. WAS 터미널에서 DB로 접속

<figure><img src="../../.gitbook/assets/image (325).png" alt=""><figcaption></figcaption></figure>


# Task 2 - RunCommand 활용 웹서버 구축

1. System Manager 서비스로 이동

<figure><img src="../../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>



2. 명령실행 > 명령실행 클릭

<figure><img src="../../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>





3. AWS-RunShellScript 를 검색하고 해당항목을 선택

<figure><img src="../../.gitbook/assets/image (48).png" alt="" width="563"><figcaption></figcaption></figure>





4. 명령 박스에 아래 코드를 입력

{% code overflow="wrap" lineNumbers="true" %}
```
sudo systemctl stop httpd
sudo systemctl disable httpd

sudo yum update -y
sudo yum install -y nginx
sudo systemctl start nginx
sudo systemctl enable nginx

echo "<h1>Success! Nginx replaced httpd via Run Command</h1>" | sudo tee /usr/share/nginx/html/index.html
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>



5. 대상에서 수동으로 인스턴스 선택 > 해당 인스턴스를 선택

<figure><img src="../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>



6. S3 버킷에 쓰기 활성화 해제

<figure><img src="../../.gitbook/assets/image (51).png" alt="" width="428"><figcaption></figcaption></figure>





7. 하단의 실행 클릭

<figure><img src="../../.gitbook/assets/image (52).png" alt="" width="254"><figcaption></figcaption></figure>







8. 새로고침을 클릭하여 상태가 성공임을 확인

<figure><img src="../../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>



9. 해당 인스턴스의 퍼블릭IP를 웹브라우저로 접속

메시지가 위 4번단계에서 설정했던 메시지로 바뀌었는지 확인

<figure><img src="../../.gitbook/assets/image (54).png" alt=""><figcaption></figcaption></figure>

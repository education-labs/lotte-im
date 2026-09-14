# Task 3 - 동적 Web 서버 구축

1. Lab3 > Task2 를 참고하여 아래와 같은 ec2 설정
   * 이름 : user\*\*-linux-web-2
   * 애플리케이션 및 OS 이미지 : Amazon Linux
   * Amazon Machine Image : Amazon Linux 2023 kernel-6.1 AMI
   * 인스턴스 유형 : t3.small
   * 키페어 : user\*\*-key
   * VPC : user\*\*-vpc&#x20;
   * 서브넷 : user\*\*-subnet-2
   * 퍼블릭 IP 자동할당 : 활성화
   * 방화벽(보안그룹) : 기존 보안 그룹
   * 보안그룹 이름 : user\*\*-web-sg



2. 하단으로 스크롤하여 고급 세부 정보를 확장하고, 사용자 데이터 박스에 아래 코드 입력

<figure><img src="../../.gitbook/assets/image (332).png" alt="" width="251"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (333).png" alt="" width="563"><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```
#!/bin/bash
# 1. 시스템 업데이트 및 Apache 웹 서버 설치
dnf update -y
dnf install -y httpd
systemctl start httpd
systemctl enable httpd

# 2. IMDSv2 토큰 가져오기 (6시간 동안 유효한 토큰 발행)
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")

# 3. 토큰을 사용하여 인스턴스 메타데이터 추출
INSTANCE_ID=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/instance-id)
LOCAL_IP=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/local-ipv4)
AVAILABILITY_ZONE=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/placement/availability-zone)

# 4. 추출한 정보를 사용하여 동적 웹 페이지 생성
cat <<EOF > /var/www/html/index.html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>EC2 Identity Page</title>
    <style>
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; text-align: center; margin-top: 100px; background-color: #f4f7f9; color: #232f3e; }
        .card { background: white; padding: 40px; border-radius: 15px; box-shadow: 0 10px 25px rgba(0,0,0,0.1); display: inline-block; border-top: 8px solid #ff9900; }
        h1 { font-size: 2.5em; margin-bottom: 20px; }
        .info-box { text-align: left; background: #f1f3f5; padding: 20px; border-radius: 8px; margin: 20px 0; }
        .label { font-weight: bold; color: #7d8c8d; display: inline-block; width: 150px; }
        .value { color: #2ecc71; font-family: monospace; font-size: 1.1em; }
        .footer { font-size: 0.9em; color: #95a5a6; }
    </style>
</head>
<body>
    <div class="card">
        <h1>"나는 누구? 여긴 어디?"</h1>
        <div class="info-box">
            <p><span class="label">인스턴스 ID:</span> <span class="value">$INSTANCE_ID</span></p>
            <p><span class="label">프라이빗 IP:</span> <span class="value">$LOCAL_IP</span></p>
            <p><span class="label">가용 영역(AZ):</span> <span class="value">$AVAILABILITY_ZONE</span></p>
        </div>
        <p class="footer">이 페이지는 <b>User Data</b> 스크립트에 의해 배포 시점에 자동 생성되었습니다.</p>
    </div>
</body>
</html>
EOF
```
{% endcode %}



3. 인스턴스 시작 클릭

<figure><img src="../../.gitbook/assets/image (335).png" alt="" width="480"><figcaption></figcaption></figure>







4. 해당 EC2 인스턴스의 Public IP를 확인하고 브라우저에서 접속

<figure><img src="../../.gitbook/assets/image (489).png" alt=""><figcaption></figcaption></figure>

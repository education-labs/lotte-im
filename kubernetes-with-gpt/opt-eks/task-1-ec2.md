# Task 1 - EC2 가상머신 배포



1. AWS 계정 로그인

&#x20;제공 받은 계정으로 로그인합니다.



2. EC2 서비스로 이동

&#x20;상단의 검색창 클릭, EC2 검색, 하단 EC2 클릭

<figure><img src="../../.gitbook/assets/image (585).png" alt=""><figcaption></figcaption></figure>



3. 인스턴스 시작

&#x20;좌측 인스턴스 클릭, 우측 인스턴스 시작 클릭

<figure><img src="../../.gitbook/assets/image (586).png" alt=""><figcaption></figcaption></figure>



4. 아래와 같이 설정

* 이름 : user\*\*-ide
* 애플리케이션 및 OS 이미지 : Ubutu
* AMI : Ubuntu Server 24.04

<figure><img src="../../.gitbook/assets/image (587).png" alt=""><figcaption></figcaption></figure>





5. 아래와 같이 설정

* 인스턴스 유형 : t3.medium
* 키페어 : 키 페어 없이 계속 진행

<figure><img src="../../.gitbook/assets/image (588).png" alt=""><figcaption></figcaption></figure>



6. 아래와 같이 설정

* 방화벽 : 기존 보안 그룹 선택
* 보안그룹 : default&#x20;
* 스토리지 구성 : 20GB&#x20;

<figure><img src="../../.gitbook/assets/image (589).png" alt=""><figcaption></figcaption></figure>





7. 아래와 같이 설정하여 사용자데이터 입력

* 고급 세부정보를 확장

<figure><img src="../../.gitbook/assets/image (590).png" alt=""><figcaption></figcaption></figure>





* 아래 코드를 사용자데이터에 입력

```
#!/bin/bash

# ==========================================
# 1. 설정 변수 (비밀번호, 포트, 버전)
# ==========================================
USER_ID="ubuntu"
USER_PW="AiEKS!23"
IDE_PORT="8080"
CODE_SERVER_VERSION="4.96.4"

# 로그 기록 (디버깅용: /var/log/user-data.log)
exec > >(tee /var/log/user-data.log|logger -t user-data -s 2>/dev/console) 2>&1

echo ">>> [Start] Setup Script (HTTP Mode)"

# ==========================================
# 2. 필수 패키지 설치
# ==========================================
export DEBIAN_FRONTEND=noninteractive
apt-get update
apt-get install -y unzip jq git curl wget net-tools

# ==========================================
# 3. SSH 비밀번호 접속 설정 (Ubuntu 24.04 완벽 대응)
# ==========================================
echo ">>> [Setup] Configuring SSH..."

# 3-1. ubuntu 계정 비밀번호 설정
echo "$USER_ID:$USER_PW" | chpasswd

# 3-2. [핵심] 접속을 막는 Cloud-init 기본 설정 파일 삭제
# 이 파일들이 남아있으면 PasswordAuthentication: no 설정이 유지됨
rm -f /etc/ssh/sshd_config.d/50-cloud-init.conf
rm -f /etc/ssh/sshd_config.d/60-cloudimg-settings.conf

# 3-3. 접속 허용 설정 파일 생성 (우선순위 높음)
cat <<EOF > /etc/ssh/sshd_config.d/99-manual-auth.conf
PasswordAuthentication yes
PermitRootLogin prohibit-password
ChallengeResponseAuthentication no
UsePAM yes
EOF

# 3-4. 메인 설정 파일 수정 (이중 안전장치)
sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config

# 3-5. Cloud-Init이 재부팅 후 설정을 덮어쓰지 못하게 방어
sed -i 's/ssh_pwauth:   0/ssh_pwauth:   1/g' /etc/cloud/cloud.cfg
sed -i 's/ssh_pwauth:   false/ssh_pwauth:   true/g' /etc/cloud/cloud.cfg

# 3-6. SSH 서비스 재시작
systemctl restart ssh

# ==========================================
# 4. Code-Server 설치 (HTTP 모드)
# ==========================================
echo ">>> [Setup] Installing Code-Server..."

# 4-1. 안정적인 버전의 DEB 파일 다운로드 및 설치
wget "https://github.com/coder/code-server/releases/download/v${CODE_SERVER_VERSION}/code-server_${CODE_SERVER_VERSION}_amd64.deb" -O /tmp/code-server.deb
dpkg -i /tmp/code-server.deb

# 4-2. 설정 디렉토리 생성
mkdir -p /home/$USER_ID/.config/code-server

# 4-3. config.yaml 생성 (HTTP 모드: cert: false)
cat <<EOF > /home/$USER_ID/.config/code-server/config.yaml
bind-addr: 0.0.0.0:$IDE_PORT
auth: password
password: $USER_PW
cert: false
EOF

# 4-4. 소유권 변경 (root -> ubuntu)
chown -R $USER_ID:$USER_ID /home/$USER_ID/.config

# ==========================================
# 5. 서비스 등록 및 실행
# ==========================================
# 서비스 파일 강제 생성 (Unit not found 에러 방지)
cat <<EOF > /lib/systemd/system/code-server@.service
[Unit]
Description=code-server for %i
After=network.target

[Service]
Type=simple
User=%i
ExecStart=/usr/bin/code-server
Restart=always

[Install]
WantedBy=multi-user.target
EOF

# 5-1. 서비스 실행
systemctl daemon-reload
systemctl enable --now code-server@$USER_ID

echo ">>> [End] Setup Complete. Access via HTTP://$IDE_PORT"
```

<figure><img src="../../.gitbook/assets/image (591).png" alt=""><figcaption></figcaption></figure>



8. 인스턴스 시작

* 우측의 인스턴스 시작 클릭 및 인스턴스 id 클릭

<figure><img src="../../.gitbook/assets/image (592).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (593).png" alt=""><figcaption></figcaption></figure>



9. 퍼블릭 IP 확인

<figure><img src="../../.gitbook/assets/image (594).png" alt=""><figcaption></figcaption></figure>



10. 하단으로 내려 보안탭 클릭, 보안그룹 이름 클릭

<figure><img src="../../.gitbook/assets/image (595).png" alt=""><figcaption></figcaption></figure>



11. 인바운드 규칙 클릭 인바운드 규칙 편집 클릭

<figure><img src="../../.gitbook/assets/image (596).png" alt=""><figcaption></figcaption></figure>





12. 규칙 추가 클릭후 아래와 같이 설정 한 뒤 규칙 저장 클릭

* 유형 : 사용자 지정 TCP&#x20;
* 포트 범위 : 8080
* 소스 : Anywhere-IPv4

<figure><img src="../../.gitbook/assets/image (597).png" alt=""><figcaption></figcaption></figure>




# Task 1 - Workspace 구성

1. 아래 링크로 접속하여 제공받은 계정으로 로그인

{% embed url="https://console.aws.amazon.com/" %}

<figure><img src="../../.gitbook/assets/image (571).png" alt=""><figcaption></figcaption></figure>

2. 우측 상단 지역을 클릭하고 서울 클릭

<figure><img src="https://psedu.gitbook.io/oneday-kubernetes-essential/~gitbook/image?url=https%3A%2F%2F463162893-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FxHZJVIhAgJaXSAdsgazr%252Fuploads%252Fgit-blob-3b7b375f90b1eba22d19120ca2d428c58325bded%252Fimage-7.png%3Falt%3Dmedia&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=6c8e2fed&#x26;sv=2" alt=""><figcaption></figcaption></figure>





3. EC2 서비스로 이동하여 아래와 같이 인스턴스 생성

* 이름 : user\*\*-k8s
* 애플리케이션 및 OS 이미지 : Ubuntu Server 24.04 LTS
* 인스턴스 유형 : t3.medium
* 키페어 : k8skey.pem 선택
* 스토리지 구성 : 20GiB gp3 타입 선택
* 인스턴스 개수 : 3대
* 고급 세부 정보.사용자데이터 :&#x20;

```
#!/bin/bash
set -eux
exec > >(tee /var/log/k8s-userdata.log | logger -t k8s-userdata) 2>&1
echo "=== K8s 1.33 bootstrap start: $(date) ==="

# ────────────────────────────────
# 1. 기본 설정
# ────────────────────────────────
swapoff -a
sed -i '/ swap / s/^/#/' /etc/fstab
ufw disable || true

apt-get update -y
apt-get install -y \
  curl ethtool ebtables socat conntrack \
  apt-transport-https ca-certificates gnupg lsb-release

# ────────────────────────────────
# 2. 커널 모듈 & sysctl
# ────────────────────────────────
cat <<EOF > /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF

modprobe overlay
modprobe br_netfilter

cat <<EOF > /etc/sysctl.d/99-kubernetes-cri.conf
net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF

sysctl --system

# ────────────────────────────────
# 3. containerd v1.7.27
# ────────────────────────────────
CONTAINERD_VERSION="1.7.27"

curl -fsSL "https://github.com/containerd/containerd/releases/download/v${CONTAINERD_VERSION}/containerd-${CONTAINERD_VERSION}-linux-amd64.tar.gz" \
  | tar -C /usr/local -xz

# containerd systemd 서비스
curl -fsSL "https://raw.githubusercontent.com/containerd/containerd/main/containerd.service" \
  -o /etc/systemd/system/containerd.service

# runc v1.2.6
curl -fsSL "https://github.com/opencontainers/runc/releases/download/v1.2.6/runc.amd64" \
  -o /usr/local/sbin/runc
chmod +x /usr/local/sbin/runc

# containerd 설정
mkdir -p /etc/containerd
containerd config default > /etc/containerd/config.toml
sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/containerd/config.toml

systemctl daemon-reload
systemctl enable --now containerd

# ────────────────────────────────
# 4. CNI plugins v1.5.1
# ────────────────────────────────
CNI_VERSION="v1.5.1"
mkdir -p /opt/cni/bin
curl -fsSL "https://github.com/containernetworking/plugins/releases/download/${CNI_VERSION}/cni-plugins-linux-amd64-${CNI_VERSION}.tgz" \
  | tar -C /opt/cni/bin -xz

# ────────────────────────────────
# 5. crictl v1.33.0
# ────────────────────────────────
CRICTL_VERSION="v1.33.0"
curl -fsSL "https://github.com/kubernetes-sigs/cri-tools/releases/download/${CRICTL_VERSION}/crictl-${CRICTL_VERSION}-linux-amd64.tar.gz" \
  | tar -C /usr/local/bin -xz

# crictl 설정
cat <<EOF > /etc/crictl.yaml
runtime-endpoint: unix:///run/containerd/containerd.sock
image-endpoint: unix:///run/containerd/containerd.sock
timeout: 10
EOF

# ────────────────────────────────
# 6. kubeadm, kubelet, kubectl v1.33.0
# ────────────────────────────────
K8S_VERSION="v1.33.0"

curl -fsSL --remote-name-all \
  "https://dl.k8s.io/release/${K8S_VERSION}/bin/linux/amd64/kubeadm" \
  "https://dl.k8s.io/release/${K8S_VERSION}/bin/linux/amd64/kubelet" \
  "https://dl.k8s.io/release/${K8S_VERSION}/bin/linux/amd64/kubectl"

install -o root -g root -m 0755 kubeadm kubelet kubectl /usr/local/bin/
rm -f kubeadm kubelet kubectl

# kubelet systemd 서비스
RELEASE_VERSION="v0.16.2"
curl -sSL "https://raw.githubusercontent.com/kubernetes/release/${RELEASE_VERSION}/cmd/kubepkg/templates/latest/deb/kubelet/lib/systemd/system/kubelet.service" \
  | sed "s:/usr/bin:/usr/local/bin:g" \
  > /etc/systemd/system/kubelet.service

mkdir -p /etc/systemd/system/kubelet.service.d
curl -sSL "https://raw.githubusercontent.com/kubernetes/release/${RELEASE_VERSION}/cmd/kubepkg/templates/latest/deb/kubeadm/10-kubeadm.conf" \
  | sed "s:/usr/bin:/usr/local/bin:g" \
  > /etc/systemd/system/kubelet.service.d/10-kubeadm.conf

systemctl daemon-reload
systemctl enable --now kubelet
```





4. ec2 인스턴스의 이름을 수정

<figure><img src="../../.gitbook/assets/image (580).png" alt=""><figcaption></figcaption></figure>



5. Cloudshell 을 실행한 뒤, 제공받은 KeyPair 파일을 업로드

<figure><img src="../../.gitbook/assets/image (581).png" alt=""><figcaption></figcaption></figure>





6. key 파일의 권한을 변경합니다.

```
chmod 600 k8skey.pem
```

<figure><img src="../../.gitbook/assets/image (582).png" alt=""><figcaption></figcaption></figure>



7. 아래 명령으로 CP 노드에 접속

```
ssh -i k8skey.pem ubuntu@<CP-IP>
```





8. \+ 버튼을 클릭, 리전명을 클릭하여 터미널을 새로 오픈

<figure><img src="../../.gitbook/assets/image (583).png" alt=""><figcaption></figcaption></figure>







9. 아래 명령으로 worker 1 노드에 접속

```
ssh -i k8skey.pem ubuntu@<worker1-IP>
```





10. 반복하여 worker 2 노드에도 접속


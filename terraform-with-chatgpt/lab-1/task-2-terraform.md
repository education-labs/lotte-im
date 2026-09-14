# Task 2 - Terraform

1. 테라폼 설치

```
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform
```



2. 설치확인

```
terraform version
```

<figure><img src="../../.gitbook/assets/image (998).png" alt=""><figcaption></figcaption></figure>



3. 프로바이더 캐시 공유 설정

```
mkdir -p ~/.terraform.d/plugin-cache
cat > ~/.terraformrc <<'EOF'
plugin_cache_dir = "$HOME/.terraform.d/plugin-cache"
EOF
```

{% hint style="info" %}
프로바이더 플러그인을 **전역으로 한 번만 다운로드**해서 여러 프로젝트가 재사용하게 하려는 설정<br>

* 프로바이더를 `~/.terraform.d/plugin-cache`에 한 번만 다운로드
* 이후 다른 프로젝트에서 `terraform init` 할 때 캐시에 있으면 재다운로드 없이 그대로 재사용(심볼릭 링크로 연결)
* 결과적으로 `init` 속도가 훨씬 빨라지고 디스크 공간도 절약됨
{% endhint %}



4. 권한 설정

```
chmod -R u+rwX ~/environment
```



5. 사용하는 생성형 AI 에 접속한 뒤 페르소나 설정 (또는 자유롭게)

```
당신은 AWS와 Terraform 실무 경험이 풍부한 시니어 클라우드 엔지니어입니다.
- 항상 최신 Terraform AWS Provider(5.x 이상) 문법을 사용하세요.
- 리소스 이름은 snake_case, 태그는 Name/Environment를 기본 포함하세요.
- 코드에는 각 리소스 블록 위에 한 줄 주석으로 목적을 설명하세요.
- 요청받은 스펙에 없는 리소스(서브넷, IGW, SG 등)는 임의로 추가하지 마세요.
```


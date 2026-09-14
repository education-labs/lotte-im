# Task 1 - GenAI 기반 Terraform  코드 생성

1. 디렉토리 생성

```
mkdir -p ~/environment/ec2/modules/vpc
mkdir -p ~/environment/ec2/modules/ec2-instance
cd ~/environment/ec2
```



2. 다음 조건을 모두 만족하는 VPC 모듈 내용 구성

{% hint style="success" %}
VPC 모듈
{% endhint %}

<table><thead><tr><th width="176.18182373046875">리소스</th><th>요구사항</th></tr></thead><tbody><tr><td>VPC</td><td>기본 CIDR <code>10.0.0.0/16</code>, DNS Support/Hostnames <code>true</code>, Name 태그 <code>${prefix}-vpc</code></td></tr><tr><td>VPC 보조 CIDR</td><td>보조 CIDR <code>10.1.0.0/16</code>을 동일 VPC에 추가 연결</td></tr><tr><td>서브넷 5개</td><td>퍼블릭 2개(<code>10.0.1.0/24</code>, <code>10.0.2.0/24</code>), 프라이빗 2개(<code>10.0.11.0/24</code>, <code>10.0.12.0/24</code>), 보조 1개(<code>10.1.1.0/24</code>) </td></tr><tr><td>Internet Gateway</td><td>VPC에 연결, Name 태그 <code>${prefix}-igw</code></td></tr><tr><td>Public Route Table</td><td><code>0.0.0.0/0 → IGW</code>, 퍼블릭 서브넷 2개와 연결</td></tr><tr><td>Private Route Table</td><td><code>0.0.0.0/0 → NAT Gateway</code>, 프라이빗 서브넷 2개 + 보조 서브넷 1개와 연결</td></tr><tr><td>EIP + NAT Gateway</td><td><code>domain = "vpc"</code> EIP를 발급해 <code>public-a</code> 서브넷의 NAT Gateway에 연결, IGW 생성 후 생성되도록 <code>depends_on</code> 명시</td></tr><tr><td>Web/App/DB SG</td><td>Web은 80/443 전체 허용·22는 관리자 IP만, App은 8080/22를 Web SG에서만, DB는 3306을 App SG에서만</td></tr></tbody></table>



모듈 변수

<table><thead><tr><th width="213.27276611328125">변수</th><th>설명</th></tr></thead><tbody><tr><td><code>prefix</code></td><td>리소스 이름 접두사 (예: <code>user##</code>)</td></tr><tr><td><code>admin_ip_cidr</code></td><td>Web SG의 SSH 인바운드를 허용할 본인 IP(<code>/32</code>)</td></tr></tbody></table>









3. 다음 조건을 모두 만족하는 instance 모듈 내용 구성

{% hint style="success" %}
instance 모듈
{% endhint %}

<table><thead><tr><th width="169.6363525390625">리소스</th><th>요구사항</th></tr></thead><tbody><tr><td>EC2 인스턴스 1개</td><td>AMI, 인스턴스 타입, 서브넷, 보안그룹, 퍼블릭 IP 여부, <code>user_data</code>를 변수로 입력받아 생성</td></tr></tbody></table>



모듈 변수

<table><thead><tr><th width="203.81817626953125">변수</th><th>설명</th></tr></thead><tbody><tr><td><code>name</code></td><td>인스턴스 Name 태그</td></tr><tr><td><code>ami_id</code></td><td>AMI ID (루트에서 <code>data "aws_ami"</code>로 조회해 전달)</td></tr><tr><td><code>instance_type</code></td><td>기본값 <code>t3.micro</code></td></tr><tr><td><code>subnet_id</code></td><td>배치할 서브넷 ID</td></tr><tr><td><code>security_group_ids</code></td><td>붙일 보안그룹 ID 리스트</td></tr><tr><td><code>associate_public_ip</code></td><td>퍼블릭 IP 자동 할당 여부</td></tr><tr><td><code>user_data</code></td><td>부팅 스크립트(선택)</td></tr><tr><td><code>tags</code></td><td>추가 태그(<code>Environment</code>, <code>Tier</code> 등)</td></tr></tbody></table>



\
4\. 생성형 AI에게 질의하여 코드를 생성

{% hint style="warning" %}
**주의:** root 모듈 파일들도 생성 요청하여야합니다.
{% endhint %}



5. 생성한 코드를 C9의 파일로 생성

<figure><img src="../../.gitbook/assets/image (1000).png" alt=""><figcaption></figcaption></figure>



6. 코드 내용을 넣고, Ctrl + S 키를 입력

<figure><img src="../../.gitbook/assets/image (1001).png" alt=""><figcaption></figcaption></figure>



7. 파일명을 임의로 지정하고, ec2 디렉토리를 선택한 뒤 Save 클릭


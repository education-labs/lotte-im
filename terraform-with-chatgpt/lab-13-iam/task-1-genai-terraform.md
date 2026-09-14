# Task 1 - GenAI 기반 Terraform  코드 생성

1. 디렉토리 생성

```
mkdir -p ~/environment/iam/modules/vpc
cd ~/environment/iam
```

{% hint style="danger" %}
**이번 Lab은 Lab8의 S3 버킷이 실제로 존재해야 동작합니다.** 새 EC2 인스턴스에 "Lab8 버킷만 읽을 수 있는" 권한을 IAM으로 부여하는 것이 이번 Lab의 핵심이므로, Lab8 버킷이 destroy된 상태라면 먼저 다시 `apply`해두세요.
{% endhint %}

{% hint style="info" %}
사전 구성 리소스

없음(독립 폴더). 인스턴스 접속 확인용으로 최소한의 VPC만 새로 만듭니다.
{% endhint %}

2. 다음 조건을 모두 만족하는 VPC 모듈 구성

{% hint style="success" %}
VPC 모듈
{% endhint %}

<table><thead><tr><th width="176">리소스</th><th>요구사항</th></tr></thead><tbody><tr><td>VPC</td><td>기본 CIDR <code>10.0.0.0/16</code></td></tr><tr><td>퍼블릭 서브넷 1개</td><td><code>10.0.1.0/24</code>(2a)</td></tr><tr><td>IGW + Public RT</td><td>인터넷 아웃바운드</td></tr><tr><td>SG</td><td>TCP 22 ← 본인 IP(<code>/32</code>)만</td></tr></tbody></table>

모듈 입력: `prefix`, `admin_ip_cidr` / 모듈 출력: `vpc_id`, `subnet_id`, `sg_id`

3. 다음 조건을 모두 만족하는 IAM 역할/정책을 구성

{% hint style="info" icon="label" %}
* EC2가 이 역할을 위임(assume)할 수 있어야 한다(신뢰 정책의 `Principal`은 `ec2.amazonaws.com`).
* 정책은 AWS 관리형 정책(`AmazonS3FullAccess` 등)을 붙이지 않고, **직접 작성한 커스텀 정책**만 사용한다.
* 커스텀 정책은 Lab8에서 만든 버킷(`user##-assets-bucket-<account_id>`)에 대해서만 `s3:GetObject`, `s3:ListBucket`을 `Allow`한다. 다른 버킷에는 어떤 권한도 주지 않는다.
* 같은 정책 안에 같은 버킷에 대한 `s3:PutObject`, `s3:DeleteObject`, `s3:DeleteBucket*`을 명시적으로 `Deny`하는 Statement도 함께 넣는다(권한을 아예 안 주는 것 + 명시적으로 막는 것을 이중으로).
* 이 역할을 인스턴스 프로파일로 감싸서 EC2에 붙일 수 있게 한다.
{% endhint %}

{% hint style="success" %}
상세스펙
{% endhint %}

<table><thead><tr><th width="200">항목</th><th>요구사항</th></tr></thead><tbody><tr><td>IAM Role</td><td>이름 <code>user##-ec2-s3-readonly-role</code>, 신뢰 주체 <code>ec2.amazonaws.com</code></td></tr><tr><td>IAM Policy</td><td>이름 <code>user##-s3-readonly-policy</code>, Lab8 버킷 ARN 대상 Allow(GetObject, ListBucket) + Deny(PutObject, DeleteObject, DeleteBucket*)</td></tr><tr><td>Instance Profile</td><td>이름 <code>user##-ec2-s3-readonly-profile</code></td></tr><tr><td>EC2 인스턴스</td><td>이름 <code>user##-iam-test</code>, t3.micro, 퍼블릭 서브넷, 위 인스턴스 프로파일 연결, S3 자격증명은 코드/user_data 어디에도 하드코딩하지 않음</td></tr><tr><td>태그</td><td><code>Name</code>, <code>Environment=lab</code></td></tr><tr><td>output</td><td><code>role_arn</code>, <code>instance_profile_name</code>, <code>instance_id</code></td></tr></tbody></table>

4. 생성형 AI에게 질의하여 코드를 생성
5. 생성한 코드를 C9의 파일로 생성
6. 코드 내용을 넣고, Ctrl + S 키를 입력
7. 파일명을 임의로 지정하고, **iam 디렉토리를 선택**한 뒤 Save 클릭

{% hint style="info" %}
코드 내 user## 가 있다면 ## 에는 유저넘버를 입력합니다.
{% endhint %}


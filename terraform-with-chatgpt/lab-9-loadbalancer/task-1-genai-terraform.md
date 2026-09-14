# Task 1 - GenAI 기반 Terraform  코드 생성

## Task 1 - GenAI 기반 Terraform 코드 생성

1. 디렉토리 생성

```
mkdir -p ~/environment/lb/modules/vpc
mkdir -p ~/environment/lb/modules/ec2-instance
cd ~/environment/lb
```

{% hint style="danger" %}
**이번 Lab은 Lab8의 S3 버킷이 실제로 존재해야 동작합니다.** 이미지를 다시 업로드하지 않고, Lab8에서 만든 버킷을 `data` 소스로 조회해서 그대로 씁니다. Lab8을 destroy했다면 먼저 다시 `apply`해서 버킷과 `banner.jpg`가 존재하는 상태로 만들어두세요.
{% endhint %}

{% hint style="info" %}
사전 구성 리소스

이번 Lab은 프라이빗 서브넷·NAT Gateway가 필요 없습니다. 웹 인스턴스 3대가 모두 퍼블릭 서브넷에 있고 ALB가 외부 트래픽을 분산하는 구조라, VPC 모듈은 퍼블릭 서브넷 2개 + ALB SG + Web SG만 있으면 됩니다.<br>
{% endhint %}





2. 다음 조건을 모두 만족하는 VPC 모듈 구성

{% hint style="success" %}
VPC 모듈
{% endhint %}

<table><thead><tr><th width="176">리소스</th><th>요구사항</th></tr></thead><tbody><tr><td>VPC</td><td>기본 CIDR <code>10.0.0.0/16</code>, DNS Support/Hostnames <code>true</code>, Name 태그 <code>${prefix}-vpc</code></td></tr><tr><td>퍼블릭 서브넷 2개</td><td><code>10.0.1.0/24</code>(2a), <code>10.0.2.0/24</code>(2c), <code>map_public_ip_on_launch=true</code></td></tr><tr><td>Internet Gateway</td><td>VPC에 연결</td></tr><tr><td>Public Route Table</td><td><code>0.0.0.0/0 → IGW</code>, 퍼블릭 서브넷 2개와 연결</td></tr><tr><td>ALB SG</td><td>TCP 80 ← <code>0.0.0.0/0</code></td></tr><tr><td>Web SG</td><td>TCP 80 ← ALB SG만 허용(CIDR 아님) / TCP 22 ← 본인 IP(<code>/32</code>)</td></tr></tbody></table>

**모듈 변수**: `prefix`, `admin_ip_cidr`&#x20;

**모듈 출력**: `vpc_id`, `subnet_ids`, `alb_sg_id`, `web_sg_id`&#x20;





3. S3 이미지 버킷 — **새로 만들지 않고 Lab8 버킷을 조회**

{% hint style="warning" %}
**"S3 이미지 버킷"이라고만 요청하면 ChatGPT가 습관적으로 `resource "aws_s3_bucket"`을 새로 만들어버립니다.** 반드시 "버킷을 새로 만들지 말고 `data "aws_s3_bucket"`으로 기존 버킷을 조회해줘"라고 명시하세요. 이 부분을 빠뜨리면 Lab8과 이름이 겹쳐 `BucketAlreadyOwnedByYou` 오류가 나거나, 이름을 다르게 지어 엉뚱한 빈 버킷을 새로 만들게 됩니다.
{% endhint %}

<table><thead><tr><th width="176">항목</th><th>요구사항</th></tr></thead><tbody><tr><td>조회 방식</td><td><code>data "aws_s3_bucket" "assets"</code>로 Lab8과 동일한 이름(<code>user##-assets-bucket-&#x3C;account_id></code>)의 버킷을 조회</td></tr><tr><td>이미지 URL</td><td>조회한 버킷의 <code>bucket_regional_domain_name</code> + <code>banner.jpg</code>로 구성</td></tr><tr><td>생성 리소스</td><td>없음 (버킷·객체·정책 모두 Lab8이 이미 만들어둔 것을 그대로 사용)</td></tr></tbody></table>

4. `modules/ec2-instance` 및 핵심 목표 리소스(ALB + Target Group + EC2 3대) 구성

{% hint style="success" %}
instance 모듈
{% endhint %}

EC2 인스턴스 1개를 AMI, 인스턴스 타입, 서브넷, 보안그룹, 퍼블릭 IP 여부, `user_data`를 변수로 입력받아 생성하는 범용 모듈로 작성합니다.

* `module.ec2-instance`를 **`for_each`로 3번 호출**해서 웹서버 3대를 만든다(2개 AZ에 분산).
* 각 인스턴스는 `user_data`로 httpd를 설치하고, 호스트네임과 S3 이미지(`banner.jpg`)가 함께 보이는 `index.html`을 생성한다.
* Target Group(HTTP:80, health check path `/`, matcher `200`)을 만들고 3대를 모두 등록(attachment)한다.
* ALB를 퍼블릭 서브넷 2개에 걸쳐 만들고, ALB SG를 붙인다.
* 리스너(포트 80)로 들어온 요청을 Target Group으로 전달(forward)한다.

{% hint style="success" %}
상세스펙
{% endhint %}

<table><thead><tr><th width="140">인스턴스</th><th>서브넷</th><th>Name 태그</th></tr></thead><tbody><tr><td>web-1</td><td>public-a</td><td>user##-web-1</td></tr><tr><td>web-2</td><td>public-c</td><td>user##-web-2</td></tr><tr><td>web-3</td><td>public-a</td><td>user##-web-3</td></tr></tbody></table>

<table><thead><tr><th width="176">리소스</th><th>요구사항</th></tr></thead><tbody><tr><td>Target Group</td><td>이름 <code>user##-web-tg</code>, 포트 80, HTTP, health check path <code>/</code>, matcher <code>200</code></td></tr><tr><td>ALB</td><td>이름 <code>user##-alb</code>, <code>internal=false</code>, 퍼블릭 서브넷 2개, ALB SG 연결</td></tr><tr><td>Listener</td><td>포트 80, HTTP, 기본 액션 forward</td></tr></tbody></table>

5. 생성형 AI에게 질의하여 코드를 생성

{% hint style="warning" %}
**주의:** 사전 조건 리소스도 생성하게 요청해야합니다.
{% endhint %}

6. 생성한 코드를 C9의 파일로 생성
7. 코드 내용을 넣고, Ctrl + S 키를 입력
8. 파일명을 임의로 지정하고, **lb 디렉토리를 선택**한 뒤 Save 클릭

{% hint style="info" %}
코드 내 user## 가 있다면 ## 에는 유저넘버를 입력합니다.
{% endhint %}




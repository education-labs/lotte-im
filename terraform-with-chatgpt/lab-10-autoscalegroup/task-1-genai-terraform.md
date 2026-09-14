# Task 1 - GenAI 기반 Terraform  코드 생성

1. 디렉토리 생성&#x20;

```
mkdir -p ~/environment/asg/modules/vpc
cd ~/environment/asg
```

{% hint style="danger" %}
**이번 Lab도 Lab8의 S3 버킷이 실제로 존재해야 동작합니다.** Lab9와 마찬가지로 이미지를 다시 업로드하지 않고 `data` 소스로 Lab8의 버킷을 조회합니다. Lab8을 destroy했다면 먼저 다시 `apply`해서 버킷과 `banner.jpg`를 살려두세요.
{% endhint %}

2. 다음 조건을 모두 만족하는 VPC 모듈 구성 (Lab9와 동일한 트리밍 버전)

{% hint style="success" %}
VPC 모듈
{% endhint %}

<table><thead><tr><th width="176">리소스</th><th>요구사항</th></tr></thead><tbody><tr><td>VPC</td><td>기본 CIDR <code>10.0.0.0/16</code></td></tr><tr><td>퍼블릭 서브넷 2개</td><td><code>10.0.1.0/24</code>(2a), <code>10.0.2.0/24</code>(2c)</td></tr><tr><td>IGW + Public RT</td><td>인터넷 아웃바운드</td></tr><tr><td>ALB SG</td><td>TCP 80 ← <code>0.0.0.0/0</code></td></tr><tr><td>Web SG</td><td>TCP 80 ← ALB SG만 / TCP 22 ← 본인 IP</td></tr></tbody></table>

모듈 입력: `prefix`, `admin_ip_cidr` / 모듈 출력: `vpc_id`, `subnet_ids`, `alb_sg_id`, `web_sg_id`

프라이빗 서브넷·NAT는 이번에도 필요 없습니다.

3. S3 이미지 버킷 — **새로 만들지 않고 Lab8 버킷을 조회**

{% hint style="warning" %}
"S3 이미지 버킷"이라고만 요청하면 ChatGPT가 새 버킷을 만들어버리는 경우가 많습니다. 반드시 "버킷을 새로 만들지 말고 `data "aws_s3_bucket"`으로 기존 버킷을 조회해줘"라고 명시하세요.
{% endhint %}

<table><thead><tr><th width="176">항목</th><th>요구사항</th></tr></thead><tbody><tr><td>조회 방식</td><td><code>data "aws_s3_bucket" "assets"</code>로 Lab8과 동일한 이름의 버킷을 조회</td></tr><tr><td>이미지 URL</td><td>조회한 버킷의 <code>bucket_regional_domain_name</code> + <code>banner.jpg</code>로 구성</td></tr><tr><td>생성 리소스</td><td>없음</td></tr></tbody></table>

4. Launch Template 구성

{% hint style="info" %}
Lab9에서는 EC2를 `module.ec2-instance`로 3대 직접 만들었습니다. 이번 Lab에서는 그 자리를 **Auto Scaling Group**으로 대체하므로, `modules/ec2-instance`는 아예 쓰지 않습니다 — ASG가 **Launch Template**을 기반으로 인스턴스를 직접 생성·교체·삭제합니다.
{% endhint %}

<table><thead><tr><th width="176">항목</th><th>요구사항</th></tr></thead><tbody><tr><td>이름</td><td>user##-web-lt</td></tr><tr><td>AMI</td><td><code>data "aws_ami"</code>로 조회한 최신 Amazon Linux</td></tr><tr><td>인스턴스 타입</td><td>t3.micro</td></tr><tr><td>보안그룹</td><td>web_sg_id</td></tr><tr><td>user_data</td><td>httpd 설치 + S3 이미지 표시 스크립트를 <code>base64encode()</code>로 인코딩해서 전달</td></tr></tbody></table>

5. ALB + Target Group + Listener 구성

<table><thead><tr><th width="176">리소스</th><th>요구사항</th></tr></thead><tbody><tr><td>ALB</td><td>이름 user##-asg-alb, 퍼블릭 서브넷 2개, ALB SG</td></tr><tr><td>Target Group</td><td>이름 user##-asg-tg, 포트 80, HTTP, health check path /, matcher 200</td></tr><tr><td>Listener</td><td>포트 80, Target Group으로 forward</td></tr></tbody></table>

{% hint style="info" %}
Lab9와 달리 이번 Lab에는 `aws_lb_target_group_attachment` 리소스가 **없습니다.** ASG가 인스턴스를 만들고 없앨 때마다 Target Group 등록/해제까지 자동으로 처리하기 때문입니다.
{% endhint %}

6. Auto Scaling Group + Scaling Policy 구성 — **핵심 목표 리소스**

{% hint style="info" icon="label" %}
* `min_size=2`, `max_size=4`, `desired_capacity=2`로 시작한다.
* 서로 다른 AZ의 퍼블릭 서브넷 2개(`vpc_zone_identifier`)에 걸쳐 인스턴스를 배치한다.
* Launch Template의 최신 버전(`$Latest`)을 사용한다.
* `target_group_arns`로 위 Target Group에 연결해서, 인스턴스가 뜨고 내려갈 때 자동으로 등록/해제되게 한다.
* \*\*`health_check_type = "ELB"`\*\*로 설정해서, EC2 자체 상태뿐 아니라 ALB의 헬스체크 결과까지 반영해 인스턴스를 교체하게 한다.
* `health_check_grace_period`를 60초로 두어, 부팅 직후 httpd가 뜨기 전에 성급하게 unhealthy 판정하지 않게 한다.
* `Name`, `Environment` 태그가 인스턴스에도 전파(`propagate_at_launch = true`)되게 한다.
* CPU 평균 사용률 50%를 목표로 하는 Target Tracking 스케일링 정책을 하나 붙인다.
{% endhint %}

{% hint style="success" %}
상세스펙
{% endhint %}

<table><thead><tr><th width="176">항목</th><th>요구사항</th></tr></thead><tbody><tr><td>ASG 이름</td><td>user##-web-asg</td></tr><tr><td>min / max / desired</td><td>2 / 4 / 2</td></tr><tr><td>Health Check</td><td><code>type=ELB</code>, <code>grace_period=60</code></td></tr><tr><td>Scaling Policy</td><td><code>TargetTrackingScaling</code>, <code>ASGAverageCPUUtilization</code>, <code>target_value=50</code></td></tr></tbody></table>

7. 생성형 AI에게 질의하여 코드를 생성

{% hint style="warning" %}
**주의:** 사전 조건 리소스도 생성하게 요청해야합니다.
{% endhint %}

8. 생성한 코드를 C9의 파일로 생성
9. 코드 내용을 넣고, Ctrl + S 키를 입력
10. 파일명을 임의로 지정하고, **asg 디렉토리를 선택**한 뒤 Save 클릭

{% hint style="info" %}
코드 내 user## 가 있다면 ## 에는 유저넘버를 입력합니다.
{% endhint %}

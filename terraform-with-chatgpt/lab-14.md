# Lab 14 - 통합실습

## TechMart HTTPS 쇼핑 데모 배포

### 0. 시나리오

TechMart는 신제품 배너 이미지를 보여주는 쇼핑 데모 사이트를 새로 배포하려 합니다. 요구사항은 다음과 같습니다.

* 고객은 ALB의 HTTPS 엔드포인트로 안전하게(암호화된 채널로) 접속할 수 있어야 한다(도메인이 없으므로 ALB 자체 DNS 이름 기준).
* 트래픽이 몰려도 웹서버가 자동으로 늘어나야 한다.
* 상품 배너 이미지는 누구나 볼 수 있어야 하지만, 그 외 버킷 조작(업로드/삭제)은 아무나 할 수 없어야 한다.
* 주문 데이터가 들어갈 데이터베이스는 외부에서 절대 직접 접근할 수 없어야 한다.
* 웹서버는 딱 필요한 S3 버킷 하나만 읽을 수 있어야 하고, 그 이상의 AWS 권한을 가지면 안 된다.



{% hint style="info" %}
실습 진행 시 시나리오 또는 구성의 변경을 자유롭게 하셔도 무방합니다.

* 미션을 완수하는게 목표가 아닙니다.

1. GenAI 에게 프롬프트를 전달하고 받은 응답을 이해&#x20;
2. Terraform 을 통해 AWS 인프라 환경을 보다 쉽게 구축할 수 있다는 경험
{% endhint %}



### 1. 사업 요건 → 실습 매핑

| 사업 요건         | 사용 기술                       | 관련 Lab      |
| ------------- | --------------------------- | ----------- |
| 암호화된 HTTPS 접속 | ACM(셀프사인 import)            | Lab12       |
| 트래픽 급증 자동 대응  | ALB + Auto Scaling Group    | Lab9, Lab10 |
| 배너 이미지 공개 서빙  | S3(퍼블릭 정책)                  | Lab8        |
| 계층 간 네트워크 분리  | VPC/Subnet/IGW/NAT/RT       | Lab2\~5     |
| 계층별 접근 통제     | Security Group 체인           | Lab6        |
| 재사용 가능한 코드 구조 | Terraform Module            | Lab7        |
| 안전한 데이터베이스    | RDS(프라이빗, 암호화)              | Lab11       |
| 서버 권한 최소화     | IAM Role + Instance Profile | Lab13       |

### 2. 최종 아키텍처 개요

지금까지의 Lab과 가장 크게 달라지는 지점: **ASG 웹서버가 더 이상 퍼블릭 서브넷에 있지 않습니다.** Lab9·Lab10에서는 간단히 웹서버를 퍼블릭 서브넷에 뒀지만, 실전 구성에서는 ALB만 퍼블릭에 두고 실제 서버는 프라이빗 서브넷에 넣어 외부에서 인스턴스에 직접 도달할 방법 자체를 없앱니다. 그래서 NAT Gateway(Lab5)가 다시 필요해집니다 — 프라이빗 서브넷의 웹서버가 패키지 설치나 S3 접근을 위해 나가는 트래픽을 보낼 경로가 있어야 하기 때문입니다.

<table><thead><tr><th width="149.36358642578125">계층</th><th width="384.09088134765625">구성 요소</th><th>위치</th></tr></thead><tbody><tr><td>진입점</td><td>ALB (443 HTTPS 셀프사인, 80→443 리다이렉트)</td><td>퍼블릭 서브넷</td></tr><tr><td>아웃바운드 경로</td><td>NAT Gateway</td><td>퍼블릭 서브넷</td></tr><tr><td>웹</td><td>ASG (Launch Template, IAM 인스턴스 프로파일)</td><td>프라이빗 서브넷</td></tr><tr><td>데이터</td><td>RDS (MySQL, 암호화, 퍼블릭 접근 불가)</td><td>DB 전용 프라이빗 서브넷</td></tr><tr><td>정적 자산</td><td>S3 버킷(배너 이미지, 퍼블릭 GetObject만 허용)</td><td>리전 단위(VPC 밖)</td></tr><tr><td>인증서</td><td>ACM(셀프사인 import, <code>tls</code> 프로바이더로 생성)</td><td>계정 단위(VPC 밖)</td></tr><tr><td>권한</td><td>IAM Role(웹서버 → S3 버킷 한정 읽기)</td><td>계정 단위</td></tr></tbody></table>

### 3. 폴더/모듈 구성 예시

```
capstone/
├── modules/
│   ├── network/       (Lab2~5: VPC, 3계층 서브넷, IGW, NAT, RT)
│   ├── security/       (Lab6: ALB/Web/DB SG 체인)
│   ├── storage/         (Lab8: S3 버킷 + 이미지)
│   ├── iam/                (Lab13: EC2 역할 + 인스턴스 프로파일)
│   ├── compute-asg/  (Lab7,9,10: Launch Template + ASG + ALB)
│   └── database/        (Lab11: RDS)
├── cert.tf                (Lab12: tls_self_signed_cert + ACM import, 루트에서 직접 처리)
├── main.tf                (모듈 호출)
├── variables.tf
└── outputs.tf
```

### 4. 단계별 진행 순서 (마일스톤)

{% hint style="warning" %}
한 번에 전체를 GenAI 에게 요청하지 마세요. 아래 순서대로 모듈 단위로 나눠서 요청하고, 앞 단계의 output을 다음 단계 프롬프트에 명시적으로 알려주는 방식이 훨씬 안정적입니다.
{% endhint %}

1. **네트워크 기반** — VPC + 퍼블릭 서브넷 2개 + 웹 전용 프라이빗 서브넷 2개 + DB 전용 프라이빗 서브넷 2개 + IGW + NAT Gateway + 라우팅 테이블 3종(Public/Web-Private/DB-Private)
2. **보안 계층** — ALB SG → Web SG(ALB SG만 허용) → DB SG(Web SG만 허용) 체인
3. **데이터 계층** — S3 버킷(배너 이미지) + RDS(프라이빗, 암호화) 먼저 준비
4. **권한 계층** — IAM Role/Policy/Instance Profile을 S3 버킷 ARN에 맞춰 정의(컴퓨트보다 먼저 만들어야 ASG Launch Template에 바로 연결 가능)
5. **컴퓨트 계층** — Launch Template(IAM 인스턴스 프로파일 연결) + ASG(웹 프라이빗 서브넷) + ALB(우선 HTTP 80만)
6. **HTTPS** — `tls_private_key`+`tls_self_signed_cert`로 셀프사인 인증서 생성 → `aws_acm_certificate`로 import → ALB에 443 리스너 추가, 80은 443으로 리다이렉트로 전환
7. **통합 검증** — 아래 7번 체크리스트로 전체 시스템 점검

### 5. 통합 스펙

#### 5-1. 네트워크

<table><thead><tr><th width="165.99993896484375">항목</th><th>요구사항</th></tr></thead><tbody><tr><td>VPC</td><td><code>10.0.0.0/16</code></td></tr><tr><td>퍼블릭 서브넷</td><td><code>10.0.1.0/24</code>(2a), <code>10.0.2.0/24</code>(2c) — ALB, NAT GW</td></tr><tr><td>웹 프라이빗 서브넷</td><td><code>10.0.11.0/24</code>(2a), <code>10.0.12.0/24</code>(2c) — ASG 인스턴스</td></tr><tr><td>DB 프라이빗 서브넷</td><td><code>10.0.21.0/24</code>(2a), <code>10.0.22.0/24</code>(2c) — RDS</td></tr><tr><td>NAT Gateway</td><td>퍼블릭 서브넷에 위치, EIP 연결</td></tr><tr><td>라우팅</td><td>Public RT(<code>0.0.0.0/0→IGW</code>), Web-Private RT(<code>0.0.0.0/0→NAT</code>), DB-Private RT(인터넷 라우팅 없음)</td></tr></tbody></table>

#### 5-2. 보안그룹

<table><thead><tr><th width="158.727294921875">SG</th><th>인바운드</th></tr></thead><tbody><tr><td>ALB SG</td><td>TCP 80, 443 ← <code>0.0.0.0/0</code></td></tr><tr><td>Web SG</td><td>TCP 80 ← ALB SG만</td></tr><tr><td>DB SG</td><td>TCP 3306 ← Web SG만</td></tr></tbody></table>

#### 5-3. S3 (Lab8과 동일한 설계)

버킷 이름 `user##-capstone-assets-<account_id>`, 버전 관리/암호화 활성화, ACL 차단·정책 공개만 허용, HTTPS 강제 + GetObject 공개 정책, `banner.jpg` 업로드.

#### 5-4. IAM (Lab13과 동일한 설계, 대상 버킷만 이번 버킷으로 교체)

Web SG가 붙는 EC2용 Role에 이번 `capstone` 버킷에 대한 `s3:GetObject`, `s3:ListBucket`만 Allow, 쓰기/삭제는 명시적 Deny.

#### 5-5. 컴퓨트 (Launch Template + ASG)

<table><thead><tr><th width="168.9090576171875">항목</th><th>요구사항</th></tr></thead><tbody><tr><td>Launch Template</td><td>AMI(<code>data "aws_ami"</code>), t3.micro, Web SG, <strong>IAM 인스턴스 프로파일 연결</strong>, user_data는 base64encode</td></tr><tr><td>배치</td><td>웹 프라이빗 서브넷 2개(<code>vpc_zone_identifier</code>), <strong>퍼블릭 IP 없음</strong></td></tr><tr><td>ASG</td><td>min 2 / max 4 / desired 2, <code>health_check_type=ELB</code>, <code>target_group_arns</code>로 자동 등록</td></tr><tr><td>Scaling Policy</td><td>TargetTrackingScaling, CPU 50%</td></tr></tbody></table>

#### 5-6. ALB + Listener

<table><thead><tr><th width="160.9090576171875">항목</th><th>요구사항</th></tr></thead><tbody><tr><td>ALB</td><td>퍼블릭 서브넷 2개, ALB SG</td></tr><tr><td>Target Group</td><td>HTTP 80, health check <code>/</code></td></tr><tr><td>443 리스너</td><td>셀프사인 ACM 인증서, Target Group으로 forward</td></tr><tr><td>80 리스너</td><td>443으로 301 리다이렉트</td></tr></tbody></table>

#### 5-7. RDS

<table><thead><tr><th width="144.90911865234375">항목</th><th>요구사항</th></tr></thead><tbody><tr><td>서브넷 그룹</td><td>DB 프라이빗 서브넷 2개</td></tr><tr><td>엔진</td><td>MySQL 8.0, <code>db.t3.micro</code></td></tr><tr><td>네트워크</td><td><code>publicly_accessible=false</code>, DB SG만 연결</td></tr><tr><td>암호화/백업</td><td><code>storage_encrypted=true</code>, <code>backup_retention_period=7</code></td></tr></tbody></table>

#### 5-8. HTTPS 인증서 (Lab12와 동일한 셀프사인 import 방식)

<table><thead><tr><th width="120.9090576171875">항목</th><th>요구사항</th></tr></thead><tbody><tr><td>개인키</td><td><code>tls_private_key</code>, RSA 2048</td></tr><tr><td>인증서</td><td><code>tls_self_signed_cert</code>, CN <code>user##.capstone.local</code>, 유효기간 8760시간</td></tr><tr><td>ACM 등록</td><td><code>aws_acm_certificate</code>를 <code>private_key</code>+<code>certificate_body</code> import 방식으로 생성 (DNS 검증 없음)</td></tr></tbody></table>

### 6. 생성형 AI 활용 팁

* 모듈 하나를 요청할 때마다 "이 모듈은 다른 모듈의 output(예: `module.network.web_subnet_ids`)을 입력으로 받는다"고 명시하세요.
* "지금까지 만든 코드 전체를 보여줄 테니, 이 모듈만 이어서 작성해줘" 식으로 이전 산출물을 컨텍스트로 준 뒤 이어가면 일관성이 훨씬 좋아집니다.
* 네트워크·보안·데이터·권한을 컴퓨트보다 먼저 만들어야 나중 단계에서 output을 참조하기 쉽습니다.
* ACM 인증서는 "요청(request)"이 아니라 "가져오기(import)"라고 명확히 말해야 ChatGPT가 도메인 검증 코드를 끼워 넣지 않습니다.

### 7. 통합 검증 체크리스트

<table><thead><tr><th width="59.18182373046875">#</th><th width="168.72723388671875">확인 항목</th><th>확인 방법</th></tr></thead><tbody><tr><td>1</td><td>HTTPS 접속</td><td><code>curl -sk -o /dev/null -w "%{http_code}\n" https://$(terraform output -raw alb_dns_name)</code> → 200</td></tr><tr><td>2</td><td>HTTP→HTTPS 리다이렉트</td><td><code>curl -sI http://$(terraform output -raw alb_dns_name)</code> → 301</td></tr><tr><td>3</td><td>셀프사인 인증서 확인</td><td><code>openssl s_client</code>로 subject/issuer 동일한지 확인</td></tr><tr><td>4</td><td>ASG 인스턴스에 퍼블릭 IP 없음</td><td><code>aws ec2 describe-instances</code>로 <code>PublicIpAddress</code> 없음 확인</td></tr><tr><td>5</td><td>ALB를 거치지 않은 직접 접속 불가</td><td>프라이빗 IP는 VPC 밖에서 애초에 라우팅되지 않음</td></tr><tr><td>6</td><td>웹서버가 S3 이미지에 접근 가능</td><td>페이지에 배너 이미지 정상 표시</td></tr><tr><td>7</td><td>웹서버 IAM 역할이 다른 버킷은 접근 불가</td><td><code>aws iam simulate-principal-policy</code>로 확인</td></tr><tr><td>8</td><td>RDS 퍼블릭 접근 불가</td><td><code>aws rds describe-db-instances</code>의 <code>PubliclyAccessible=False</code></td></tr><tr><td>9</td><td>DB SG가 Web SG만 허용</td><td><code>aws ec2 describe-security-groups</code>로 소스 확인</td></tr><tr><td>10</td><td>강제 스케일 아웃 동작</td><td><code>set-desired-capacity 3</code> 후 Target Group에 3대 healthy</td></tr></tbody></table>

### 8. 정리(destroy) 순서 및 주의사항

하나의 프로젝트로 묶여 있으므로 `terraform destroy -auto-approve` 한 번으로 대부분 정리됩니다.

* S3 버킷 안에 객체가 남아있으면 삭제가 실패할 수 있습니다 — `banner.jpg`가 `aws_s3_object`로 관리되고 있다면 자동으로 같이 삭제됩니다.
* 셀프사인 인증서/키는 `tls` 프로바이더 리소스라 AWS 과금과 무관하며, destroy 시 다른 리소스와 함께 정리됩니다.

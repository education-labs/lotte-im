# Task 1 - GenAI 기반 Terraform  코드 생성



1. 디렉토리 생성

```
mkdir -p ~/environment/rds/modules/vpc
cd ~/environment/rds
```

{% hint style="info" %}
사전 구성 리소스

없음. 이번 Lab도 독립 폴더로 진행하며, 필요한 VPC/서브넷/보안그룹은 이 폴더 안에 새로 작성합니다.
{% endhint %}

2. 다음 조건을 모두 만족하는 VPC 모듈 구성

{% hint style="success" %}
VPC 모듈
{% endhint %}

<table><thead><tr><th width="176">리소스</th><th>요구사항</th></tr></thead><tbody><tr><td>VPC</td><td>기본 CIDR <code>10.0.0.0/16</code>, Name 태그 <code>${prefix}-vpc</code></td></tr><tr><td>프라이빗 서브넷 2개</td><td><code>10.0.11.0/24</code>(2a), <code>10.0.12.0/24</code>(2c) — 서로 다른 AZ</td></tr><tr><td>DB SG</td><td>TCP 3306 ← VPC 자체 CIDR(<code>10.0.0.0/16</code>)만 허용, 인터넷 전체(<code>0.0.0.0/0</code>) 금지</td></tr></tbody></table>

모듈 입력: `prefix` / 모듈 출력: `vpc_id`, `private_subnet_ids`, `db_sg_id`

{% hint style="info" %}
이번 Lab은 인터넷 게이트웨이나 NAT Gateway가 필요 없습니다. RDS는 애초에 인터넷과 직접 통신할 이유가 없는 리소스이므로, 프라이빗 서브넷과 최소한의 보안그룹만 있으면 됩니다.
{% endhint %}

3. 다음 조건을 모두 만족하는 RDS 인스턴스를 구성

{% hint style="info" icon="label" %}
* DB 서브넷 그룹은 서로 다른 AZ의 프라이빗 서브넷 2개를 포함해야 한다.
* 파라미터 그룹은 기본(default) 그룹을 그대로 쓰지 않고, MySQL 8.0 계열의 커스텀 파라미터 그룹을 새로 만들어 사용한다.
* DB 인스턴스는 **퍼블릭 접근이 불가능**해야 한다(`publicly_accessible = false`).
* 스토리지는 저장 시 암호화되어 있어야 한다.
* 마스터 비밀번호는 코드에 평문으로 하드코딩하지 않고, `sensitive = true`로 표시한 변수로 입력받는다.
* 자동 백업이 7일간 보관되도록 설정한다.
* 실습용이므로 삭제 보호는 꺼두고, 삭제 시 최종 스냅샷을 남기지 않도록 설정한다(`skip_final_snapshot = true`).
* Multi-AZ는 이번 Lab의 기본 스펙에서는 사용하지 않는다(비용 때문에 기본은 단일 AZ).
{% endhint %}

{% hint style="success" %}
상세스펙
{% endhint %}

<table><thead><tr><th width="240">항목</th><th>요구사항</th></tr></thead><tbody><tr><td>DB Subnet Group</td><td>이름 <code>user##-db-subnet-group</code>, 프라이빗 서브넷 2개</td></tr><tr><td>Parameter Group</td><td>이름 <code>user##-mysql-params</code>, family <code>mysql8.0</code>, <code>character_set_server=utf8mb4</code> 파라미터 변경</td></tr><tr><td>DB 식별자</td><td><code>user##-db</code></td></tr><tr><td>엔진</td><td><code>mysql</code>, 버전 <code>8.0</code></td></tr><tr><td>인스턴스 클래스</td><td><code>db.t3.micro</code></td></tr><tr><td>스토리지</td><td><code>20</code>GB, <code>gp3</code>, <code>storage_encrypted=true</code></td></tr><tr><td>네트워크</td><td><code>publicly_accessible=false</code>, 위 DB SG 연결</td></tr><tr><td>백업</td><td><code>backup_retention_period=7</code></td></tr><tr><td>정리 옵션</td><td><code>skip_final_snapshot=true</code>, <code>deletion_protection=false</code></td></tr><tr><td>비밀번호</td><td><code>var.db_password</code> (타입 <code>string</code>, <code>sensitive=true</code>, 기본값 없음)</td></tr><tr><td>태그</td><td><code>Name=user##-db</code>, <code>Environment=lab</code></td></tr><tr><td>output</td><td><code>db_endpoint</code>, <code>db_instance_id</code>, <code>engine_version</code></td></tr></tbody></table>

4. 생성형 AI에게 질의하여 코드를 생성

{% hint style="warning" %}
**주의:** "RDS 만들어줘"라고만 하면 ChatGPT가 테스트 편의를 위해 `publicly_accessible = true`로 만들거나, 보안그룹을 `0.0.0.0/0`으로 여는 경우가 있습니다. \*\*"이 DB는 인터넷에서 절대 접근되면 안 되고, VPC 내부 트래픽만 허용해야 한다"\*\*고 명확히 지정하세요.

비밀번호도 "변수로 입력받되 `sensitive = true`로 표시하고, 기본값 없이 반드시 실행 시점에 값을 주도록 해달라"고 명시하세요. 그냥 "비밀번호는 변수로 해줘"라고만 하면 코드에 기본값이 평문으로 박혀 나오는 경우가 있습니다.
{% endhint %}

5. 생성한 코드를 C9의 파일로 생성
6. 코드 내용을 넣고, Ctrl + S 키를 입력
7. 파일명을 임의로 지정하고, **rds 디렉토리를 선택**한 뒤 Save 클릭

{% hint style="info" %}
코드 내 user## 가 있다면 ## 에는 유저넘버를 입력합니다.
{% endhint %}

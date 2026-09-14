# Task 1 - GenAI 기반 Terraform  코드 생성

{% hint style="info" %}
**이번 Lab은 실제 도메인 없이 진행합니다.** 원래 Route53 & ACM은 실제로 소유한 도메인이 있어야 완전한 실습이 가능하지만, 도메인이 없어도 **셀프사인(self-signed) 인증서**를 ACM에 "가져오기(import)"하는 방식으로 HTTPS 리스너 구성 원리 자체는 그대로 검증할 수 있습니다.&#x20;
{% endhint %}

1. 디렉토리 생성

```
mkdir -p ~/environment/route53-acm/modules/vpc
cd ~/environment/route53-acm
```

{% hint style="info" %}
사전 구성 리소스

없음(독립 폴더). Route53 호스팅존이나 도메인도 필요 없습니다.
{% endhint %}

2. 다음 조건을 모두 만족하는 VPC 모듈 구성

{% hint style="success" %}
VPC 모듈
{% endhint %}

<table><thead><tr><th width="176">리소스</th><th>요구사항</th></tr></thead><tbody><tr><td>VPC</td><td>기본 CIDR <code>10.0.0.0/16</code></td></tr><tr><td>퍼블릭 서브넷 2개</td><td><code>10.0.1.0/24</code>(2a), <code>10.0.2.0/24</code>(2c)</td></tr><tr><td>IGW + Public RT</td><td>인터넷 아웃바운드</td></tr><tr><td>ALB SG</td><td>TCP 80, TCP 443 ← <code>0.0.0.0/0</code></td></tr></tbody></table>

모듈 입력: `prefix` / 모듈 출력: `vpc_id`, `subnet_ids`, `alb_sg_id`

3. 셀프사인 인증서를 만들어 ACM에 가져오기(import)

{% hint style="info" icon="label" %}
* `tls_private_key`로 RSA 2048비트 개인키를 생성한다.
* `tls_self_signed_cert`로 위 개인키를 이용해 셀프사인 인증서를 생성한다(Common Name은 `user##.self-signed.local`처럼 임의 값, 유효기간은 1년, `server_auth` 용도 포함).
* `aws_acm_certificate`를 **`domain_name`/`validation_method`가 아니라 `private_key`/`certificate_body`로 가져오기(import)** 방식으로 생성한다. 이 방식은 DNS 검증 절차 없이 즉시 `ISSUED` 상태가 된다.
{% endhint %}

4. ALB + HTTPS 리스너 구성

{% hint style="info" icon="label" %}
* ALB는 퍼블릭 서브넷 2개에 걸쳐 만들고 ALB SG를 붙인다.
* 443 리스너는 위에서 가져온 인증서를 사용하고, 기본 액션은 고정 응답(`fixed-response`, `text/plain`, `200`, 본문에 `user##`가 포함된 문자열)으로 둔다.
* 80 리스너는 요청을 그대로 처리하지 않고, **443으로 리다이렉트**(`redirect`, `status_code=HTTP_301`)한다.
{% endhint %}

{% hint style="success" %}
상세스펙
{% endhint %}

<table><thead><tr><th width="121.45452880859375">항목</th><th>요구사항</th></tr></thead><tbody><tr><td>개인키</td><td><code>tls_private_key</code>, RSA 2048</td></tr><tr><td>인증서</td><td><code>tls_self_signed_cert</code>, CN <code>user##.self-signed.local</code>, 유효기간 8760시간(1년), <code>allowed_uses=["key_encipherment","digital_signature","server_auth"]</code></td></tr><tr><td>ACM 등록</td><td><code>aws_acm_certificate</code>를 import 방식(<code>private_key</code>+<code>certificate_body</code>)으로 생성</td></tr><tr><td>ALB</td><td>이름 <code>user##-https-alb</code>, 퍼블릭 서브넷 2개, ALB SG</td></tr><tr><td>443 리스너</td><td>위 ACM 인증서 연결, <code>ssl_policy=ELBSecurityPolicy-TLS13-1-2-2021-06</code>, 기본 액션 fixed-response 200</td></tr><tr><td>80 리스너</td><td>443으로 301 리다이렉트</td></tr><tr><td>태그</td><td><code>Name</code>, <code>Environment=lab</code></td></tr><tr><td>output</td><td><code>certificate_arn</code>, <code>alb_dns_name</code></td></tr></tbody></table>

5. 생성형 AI에게 질의하여 코드를 생성
6. 생성한 코드를 C9의 파일로 생성
7. 코드 내용을 넣고, Ctrl + S 키를 입력
8. 파일명을 임의로 지정하고, **route53-acm 디렉토리를 선택**한 뒤 Save 클릭

{% hint style="info" %}
코드 내 user## 가 있다면 ## 에는 유저넘버를 입력합니다.
{% endhint %}


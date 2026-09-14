# Task 1 - GenAI 기반 Terraform  코드 생성

1. 디렉토리 생성

```
mkdir ~/environment/eip-nat
```

{% hint style="info" %}
사전 구성 리소스
{% endhint %}

<table><thead><tr><th width="119.45452880859375">리소스</th><th>요구사항</th></tr></thead><tbody><tr><td>VPC</td><td>기본 CIDR <code>10.0.0.0/16</code>, 보조 CIDR <code>10.1.0.0/16</code> 연결, DNS Support/Hostnames <code>true</code>, Name 태그 <code>user##-vpc</code></td></tr><tr><td>서브넷 5개</td><td>퍼블릭 2개(<code>10.0.1.0/24</code>, <code>10.0.2.0/24</code>), 프라이빗 2개(<code>10.0.11.0/24</code>, <code>10.0.12.0/24</code>), 보조 1개(<code>10.1.1.0/24</code>)</td></tr><tr><td>IGW</td><td>VPC에 연결, Name 태그 <code>user##-igw</code></td></tr><tr><td>Public RT</td><td><code>0.0.0.0/0 → IGW</code>, 퍼블릭 서브넷 2개와 연결</td></tr><tr><td>Private RT</td><td>프라이빗 서브넷 2개 + 보조 서브넷 1개와 연결</td></tr></tbody></table>





2. 다음 조건을 모두 만족하는 IGW RT 구성&#x20;

{% hint style="info" icon="label" %}
* Elastic IP 1개를 발급한다(`domain = "vpc"`).
* NAT Gateway 1개를 퍼블릭 서브넷(`user##-public-a`)에 만들고, 위에서 발급한 EIP를 연결한다.
* NAT Gateway는 IGW가 먼저 생성된 뒤에 만들어지도록 의존성을 명시한다.
* 프라이빗 라우팅 테이블(`user##-private-rt`)에 `0.0.0.0/0 → NAT Gateway` 라우팅 규칙을 추가한다.
* 퍼블릭 라우팅 테이블은 변경하지 않는다(여전히 `0.0.0.0/0 → IGW`만 존재)
{% endhint %}

{% hint style="success" %}
상세스펙
{% endhint %}

<table><thead><tr><th width="194.3636474609375">리소스</th><th>요구사항</th></tr></thead><tbody><tr><td>EIP</td><td><code>domain = "vpc"</code>, Name 태그 <code>user##-nat-eip</code></td></tr><tr><td>NAT Gateway</td><td><code>subnet_id = user##-public-a</code>, <code>allocation_id =</code> 위 EIP, IGW 생성 후 생성되도록 <code>depends_on</code> 명시, Name 태그 <code>user##-natgw</code></td></tr><tr><td>Private RT 라우팅 규칙</td><td><code>0.0.0.0/0 → NAT Gateway</code> (Public RT가 아닌 Private RT에 추가)</td></tr><tr><td>태그</td><td>모든 리소스에 <code>Name</code> 태그, <code>Environment = lab</code> 태그</td></tr></tbody></table>



\
3\. 생성형 AI에게 질의하여 코드를 생성

{% hint style="warning" %}
**주의:** 사전 조건 리소스도 생성하게 요청해야합니다.
{% endhint %}



4. 생성한 코드를 C9의 파일로 생성

<figure><img src="../../.gitbook/assets/image (1000).png" alt=""><figcaption></figcaption></figure>



5. 코드 내용을 넣고, Ctrl + S 키를 입력

<figure><img src="../../.gitbook/assets/image (1001).png" alt=""><figcaption></figcaption></figure>



6. &#x20;파일명을 임의로 지정하고, eip-nat 디렉토리를 선택한 뒤 Save 클릭


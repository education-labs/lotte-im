# Task 1 - GenAI 기반 Terraform  코드 생성

1. 디렉토리 생성

```
mkdir ~/environment/igw-rt
```

{% hint style="info" %}
사전 구성 리소스
{% endhint %}

<table><thead><tr><th width="161.6363525390625">리소스</th><th>요구사항</th></tr></thead><tbody><tr><td>VPC</td><td>기본 CIDR <code>10.0.0.0/16</code>, DNS Support/Hostnames <code>true</code>, Name 태그 <code>user##-vpc</code></td></tr><tr><td>VPC 보조 CIDR</td><td>보조 CIDR <code>10.1.0.0/16</code>을 동일 VPC에 추가 연결</td></tr><tr><td>퍼블릭 서브넷 2개</td><td><code>10.0.1.0/24</code>(2a), <code>10.0.2.0/24</code>(2c), <code>map_public_ip_on_launch = true</code></td></tr><tr><td>프라이빗 서브넷 2개</td><td><code>10.0.11.0/24</code>(2a), <code>10.0.12.0/24</code>(2c)</td></tr><tr><td>보조 서브넷 1개</td><td><code>10.1.1.0/24</code>(2a), 보조 CIDR 연결 완료 후 생성(<code>depends_on</code>)</td></tr></tbody></table>



2. 다음 조건을 모두 만족하는 IGW RT 구성&#x20;

{% hint style="info" icon="label" %}
* 인터넷 게이트웨이 1개를 만들어 VPC에 연결(attach)한다.
* 퍼블릭 라우팅 테이블 1개를 만들어 `0.0.0.0/0` 트래픽을 IGW로 보내는 라우팅 규칙을 넣는다.
* 퍼블릭 라우팅 테이블을 퍼블릭 서브넷 2개 모두에 연결(association)한다.
* 프라이빗 라우팅 테이블 1개를 만들되, 인터넷으로 나가는 라우팅 규칙(`0.0.0.0/0`)은 넣지 않는다.
* 프라이빗 라우팅 테이블을 프라이빗 서브넷 2개와 보조 서브넷 1개에 연결(association)한다.
{% endhint %}

{% hint style="success" %}
상세스펙
{% endhint %}

<table><thead><tr><th width="175.45452880859375">리소스</th><th>요구사항</th></tr></thead><tbody><tr><td>Internet Gateway</td><td>VPC에 연결(attach), Name 태그 <code>user##-igw</code></td></tr><tr><td>Public Route Table</td><td>라우팅 규칙 <code>0.0.0.0/0 → user##-igw</code>, Name 태그 <code>user##-public-rt</code></td></tr><tr><td>Public RT 연결</td><td><code>user##-public-a</code>, <code>user##-public-c</code> 서브넷과 연결</td></tr><tr><td>Private Route Table</td><td>명시적 인터넷 라우팅 규칙 없음(로컬 라우팅만 존재), Name 태그 <code>user##-private-rt</code></td></tr><tr><td>Private RT 연결</td><td><code>user##-private-a</code>, <code>user##-private-c</code>, <code>user##-secondary-a</code> 서브넷과 연결</td></tr><tr><td>태그</td><td>모든 리소스에 <code>Name</code> 태그, <code>Environment = lab</code> 태그</td></tr></tbody></table>

\
3\. 생성형 AI에게 질의하여 코드를 생성

{% hint style="warning" %}
**주의:** 사전 조건 리소스도 생성하게 요청해야합니다.
{% endhint %}



4. 생성한 코드를 C9의 파일로 생성

<figure><img src="../../.gitbook/assets/image (1000).png" alt=""><figcaption></figcaption></figure>



5. 코드 내용을 넣고, Ctrl + S 키를 입력

<figure><img src="../../.gitbook/assets/image (1001).png" alt=""><figcaption></figcaption></figure>



6. &#x20;파일명을 임의로 지정하고, igwrt 디렉토리를 선택한 뒤 Save 클릭


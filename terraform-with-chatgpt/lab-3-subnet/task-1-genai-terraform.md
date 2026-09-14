# Task 1 - GenAI 기반 Terraform  코드 생성

1. 디렉토리 생성

```
mkdir ~/environment/subnet
```

{% hint style="info" %}
사전 구성 리소스
{% endhint %}

<table><thead><tr><th width="175.45452880859375">리소스</th><th width="565.2727661132812">요구사항</th></tr></thead><tbody><tr><td>VPC</td><td>기본 CIDR <code>10.0.0.0/16</code>, DNS Support/Hostnames <code>true</code>, Name 태그 <code>user##-vpc</code></td></tr><tr><td>VPC 보조 CIDR</td><td>보조 CIDR <code>10.1.0.0/16</code>을 동일 VPC에 추가 연결</td></tr></tbody></table>





2. 다음 조건을 모두 만족하는 Subnet 을 구성

{% hint style="info" icon="label" %}
* 기본 CIDR(`10.0.0.0/16`) 위에 **퍼블릭 서브넷 2개**를 서로 다른 AZ(`ap-northeast-2a`, `ap-northeast-2c`)에 만든다.
* 기본 CIDR 위에 **프라이빗 서브넷 2개**를 서로 다른 AZ(`ap-northeast-2a`, `ap-northeast-2c`)에 만든다.
* 보조 CIDR(`10.1.0.0/16`) 위에 **서브넷 1개**를 `ap-northeast-2a`에 만든다.
* 퍼블릭 서브넷은 `map_public_ip_on_launch = true`, 프라이빗/보조 서브넷은 `false`(또는 미지정 기본값)여야 한다.
* 모든 서브넷에 `Tier` 태그(`Public` / `Private` / `Secondary`)를 부여한다.
* 보조 CIDR 위의 서브넷은 **보조 CIDR 연결(association)이 완료된 후에** 생성되도록 의존성이 코드에 명시되어 있어야 한다.
{% endhint %}

{% hint style="success" %}
상세스펙
{% endhint %}

<table><thead><tr><th width="123.63641357421875">서브넷</th><th>CIDR</th><th>AZ</th><th width="107.63629150390625">Public IP 자동할당</th><th width="119.54541015625">Tier 태그</th><th width="126.636474609375">Name 태그</th></tr></thead><tbody><tr><td>Public A</td><td><code>10.0.1.0/24</code></td><td>ap-northeast-2a</td><td><code>true</code></td><td><code>Public</code></td><td><code>user##-public-a</code></td></tr><tr><td>Public C</td><td><code>10.0.2.0/24</code></td><td>ap-northeast-2c</td><td><code>true</code></td><td><code>Public</code></td><td><code>user##-public-c</code></td></tr><tr><td>Private A</td><td><code>10.0.11.0/24</code></td><td>ap-northeast-2a</td><td><code>false</code></td><td><code>Private</code></td><td><code>user##-private-a</code></td></tr><tr><td>Private C</td><td><code>10.0.12.0/24</code></td><td>ap-northeast-2c</td><td><code>false</code></td><td><code>Private</code></td><td><code>user##-private-c</code></td></tr><tr><td>Secondary A</td><td><code>10.1.1.0/24</code></td><td>ap-northeast-2a</td><td><code>false</code></td><td><code>Secondary</code></td><td><code>user##-secondary-a</code></td></tr></tbody></table>



\
3\. 생성형 AI에게 질의하여 코드를 생성

{% hint style="warning" %}
**주의:** 사전 조건 리소스도 생성하게 요청해야합니다.
{% endhint %}



4. 생성한 코드를 C9의 파일로 생성

<figure><img src="../../.gitbook/assets/image (1000).png" alt=""><figcaption></figcaption></figure>



5. 코드 내용을 넣고, Ctrl + S 키를 입력

<figure><img src="../../.gitbook/assets/image (1001).png" alt=""><figcaption></figcaption></figure>



6. &#x20;파일명을 임의로 지정하고, subnet 디렉토리를 선택한 뒤 Save 클릭


# Task 1 - GenAI 기반 Terraform  코드 생성

1. 디렉토리 생성

```
mkdir ~/environment/sg
```

{% hint style="info" %}
사전 구성 리소스

보안그룹은 서브넷이 아니라 VPC 단위로 붙는 리소스이므로, 서브넷·IGW·RT·NAT Gateway는 이번 Lab에서 굳이 다시 만들지 않습니다.
{% endhint %}

<table><thead><tr><th width="120.9090576171875">리소스</th><th>요구사항</th></tr></thead><tbody><tr><td>VPC</td><td>기본 CIDR <code>10.0.0.0/16</code>, DNS Support/Hostnames <code>true</code>, Name 태그 <code>user##-vpc</code></td></tr></tbody></table>



2. 다음 조건을 모두 만족하는 SG 구성&#x20;

{% hint style="info" icon="label" %}
* Web SG: `80`(HTTP), `443`(HTTPS)는 모든 곳(`0.0.0.0/0`)에서 허용. `22`(SSH)는 본인의 퍼블릭 IP(`/32`)에서만 허용.
* App SG: `8080`(애플리케이션 포트)과 `22`(SSH)를 Web SG를 소스로 지정해서 허용한다(CIDR이 아니라 SG 참조).
* DB SG: `3306`(MySQL)을 App SG를 소스로 지정해서만 허용한다. Web SG나 `0.0.0.0/0`에서는 허용하지 않는다.
* 세 보안그룹 모두 아웃바운드(egress)는 모든 트래픽을 `0.0.0.0/0`으로 명시적으로 허용한다.
* 세 개의 티어를 하나의 보안그룹으로 합치지 않고, 반드시 3개로 분리한다.
{% endhint %}

{% hint style="success" %}
상세스펙
{% endhint %}

<table><thead><tr><th width="120.0909423828125">보안그룹</th><th width="269.3636474609375">인바운드(Ingress) 규칙</th><th width="211.727294921875">아웃바운드(Egress)</th><th>Name 태그</th></tr></thead><tbody><tr><td>Web SG</td><td>TCP 80, 443 ← <code>0.0.0.0/0</code> / TCP 22 ← 본인 IP(<code>/32</code>)</td><td>전체 허용 → <code>0.0.0.0/0</code></td><td><code>user##-web-sg</code></td></tr><tr><td>App SG</td><td>TCP 8080 ← Web SG / TCP 22 ← Web SG</td><td>전체 허용 → <code>0.0.0.0/0</code></td><td><code>user##-app-sg</code></td></tr><tr><td>DB SG</td><td>TCP 3306 ← App SG</td><td>전체 허용 → <code>0.0.0.0/0</code></td><td><code>user##-db-sg</code></td></tr></tbody></table>



\
3\. 생성형 AI에게 질의하여 코드를 생성

{% hint style="warning" %}
**주의:** 사전 조건 리소스도 생성하게 요청해야합니다.
{% endhint %}



4. 생성한 코드를 C9의 파일로 생성

<figure><img src="../../.gitbook/assets/image (1000).png" alt=""><figcaption></figcaption></figure>



5. 코드 내용을 넣고, Ctrl + S 키를 입력

<figure><img src="../../.gitbook/assets/image (1001).png" alt=""><figcaption></figcaption></figure>



6. &#x20;파일명을 임의로 지정하고, sg 디렉토리를 선택한 뒤 Save 클릭


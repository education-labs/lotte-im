# Task 3 - NSG 생성과 규칙 구성

1. "네트워크 보안 그룹"을 검색해 서비스로 이동 후 \[만들기]

<figure><img src="../../.gitbook/assets/image (1062).png" alt="" width="563"><figcaption></figcaption></figure>



2. `nsg-web-user**` · `nsg-app-user**` · `nsg-db-user**` 세 개를 만듭니다.

<figure><img src="../../.gitbook/assets/image (1063).png" alt="" width="375"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1071).png" alt="" width="563"><figcaption></figcaption></figure>







3. 각 NSG의 \[인바운드 보안 규칙]에서 규칙을 추가합니다.

**규칙 구성 (nsg-web-user\*)**

<table><thead><tr><th width="98.90911865234375">소스</th><th width="93.45465087890625">원본 포트 범위</th><th width="88.45452880859375">대상주소</th><th>서비스</th><th>작업</th><th>우선순위</th><th width="85.272705078125">이름</th></tr></thead><tbody><tr><td>Any</td><td>*</td><td>Any</td><td>HTTPS</td><td>허용</td><td>110</td><td>https</td></tr><tr><td>My IP address</td><td>*</td><td>Any</td><td>SSH</td><td>허용</td><td>120</td><td>ssh</td></tr><tr><td>Any</td><td>*</td><td>Any</td><td>Custom</td><td>(프로토콜<br>ICMPv4)<br>허용</td><td>130</td><td>ping</td></tr></tbody></table>



**규칙 구성 (nsg-app-user\*)**

<table><thead><tr><th width="98.90911865234375">소스(IP)</th><th width="77.09100341796875">원본 포트 범위</th><th width="77.54541015625">대상주소</th><th width="86.727294921875">서비스</th><th width="88">대상 포트 범위</th><th width="76.1817626953125">작업</th><th width="105.2728271484375">우선순위</th><th width="103.45458984375">이름</th></tr></thead><tbody><tr><td>10.**.8.0/24</td><td>8080</td><td>Any</td><td>Custom</td><td>8080</td><td>허용</td><td>110</td><td>8080</td></tr></tbody></table>









4. 규칙을 만든 뒤 \[서브넷] 메뉴에서 각 서브넷에 연결합니다.&#x20;

`nsg-web-user** → snet-web`,&#x20;

`nsg-app-user** → snet-app`,&#x20;

`nsg-db-user** → snet-db`

<figure><img src="../../.gitbook/assets/image (1073).png" alt=""><figcaption></figcaption></figure>





* [ ] NSG 3개가 각 서브넷에 연결되었는가
* [ ] 기본 규칙 65500 `DenyAllInBound` 가 보이는가
* [ ] ICMP 허용 규칙을 추가했는가

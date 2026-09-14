# Task 1 - 허브 VNet 생성

Hub-Spoke에서 **허브**는 게이트웨이·방화벽 같은 공용 장비가 들어가는 VNet입니다. 서브넷 4개를 지금 한 번에 만들어 두면 Lab 4(방화벽)·Lab 5(게이트웨이)에서 추가 작업 없이 바로 배포할 수 있습니다.

1. "가상 네트워크" → \[만들기]
2. Lab 2에서 만든 **같은 리소스 그룹**(`rg-user**`)을 선택합니다. 이름은 `hub-vet-user**` 로 작성합니다.

<figure><img src="../../.gitbook/assets/image (1064).png" alt="" width="563"><figcaption></figcaption></figure>

3. 주소 공간을 `10.**.0.0/24` 로 설정합니다.

<figure><img src="../../.gitbook/assets/image (1065).png" alt=""><figcaption></figcaption></figure>



4. 서브넷을 아래와 같이 **4개 모두** 추가합니다. (이름 정확히 · 대소문자 포함)

**입력값**

<table><thead><tr><th>서브넷 용도</th><th width="292.9090576171875">이름</th><th>시작 주소</th></tr></thead><tbody><tr><td>Azure Firewall</td><td>AzureFirewallSubnet</td><td>10.**.0.0/26</td></tr><tr><td>Virtual Network Gateway</td><td>GatewaySubnet</td><td>10.**.0.64/27</td></tr><tr><td>default</td><td>snet-mgmt</td><td>10.**.0.96/27</td></tr><tr><td>Firewall Management</td><td>AzureFirewallManagementSubnet</td><td>10.**.0.128/26</td></tr></tbody></table>

5. \[검토 + 만들기] → \[만들기]

<figure><img src="../../.gitbook/assets/image (1066).png" alt=""><figcaption></figcaption></figure>



> 💡 **AzureFirewallManagementSubnet을 지금 함께 만드는 이유** — Lab 4에서 Azure Firewall을 **Basic SKU**로 배포할 때 이 서브넷이 **필수**로 요구됩니다. 방화벽 생성 화면에서 만들려면 번거로워서 여기서 미리 만들어 둡니다. (서브넷 이름은 VNet 안에서만 고유하면 되므로 다른 수강생과 같은 이름이어도 문제없습니다.)
>
> 💡 **허브를 `/24`로 잡은 이유** — 네 서브넷이 `.0~.191`을 쓰고 `.192~.255`가 남습니다. 나중에 Bastion(`/26`)을 넣을 자리입니다.

* [ ] 허브 VNet이 생성되었는가
* [ ] 서브넷 4개의 이름이 정확한가 (대소문자 포함)
* [ ] 스포크 대역(`10.**.8.0/22`)과 겹치지 않는가

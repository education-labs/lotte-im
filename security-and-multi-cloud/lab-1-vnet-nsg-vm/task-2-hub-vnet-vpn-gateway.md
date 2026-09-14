# Task 2 - 허브 VNet 생성 + VPN Gateway 배포 걸기

VPN Gateway는 허브 VNet의 GatewaySubnet 위에 만들어집니다. 따라서 순서는 **① 허브 VNet 생성 → ② 게이트웨이 배포 시작** 입니다.

## ① 허브 VNet + 서브넷 4개 생성

1. "가상 네트워크" → [만들기]
2. 같은 리소스 그룹(`rg-network-prod-userNN`)에 만듭니다.
3. 주소 공간을 `10.NN.0.0/24` 로 설정합니다.
4. 서브넷을 아래와 같이 **4개 모두** 추가합니다. (이름 정확히 · 대소문자 포함)

**입력값**

<table header-row="true">
<tr>
<td>항목</td>
<td>값</td>
</tr>
<tr>
<td>이름</td>
<td>vnet-hub-krc-prod-userNN</td>
</tr>
<tr>
<td>주소 공간</td>
<td>10.NN.0.0/24</td>
</tr>
<tr>
<td>AzureFirewallSubnet</td>
<td>10.NN.0.0/26</td>
</tr>
<tr>
<td>GatewaySubnet</td>
<td>10.NN.0.64/27</td>
</tr>
<tr>
<td>snet-mgmt</td>
<td>10.NN.0.96/27</td>
</tr>
<tr>
<td>AzureFirewallManagementSubnet</td>
<td>10.NN.0.128/26</td>
</tr>
</table>

5. [검토 + 만들기] → [만들기]

> ⚠️ 서브넷 이름은 정확히 입력해야 합니다. `AzureFirewallSubnet`, `GatewaySubnet`, `AzureFirewallManagementSubnet` — 대소문자까지 일치해야 하며, 틀리면 이후 Lab 3·Lab 4에서 배포가 실패합니다.
>
> 💡 **AzureFirewallManagementSubnet을 지금 함께 만드는 이유** — Lab 3에서 Azure Firewall을 Basic SKU로 배포할 때 이 서브넷이 **필수**로 요구됩니다. 방화벽 생성 화면에서 만들려면 번거로워서 여기서 미리 만들어 둡니다. (서브넷 이름은 VNet 안에서만 고유하면 되므로, 다른 수강생과 같은 이름이어도 문제없습니다.)

- [ ] 허브 VNet이 생성되었는가
- [ ] 서브넷 이름이 정확한가 (대소문자 포함)
- [ ] 스포크 대역과 겹치지 않는가

## ② VPN Gateway 배포 시작

포털에서 "가상 네트워크 게이트웨이" 검색 → [만들기]

- 이름 `vgw-hub-krc-prod-userNN`, 지역 `Korea Central`
- 게이트웨이 종류 **VPN** / VPN 종류 **경로 기반(Route-based)** ★ 나중에 변경 불가
- SKU **VpnGw2AZ** / 세대 **Generation 2**
- 가상 네트워크 `vnet-hub-krc-prod-userNN` 선택 → **GatewaySubnet 자동 인식** 확인
- 공용 IP `pip-vgw-1-userNN` (Standard) → **활성-활성 모드: 사용** → 두 번째 공용 IP `pip-vgw-2-userNN` (Standard)
- BGP: **사용 안 함**
- [검토 + 만들기] → [만들기]

배포가 시작되면 **여기서 멈추고 Task 3부터 이어서 진행합니다.** 배포 완료 확인 · 공용 IP 메모 · 피어링 게이트웨이 전송 설정은 나중에 **Lab 4**에서 이어집니다.

> ⚠️ 상세 화면과 주의사항은 **Lab 4 Task 1(VPN Gateway 배포)** 문서 참고. **"경로 기반"과 "활성-활성"** 은 Lab 5의 AWS 연결에 필수이며, 잘못 만들면 게이트웨이를 지우고 45분을 다시 기다려야 합니다.

- [ ] VPN Gateway 배포를 시작했는가 (경로 기반 / VpnGw2AZ / 활성-활성)

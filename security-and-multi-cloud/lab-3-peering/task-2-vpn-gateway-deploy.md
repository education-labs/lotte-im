# Task 2 - VPN Gateway 배포 걸어두기

> 🚀 **이 Task는 "시작만" 하고 넘어갑니다.** VPN Gateway는 배포에 **20~45분**이 걸립니다. 여기서 배포를 걸어두고 Task 3(Peering) → Lab 4(방화벽)를 진행하면, Lab 5에 도착할 때쯤 배포가 끝나 있습니다.

VPN Gateway는 허브 VNet의 **GatewaySubnet 위에** 만들어집니다. 그래서 Task 1(허브 VNet 생성)이 먼저입니다.

## 배포 시작

1. 포털에서 "가상 네트워크 게이트웨이" 검색 → [만들기]
2. 아래 값을 입력합니다.

<table header-row="true">
<tr>
<td>항목</td>
<td>값</td>
<td>나중에 변경</td>
</tr>
<tr>
<td>이름</td>
<td>vgw-hub-krc-prod-user**</td>
<td>불가</td>
</tr>
<tr>
<td>지역</td>
<td>Korea Central</td>
<td>불가</td>
</tr>
<tr>
<td>게이트웨이 종류</td>
<td>VPN</td>
<td>불가</td>
</tr>
<tr>
<td>VPN 종류</td>
<td>경로 기반 (Route-based) ★</td>
<td>불가</td>
</tr>
<tr>
<td>SKU</td>
<td>VpnGw2AZ</td>
<td>업그레이드만 가능</td>
</tr>
<tr>
<td>세대</td>
<td>Generation 2</td>
<td>불가</td>
</tr>
<tr>
<td>가상 네트워크</td>
<td>vnet-hub-krc-prod-user** (GatewaySubnet 자동 인식)</td>
<td>불가</td>
</tr>
<tr>
<td>공용 IP</td>
<td>pip-vgw-1-user** (Standard)</td>
<td>—</td>
</tr>
<tr>
<td>활성-활성 모드</td>
<td>사용 ★ → 두 번째 공용 IP pip-vgw-2-user** (Standard)</td>
<td>가능(재배포 발생)</td>
</tr>
<tr>
<td>BGP</td>
<td>사용 안 함 (이번 실습은 정적 라우팅)</td>
<td>가능</td>
</tr>
</table>

3. [검토 + 만들기] → [만들기]
4. **배포가 시작되면 여기서 멈추고 Task 3으로 넘어갑니다.**

> ⚠️ **"경로 기반"과 "활성-활성"은 반드시 지금 제대로 선택하세요.**
> - **경로 기반** : 정책 기반은 BGP·활성-활성·P2S를 지원하지 않습니다. 정책 기반으로 만들면 **Lab 6의 AWS 연결에서 막히고**, 그때는 게이트웨이를 지우고 45분을 다시 기다려야 합니다.
> - **활성-활성** : AWS는 S2S VPN 연결마다 터널을 **항상 2개** 제공합니다. 이를 제대로 받으려면 Azure 쪽에도 인스턴스(공용 IP)가 2개 있어야 합니다. 지금 끄면 Lab 5의 8083 상태 프로브도, Lab 6의 이중화 구성도 확인할 수 없습니다.
>
> 💡 **배포 완료 확인 · 공용 IP 메모 · 피어링의 게이트웨이 전송 설정**은 **Lab 5 Task 1**에서 이어서 합니다. 지금은 배포만 걸어두면 됩니다.

- [ ] VPN Gateway 배포가 **시작**되었는가
- [ ] VPN 종류가 **경로 기반**인가 ★
- [ ] **활성-활성 = 사용** 이고 공용 IP를 2개 만들었는가 ★

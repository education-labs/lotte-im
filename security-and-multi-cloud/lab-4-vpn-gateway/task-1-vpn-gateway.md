# Task 1 - VPN Gateway 배포 (가장 먼저 시작)

> ⚠️ **배포에 20~45분이 걸립니다.** Lab 1 Task 2에서 이미 걸어두었다면 아래 1~11단계는 건너뛰고 **「배포 완료 후」** 부터 진행하세요. SKU와 VPN 종류는 **나중에 변경할 수 없어** 다시 만들려면 45분을 또 기다립니다.

1. "가상 네트워크 게이트웨이"를 검색해 [만들기]
2. 구독·리소스 그룹(`rg-network-prod-userNN`)을 확인하고 이름 `vgw-hub-krc-prod-userNN`, 지역 `Korea Central`을 입력합니다.
3. 게이트웨이 종류 **VPN**, VPN 종류 **경로 기반** 선택.
4. SKU **VpnGw2AZ**, 세대 **Generation 2** 선택.
5. 가상 네트워크에서 `vnet-hub-krc-prod-userNN` 선택 → **GatewaySubnet이 자동 인식**되는지 확인합니다.
6. 공용 IP 주소에서 [새로 만들기] → 이름 `pip-vgw-1-userNN`, SKU **Standard**.
7. **활성-활성 모드: 사용**을 선택합니다 → 두 번째 공용 IP 입력란이 나타납니다.
8. 두 번째 공용 IP를 만듭니다. 이름 `pip-vgw-2-userNN`, SKU **Standard**.
9. BGP 구성: **사용 안 함** (이번 실습은 정적 라우팅).
10. [검토 + 만들기] → [만들기].
11. 배포가 시작되면 여기서 멈추고, 배포되는 동안 다른 Lab을 진행합니다.

**입력값 정리**

<table header-row="true">
<tr>
<td>항목</td>
<td>값</td>
<td>나중에 변경</td>
</tr>
<tr>
<td>이름</td>
<td>vgw-hub-krc-prod-userNN</td>
<td>불가</td>
</tr>
<tr>
<td>게이트웨이 종류</td>
<td>VPN</td>
<td>불가</td>
</tr>
<tr>
<td>VPN 종류</td>
<td>경로 기반 (Route-based)</td>
<td>불가 ★</td>
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
<td>vnet-hub-krc-prod-userNN</td>
<td>불가</td>
</tr>
<tr>
<td>활성-활성</td>
<td>사용 ★</td>
<td>가능(재배포 발생)</td>
</tr>
<tr>
<td>공용 IP</td>
<td>pip-vgw-1-userNN / pip-vgw-2-userNN (Standard)</td>
<td>—</td>
</tr>
<tr>
<td>BGP</td>
<td>사용 안 함</td>
<td>가능</td>
</tr>
</table>

> ⚠️ **"경로 기반"을 반드시 선택하세요.** 정책 기반은 BGP와 활성-활성을 지원하지 않습니다. 정책 기반으로 만들면 **Lab 5 AWS 연결에서 막히고**, 그때는 게이트웨이를 지우고 45분을 다시 기다려야 합니다.
>
> ⚠️ **활성-활성을 켜는 이유는 Lab 5 AWS 연결을 위해서입니다.** AWS는 S2S VPN 연결마다 터널을 **항상 2개** 제공합니다. 이를 제대로 받으려면 Azure 쪽에도 인스턴스(공용 IP)가 2개 있어야 합니다. 지금 끄면 Task 3의 8083 프로브도, Lab 5의 이중화 구성도 확인할 수 없습니다.

## 배포 완료 후

1. 게이트웨이 [개요]에서 상태가 **성공**인지 확인합니다.
2. **공용 IP 두 개를 메모장에 기록합니다.**
3. **Lab 2에서 만든 피어링으로 돌아가 게이트웨이 전송을 켭니다.** 게이트웨이가 생긴 지금에야 설정할 수 있는 항목입니다. **순서가 중요합니다 — 허브 먼저, 그다음 스포크.**
   - **① 허브 쪽** — `vnet-hub-krc-prod-userNN` → [피어링] → `peer-hub-to-spoke-userNN` 클릭 → 「로컬 가상 네트워크 피어링 설정」에서 **세 번째 체크박스** 켜기 → [저장]
   - **② 스포크 쪽** — `vnet-spoke-app-krc-prod-userNN` → [피어링] → `peer-spoke-to-hub-userNN` 클릭 → **네 번째 체크박스** 켜기 → [저장]

<table header-row="true">
<tr>
<td>위치</td>
<td>체크박스 문구</td>
<td>설정</td>
</tr>
<tr>
<td>① 허브 (peer-hub-to-spoke)</td>
<td>'vnet-hub…' 게이트웨이 또는 경로 서버가 트래픽을 'vnet-spoke…'에게 전달하도록 허용</td>
<td>체크</td>
</tr>
<tr>
<td>② 스포크 (peer-spoke-to-hub)</td>
<td>'vnet-spoke…'이(가) 'vnet-hub…'의 원격 게이트웨이 또는 경로 서버를 사용하도록 설정</td>
<td>체크</td>
</tr>
<tr>
<td>나머지 두 개</td>
<td>액세스 허용 · 전달된 트래픽 수신 허용</td>
<td>Lab 2에서 이미 체크됨</td>
</tr>
</table>

> ⚠️ **순서를 지켜야 합니다.** 허브의 「전달하도록 허용」이 꺼져 있으면 스포크의 「원격 게이트웨이 사용」은 **저장 자체가 거부됩니다.** Azure가 강제하는 순서입니다.
>
> 💡 **왜 지금 켜야 하나** — VPN Gateway는 기본적으로 **허브 VNet 대역만** 압니다. 이 설정을 켜야 스포크 대역(`10.NN.8.0/22`)이 게이트웨이에 전파됩니다. 안 켜면 **Lab 5 AWS 터널이 붙어도 돌아오는 패킷이 스포크를 찾지 못해 ping이 실패**합니다. 터널 상태는 정상으로 보이기 때문에 찾기 어려운 고장입니다.

- [ ] 게이트웨이 상태가 "성공"인가
- [ ] VPN 종류가 "경로 기반"인가
- [ ] 공용 IP가 **2개** 할당되었는가
- [ ] **두 IP를 메모했는가** → Lab 5에서 AWS에 입력 ★
- [ ] **허브 피어링에 「전달하도록 허용」을 켰는가** ★
- [ ] **스포크 피어링에 「원격 게이트웨이 사용」을 켰는가** ★

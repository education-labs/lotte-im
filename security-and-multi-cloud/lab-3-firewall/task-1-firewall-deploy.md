# Task 1 - Azure Firewall 배포

1. "방화벽"을 검색해 서비스로 이동 후 [만들기] — 검색 결과가 안 나오면 **영문 `Firewall`로 검색**하세요 (포털 언어 설정에 따라 한글 검색이 안 잡힐 때가 있습니다)
2. 리소스 그룹 : `rg-network-prod-userNN`
3. 이름·지역을 입력하고 방화벽 SKU를 **Basic** 으로 선택합니다.
4. 방화벽 관리 : "방화벽 정책 사용"을 선택합니다.
5. 방화벽 정책을 새로 만듭니다. (`afwp-corp-base-userNN`)
6. 가상 네트워크에서 `vnet-hub-krc-prod-userNN` 를 선택합니다.
7. 공용 IP를 새로 만듭니다. (Standard)

**입력값**

<table header-row="true">
<tr>
<td>항목</td>
<td>값</td>
</tr>
<tr>
<td>이름</td>
<td>afw-hub-krc-prod-userNN</td>
</tr>
<tr>
<td>SKU</td>
<td>Basic</td>
</tr>
<tr>
<td>정책</td>
<td>afwp-corp-base-userNN (신규)</td>
</tr>
<tr>
<td>가상 네트워크</td>
<td>vnet-hub-krc-prod-userNN</td>
</tr>
<tr>
<td>방화벽 서브넷</td>
<td>AzureFirewallSubnet (자동 인식)</td>
</tr>
<tr>
<td>공용 IP</td>
<td>pip-afw-prod-userNN (Standard)</td>
</tr>
</table>

> ⚠️ **Basic SKU는 관리용 서브넷(`AzureFirewallManagementSubnet`)이 필수입니다** (관리 트래픽 분리용 · 관리용 공용 IP도 함께 생성됨). 이 서브넷은 **Lab 1 Task 2에서 이미 만들었습니다** — 방화벽 배포 화면에서 기존 서브넷을 그대로 선택하세요.

**서브넷 확인**

<table header-row="true">
<tr>
<td>서브넷</td>
<td>대역</td>
<td>생성 시점</td>
</tr>
<tr>
<td>AzureFirewallSubnet</td>
<td>10.NN.0.0/26</td>
<td>Lab 1</td>
</tr>
<tr>
<td>GatewaySubnet</td>
<td>10.NN.0.64/27</td>
<td>Lab 1</td>
</tr>
<tr>
<td>snet-mgmt</td>
<td>10.NN.0.96/27</td>
<td>Lab 1</td>
</tr>
<tr>
<td>AzureFirewallManagementSubnet</td>
<td>10.NN.0.128/26</td>
<td>Lab 1</td>
</tr>
</table>

> 💡 허브를 `/24`로 잡은 이유가 여기서 드러납니다. 네 서브넷이 `.0~.191`을 쓰고 `.192~.255`가 남습니다 — 나중에 Bastion(`/26`)을 넣을 자리입니다.

## 배포 대기 시간에 진단 설정을 미리 켜 두세요

이걸 켜야 Task 5의 허용·거부 로그가 쌓입니다.

1. **Log Analytics 작업 영역이 없다면 먼저 만듭니다** — 포털 검색창에 `Log Analytics` 입력 → 작업 영역 → 만들기 → 리소스 그룹 `rg-network-prod-userNN`, 이름 `law-network-prod`, 지역 `Korea Central` → 검토+만들기. (1~2분 소요)
2. **방화벽에 진단 설정 연결** — `afw-hub-krc-prod-userNN` → 왼쪽 메뉴 **모니터링** 그룹 안의 **진단 설정** → 진단 설정 추가 → 이름 `diag-afw` → 로그에서 `allLogs`(또는 AzureFirewall로 시작하는 항목들) 체크 → 대상에 **Log Analytics 작업 영역으로 전송** 체크 → 위에서 만든 작업 영역 선택 → 저장.

> ⚠️ 진단 설정은 개요 화면이 아니라 **왼쪽 메뉴 하단 "모니터링"** 아래에 있어 찾기 어렵습니다.

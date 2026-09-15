# Task 1 - Azure Firewall 배포

1. "firewall"을 검색해 서비스로 이동 후 \[만들기]

<figure><img src="../../.gitbook/assets/image (1089).png" alt=""><figcaption></figcaption></figure>



2. 아래와 같이 입력

* 리소스 그룹 : rg-user\*\*
* 이름 : firewall-user\*\*
* 지역 : Korea Central

<figure><img src="../../.gitbook/assets/image (1090).png" alt="" width="548"><figcaption></figcaption></figure>



* SKU : 기본
* Firewall policy : Add new 클릭

<figure><img src="../../.gitbook/assets/image (1091).png" alt="" width="439"><figcaption></figcaption></figure>

* 정책 이름 : `afwp-corp-base-user**`

<figure><img src="../../.gitbook/assets/image (1092).png" alt="" width="423"><figcaption></figcaption></figure>







3. 가상 네트워크에서 `hub-vnet-user**` 를 선택합니다. \
   그리고 공용 IP 주소는 새로 추가를 클릭하여, 이름은 `fw-pip-user**` 를 입력합니다.

<figure><img src="../../.gitbook/assets/image (1093).png" alt=""><figcaption></figcaption></figure>





4. 방화벽 관리 NIC의 관리 퍼블릭 IP 주소도 새로 추가합니다.&#x20;

* 이름 : afw-pip-user\*\*

<figure><img src="../../.gitbook/assets/image (1094).png" alt=""><figcaption></figcaption></figure>

> ⚠️ **Basic SKU는 관리용 서브넷(`AzureFirewallManagementSubnet`)이 필수입니다** (관리 트래픽 분리용 · 관리용 공용 IP도 함께 생성됨). 이 서브넷은 **Lab 3 Task 1에서 이미 만들었습니다** — 방화벽 배포 화면에서 기존 서브넷을 그대로 선택하세요.



5. 검토 + 만들기 -> 만들기

<figure><img src="../../.gitbook/assets/image (1095).png" alt="" width="422"><figcaption></figcaption></figure>

##

##

## 6. 배포 대기 시간에 진단 설정을 미리 켜 두세요

이걸 켜야 Task 5의 허용·거부 로그가 쌓입니다.

1. **Log Analytics 작업 영역이 없다면 먼저 만듭니다** — 포털 검색창에 `Log Analytics` 입력 → 작업 영역 → 만들기 → 리소스 그룹 `rg-user**`, 이름 `law-network-user**`, 지역 `Korea Central` → 검토+만들기. (1\~2분 소요)

<figure><img src="../../.gitbook/assets/image (1096).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1097).png" alt="" width="563"><figcaption></figcaption></figure>





2. **방화벽에 진단 설정 연결** — `afw-hub-krc-prod-user**` → 왼쪽 메뉴 **모니터링** 그룹 안의 **진단 설정** → 진단 설정 추가 → 이름 diag-afw-user\*\* → 로그에서 `allLogs`(또는 AzureFirewall로 시작하는 항목들) 체크 → 대상에 **Log Analytics 작업 영역으로 전송** 체크 → 위에서 만든 작업 영역 선택 → 저장.

<figure><img src="../../.gitbook/assets/image (1098).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1099).png" alt="" width="563"><figcaption></figcaption></figure>



> ⚠️ 진단 설정은 개요 화면이 아니라 **왼쪽 메뉴 하단 "모니터링"** 아래에 있어 찾기 어렵습니다.

## ★ 배포 완료 후 — 방화벽 사설 IP 메모

**Task 3에서 경로 테이블의 '다음 홉 주소'로 쓸 값입니다. 여기서 반드시 메모하세요.**

1. 배포가 끝나면 `firewall-user**` → \[개요] 로 이동합니다.

<figure><img src="../../.gitbook/assets/image (1100).png" alt=""><figcaption></figcaption></figure>



2. **방화벽 개인 IP(Firewall private IP)** 항목의 값을 메모합니다.\
   `AzureFirewallSubnet` 이 `10.**.0.0/26` 이므로 보통 **`10.**.0.4`** 가 할당됩니다 (Azure가 각 서브넷의 앞 4개 주소를 예약하므로 첫 사용 가능 주소가 `.4`)







* [ ] 방화벽 상태가 **성공**인가
* [ ] **방화벽 사설 IP를 메모했는가** ★ → Task 3에서 사용
* [ ] 진단 설정(Log Analytics)을 켰는가 → Task 5의 로그 확인에 필요

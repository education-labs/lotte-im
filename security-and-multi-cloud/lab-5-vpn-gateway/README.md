# Lab 5 - VPN Gateway 배포와 하이브리드 연결

이 게이트웨이는 **Lab 3 Task 2에서 배포를 이미 시작**했습니다. Lab 2~3에서 만든 허브 VNet에 **밖으로 나가는 문**을 답니다. 여기서 만든 게이트웨이를 Lab 6에서 AWS 연결에 **그대로 재사용**합니다.

> ⚠️ **이 페이지의 이름·대역은 예시입니다.** `vgw-hub-krc-prod-user**` → `vgw-hub-내번호`, `10.**.x` → `10.내번호.x` 로 **바꿔서 입력**하세요.

## 이번 Lab에서 만들 것

<table header-row="true">
<tr>
<td>#</td>
<td>리소스</td>
<td>이름</td>
<td>역할</td>
<td>이후 사용처</td>
</tr>
<tr>
<td>1</td>
<td>Virtual Network Gateway</td>
<td>vgw-hub-krc-prod-user**</td>
<td>Azure 쪽 VPN 장비</td>
<td>Lab 6에서 재사용 ★</td>
</tr>
<tr>
<td>2</td>
<td>공용 IP 2개</td>
<td>pip-vgw-1-user** / pip-vgw-2-user**</td>
<td>게이트웨이의 외부 주소</td>
<td>Lab 6에서 AWS에 입력 ★</td>
</tr>
<tr>
<td>3</td>
<td>Local Network Gateway</td>
<td>lng-onprem-idc-user**</td>
<td>가상의 IDC를 정의</td>
<td>연습용 (연결되지 않음)</td>
</tr>
</table>

```
                   ┌────────────────────────────────────┐
[ 온프레미스 IDC ] │   Hub VNet 10.**.0.0/24            │
192.168.**.0/24 ┈┈┈ ┤  └ GatewaySubnet                   │
(실제 장비 없음)   │        └ vgw-hub-krc-prod-user** ←─┼── 오늘 만드는 것
                   │           pip-vgw-1-user**/2       │
[ AWS VPC ]        │                                    │
172.16.**.0/24 ←────┴────────────────────────────────────┘
(Lab 6)                  같은 게이트웨이에 AWS 연결을 추가
```

> ⚠️ **게이트웨이는 하나, 연결은 여러 개입니다.** IDC용 연결 연습(Task 2)은 선택 사항이며, Lab 6에서 AWS용 연결을 **같은 게이트웨이에** 추가합니다. 게이트웨이를 두 번 만들지 않습니다.
>
> ✅ **이 배포는 Lab 3 Task 2에서 이미 시작했습니다.** 여기서는 「배포 완료 후」 절차만 수행하세요 — 상태 '성공' 확인 · 공용 IP 2개 메모 · 피어링의 게이트웨이 전송 설정 켜기(허브 → 스포크 순서). 배포를 아직 안 걸었다면 Task 1부터 지금 진행합니다.

## Task 목록

* [Task 1 - VPN Gateway 배포 (가장 먼저 시작)](task-1-vpn-gateway.md)
* [Task 2 - Local Network Gateway 구성 (온프레미스 가정)](task-2-local-network-gateway.md)
* [Task 3 - 게이트웨이 상태 진단 실습](task-3-diagnostics.md)

# Lab 3 - 허브 VNet과 VNet Peering

Lab 2에서 만든 **스포크**에 이어, 이번 Lab에서는 **허브** VNet을 만들고 둘을 Peering으로 잇습니다. 그리고 시간이 오래 걸리는 **VPN Gateway 배포를 미리 걸어둡니다.**

> ⚠️ **이 페이지의 이름·대역은 예시입니다.** `vnet-hub-krc-prod-user**` 의 `**` 자리에 배정받은 번호를, `10.**.x` 의 `**` 자리에 같은 번호를 넣어 입력하세요. 단, **서브넷 이름(GatewaySubnet 등)은 그대로** 씁니다 — Azure가 정한 고정 이름입니다.

## 진행 순서와 이유

```
Task 1  허브 VNet 생성           ← GatewaySubnet 이 있어야 게이트웨이를 만들 수 있다
   ↓
Task 2  VPN Gateway 배포 걸기     ← 20~45분 걸림. 걸어두고 바로 다음으로
   ↓                                (배포는 Lab 4 동안 백그라운드로 진행)
Task 3  Peering 구성과 검증
```

## Task 목록

* [Task 1 - 허브 VNet 생성](task-1-hub-vnet.md)
* [Task 2 - VPN Gateway 배포 걸어두기](task-2-vpn-gateway-deploy.md)
* [Task 3 - Peering 구성과 검증](task-3-peering.md)

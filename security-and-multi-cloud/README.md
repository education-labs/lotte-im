# Security & Multi Cloud

Azure의 자격 증명·권한 체계(IAM)를 확인한 뒤, Hub-Spoke 네트워크를 직접 구성하고 Azure Firewall · VPN Gateway를 거쳐 **AWS와 하이브리드로 연결**하는 실습입니다. Lab 2에서 만든 스포크 VNet과 VM은 Lab 6까지 계속 사용하므로 **중간에 리소스를 삭제하지 마세요.**

교재 : `07. 보안 및 멀티클라우드 운영 기초_교재.pptx`

## 실습 환경과 공통 규칙

| 항목 | 값 |
|---|---|
| 리전 | Korea Central (모든 Azure 리소스 동일) · AWS는 서울(ap-northeast-2) |
| Azure 대역 | `10.**.0.0/16` (** = 내 번호) |
| AWS 대역 | `172.16.**.0/24` |
| IDC 대역(가정) | `192.168.**.0/24` |
| 명명 규칙 | 리소스 이름 뒤에 `-user**` 을 붙임 (예: `vnet-hub-krc-prod-user01`) |

> **시작 전에 — 내 번호로 바꿔 만듭니다.** 여러 명이 같은 구독을 함께 쓰므로, 문서에 적힌 이름과 대역을 그대로 쓰면 옆 사람과 충돌하거나 남의 리소스를 건드리게 됩니다. 문서의 값은 전부 예시입니다 — `user**` → 내 번호, `10.**.x` → `10.내번호.x` 로 바꿔서 입력하세요. 단, `GatewaySubnet` · `AzureFirewallSubnet` · `AzureFirewallManagementSubnet` · `AzureBastionSubnet` 같은 **서브넷 이름은 Azure가 정한 고정 이름**이라 바꾸지 않습니다.

## Lab 구성

| Lab | 내용 | 핵심 산출물 |
|---|---|---|
| [Lab 1](lab-1-azure-iam/README.md) | Azure IAM (Entra ID · RBAC) | 사용자·그룹, 역할 할당, 데이터 평면 역할, 서비스 주체·관리 ID |
| [Lab 2](lab-2-vnet-nsg-vm/README.md) | VNet · Subnet · NSG · VM 구성 | 대역 설계표, 스포크 VNet, NSG 3개, VM 2대 |
| [Lab 3](lab-3-peering/README.md) | 허브 VNet과 VNet Peering | 허브 VNet(특수 서브넷 4개), **VPN Gateway 배포 시작**, 허브 ↔ 스포크 Peering |
| [Lab 4](lab-4-firewall/README.md) | Hub-Spoke + Azure Firewall + 경로 테이블 | Azure Firewall(Basic), UDR, 허용·차단 검증 |
| [Lab 5](lab-5-vpn-gateway/README.md) | VPN Gateway 배포와 하이브리드 연결 | VPN Gateway 배포 확인, Local Network Gateway, 상태 진단 |
| [Lab 6](lab-6-aws-connection/README.md) | Azure ↔ AWS 연결 | AWS VGW/CGW/S2S VPN, Azure LNG/Connection, 통신 검증 |

## 진행 순서상 주의할 점

- Lab 1(IAM)은 **독립적인 실습**입니다. 여기서 만든 사용자·그룹·리소스 그룹은 Lab 2 이후에서 쓰지 않으므로 Lab 1 종료 시 정리해도 됩니다.
- **Lab 3 Task 2에서 VPN Gateway 배포를 걸어둡니다** (20~45분 소요). 걸어둔 채로 Lab 3 Task 3 → Lab 4를 진행하면 Lab 5에 도착할 때쯤 배포가 끝나 있습니다. **경로 기반 · 활성-활성** 을 반드시 확인하세요 — 나중에 바꿀 수 없습니다.
- Lab 3의 피어링에서 **"전달된 트래픽 수신 허용"** 체크는 기본이 꺼져 있으니 직접 켜야 합니다.
- Lab 4의 경로 테이블에는 **SSH 예외 경로**(`내 공인 IP/32 → 인터넷`)를 반드시 추가해야 이후 Lab의 SSH 접속이 끊기지 않습니다.
- Lab 5에서 게이트웨이 배포가 끝나면 **Lab 3 피어링으로 돌아가 게이트웨이 전송(허브 → 스포크 순서)** 을 켭니다.
- Lab 6는 AWS 콘솔과 Azure 포털을 오가며 진행합니다. **① Azure GW 공용 IP ② AWS EC2 사설 IP ③ AWS 터널 외부 IP ④ PSK** 4개 값을 메모하며 진행하세요.

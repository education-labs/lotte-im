# Task 4 - 라우팅·보안 구성과 통신 검증

> ⚠️ **터널이 붙어도 라우팅과 규칙이 없으면 통신되지 않습니다.** 네 곳을 모두 손봐야 합니다.

## 4-1. 🟧 AWS 쪽 두 가지

1. VPC → \[서브넷] → `subnet-private-user**` → **\[라우팅 테이블] 탭**에서 이 서브넷에 **연결된 라우팅 테이블이 무엇인지 먼저 확인**합니다.

<figure><img src="../../.gitbook/assets/image (1145).png" alt="" width="563"><figcaption></figcaption></figure>

> ⚠️ 서브넷을 만들 때 라우팅 테이블을 따로 지정하지 않았다면 **VPC의 기본(main) 라우팅 테이블**에 연결되어 있습니다. 아래 작업은 **그 테이블**에 해야 합니다. 다른 테이블을 고치면 경로가 반영되지 않습니다.

2. \[라우팅 전파] 탭 → \[라우팅 전파 편집] → VGW **활성화**.

<figure><img src="../../.gitbook/assets/image (1146).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1147).png" alt=""><figcaption></figcaption></figure>



{% hint style="info" %}
경로 전파가 안 되면, \[라우팅 편집]에서 `10.**.0.0/16` → 대상 `vgw-aws-user**` 경로를 직접 추가합니다. **경로 전파와 정적 경로 중 하나만 있으면 됩니다.**
{% endhint %}



3. EC2 → \[보안 그룹] → `ec2-user**`의 보안 그룹 선택 → \[인바운드 규칙 편집].

<figure><img src="../../.gitbook/assets/image (1148).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1149).png" alt=""><figcaption></figcaption></figure>

3. 유형 **모든 ICMP - IPv4**, 소스 `10.**.0.0/16`  추가합니다.

<figure><img src="../../.gitbook/assets/image (1151).png" alt=""><figcaption></figcaption></figure>

## 4-2. 🟦 Azure 쪽 두 가지

1. 🟦 **Azure 포털로 전환** → 방화벽 정책 `afwp-corp-base-user**` → \[네트워크 규칙] → \[규칙 컬렉션 추가]

<figure><img src="../../.gitbook/assets/image (1152).png" alt="" width="563"><figcaption></figcaption></figure>



2. 아래 규칙을 추가합니다. 컬렉션 이름 `rc-net-aws`, **우선순위 `210`**, 작업 `허용`

| 항목     | 값                          |
| ------ | -------------------------- |
| 이름     | rule                       |
| 원본 유형  | IP 주소                      |
| 원본     | 10.\*\*.8.0/22 (스포크)       |
| 프로토콜   | 모두                         |
| 대상 포트  | \*                         |
| 대상 유형  | IP 주소                      |
| 대상     | 172.16.\*\*.0/24 (AWS VPC) |

<figure><img src="../../.gitbook/assets/image (1153).png" alt=""><figcaption></figcaption></figure>



> 💡 **왜 한 방향만 열어도 되나** — Azure Firewall은 **스테이트풀**이라 내가 보낸 흐름의 응답은 자동으로 돌아옵니다. 게다가 AWS→Azure 인바운드는 VGW에서 피어링을 타고 VM으로 **직행**해 방화벽을 거치지 않습니다(4-3의 비대칭 설명 참고). AWS가 먼저 Azure로 연결을 여는 시나리오까지 필요하면 그때 반대 방향 규칙을 추가하세요.



## 4-3. 통신 검증

1. 🟦 Azure VM1에 SSH로 접속합니다. — **접속이 안 되면** Lab 4 Task 3의 SSH 예외 경로(`내IP/32 → 인터넷`)가 있는지 확인하세요. 앞 Lab에서는 됐는데 지금 안 된다면 **내 공인 IP가 바뀌었을 가능성**이 큽니다 — whatismyip에서 다시 확인해 경로를 갱신합니다.
2. AWS EC2로 ping을 보냅니다.

<figure><img src="../../.gitbook/assets/image (1155).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1154).png" alt=""><figcaption></figcaption></figure>



* [ ] Azure VM → AWS EC2 ping 성공


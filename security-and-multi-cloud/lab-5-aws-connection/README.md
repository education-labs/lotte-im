# Lab 5 - Azure ↔ AWS 연결

이 과정의 도착점입니다. **Lab 4에서 만든 Azure 게이트웨이에 AWS를 연결**합니다.

> ⚠️ **이 Lab은 두 콘솔을 오갑니다.** 지금 어디에 있는지 놓치면 반드시 막힙니다. 각 Task 제목의 🟧(AWS 콘솔) / 🟦(Azure 포털) 표시를 확인하세요. 브라우저 탭 2개를 미리 열어두고 탭을 바꿔가며 진행하는 것을 권합니다.
>
> ⚠️ **이 페이지의 이름·대역은 예시입니다.** `10.10.x` → `10.내번호.x`, AWS VPC `172.31.0.0/16` → 내 대역 로 **바꿔서 입력**하세요.
>
> ❗ 이 Lab은 **대역을 서로 넣어주는 작업**이라 치환을 빼먹으면 바로 막힙니다. 특히 **Task 2의 정적 경로(= Azure 대역)** 와 **Task 3의 LNG 주소 공간(= AWS 대역)** 을 내 값으로 넣었는지 확인하세요.

## 콘솔 전환 지도와 값 전달 흐름

**어느 콘솔에서 무엇을 하나**

<table header-row="true">
<tr>
<td>Task</td>
<td>콘솔</td>
<td>작업</td>
<td>받아오는 값</td>
<td>넘겨주는 값</td>
</tr>
<tr>
<td>1</td>
<td>🟧 AWS</td>
<td>VPC · 서브넷 · EC2 · VGW · CGW</td>
<td>← Azure GW 공용 IP (Lab 4)</td>
<td>EC2 사설 IP</td>
</tr>
<tr>
<td>2</td>
<td>🟧 AWS</td>
<td>S2S VPN 연결 생성</td>
<td>—</td>
<td>터널 외부 IP · PSK</td>
</tr>
<tr>
<td>3</td>
<td>🟦 Azure</td>
<td>LNG 생성 + Lab 4 게이트웨이에 연결 추가</td>
<td>← 터널 외부 IP · PSK</td>
<td>—</td>
</tr>
<tr>
<td>4</td>
<td>🟧🟦 양쪽</td>
<td>라우팅 · 보안 그룹 · 방화벽 · 통신 검증</td>
<td>—</td>
<td>—</td>
</tr>
</table>

**값이 오가는 순서**

```
🟦 Lab 4에서 메모해 둔 것
   └ Azure GW 공용 IP  ─────────────┐
                                    ▼
🟧 Task 1  VPC·EC2 만들기 → VGW 만들고 VPC에 연결 → CGW 만들 때 [Azure GW 공용 IP] 입력
                                    │
🟧 Task 2  S2S VPN 연결 생성 (PSK 직접 지정)
        └ 결과로 나오는 것 : 터널 1 외부 IP  ·  PSK ──┐
                                                     ▼
🟦 Task 3  LNG 만들 때 [터널 외부 IP] 입력 → Lab 4 게이트웨이에 연결 추가 시 [PSK] 입력
                                                     │
🟧🟦 Task 4  양쪽 라우팅·보안 열고 ping ←───────────┘
```

> 💡 **메모장을 하나 열어두고 아래 4개 값을 채워가며 진행하세요.** 이 값들을 잃어버리면 되돌아가야 합니다.
>
> ```
> ① Azure GW 공용 IP  : __  (Lab 4에서 메모)
> ② AWS EC2 사설 IP    : __  (Task 1에서 확인)
> ③ AWS 터널 1 외부 IP : __  (Task 2에서 확인)
> ④ PSK               : __  (Task 2에서 직접 지정)
> ```

## Task 목록

* [Task 1 - AWS 측 구성 (VPC · EC2 · VGW · CGW)](task-1-aws-setup.md) 🟧
* [Task 2 - AWS Site-to-Site VPN 연결 생성](task-2-aws-vpn.md) 🟧
* [Task 3 - Azure 측 연결 구성](task-3-azure-connection.md) 🟦
* [Task 4 - 라우팅·보안 구성과 통신 검증](task-4-routing-and-verify.md) 🟧🟦

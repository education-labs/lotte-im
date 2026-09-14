# Task 1 - VPN Gateway 배포 후 설정

## VPN Gateway 배포 완료 후

1. 게이트웨이 \[개요]에서 상태가 **성공**인지 확인합니다.

<figure><img src="../../.gitbook/assets/image (1109).png" alt="" width="563"><figcaption></figcaption></figure>

2. **공용 IP 두 개를 메모장에 기록합니다.**
3.  **Lab 3에서 만든 피어링으로 돌아가 게이트웨이 전송을 켭니다.** 게이트웨이가 생긴 지금에야 설정할 수 있는 항목입니다. **순서가 중요합니다 — 허브 먼저, 그다음 스포크.**



**① 허브 쪽** — `vnet-hub-user**` → \[피어링] → `peer-hub-to-spoke-user**` 클릭 → 「로컬 가상 네트워크 피어링 설정」에서 **세 번째 체크박스** 켜기 → \[저장]

<figure><img src="../../.gitbook/assets/image (1110).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1111).png" alt="" width="398"><figcaption></figcaption></figure>





**② 스포크 쪽** — `vnet-user**` → \[피어링] → `peer-spoke-to-hub-user**` 클릭 → **네 번째 체크박스** 켜기 → \[저장]

<table><thead><tr><th>위치</th><th width="380.72711181640625">체크박스 문구</th><th>설정</th></tr></thead><tbody><tr><td>① 허브 (peer-hub-to-spoke)</td><td>'vnet-hub…' 게이트웨이 또는 경로 서버가 트래픽을 'vnet-spoke…'에게 전달하도록 허용</td><td>체크</td></tr><tr><td>② 스포크 (peer-spoke-to-hub)</td><td>'vnet-spoke…'이(가) 'vnet-hub…'의 원격 게이트웨이 또는 경로 서버를 사용하도록 설정</td><td>체크</td></tr><tr><td>나머지 두 개</td><td>액세스 허용 · 전달된 트래픽 수신 허용</td><td>Lab 3에서 이미 체크됨</td></tr></tbody></table>

> ⚠️ **순서를 지켜야 합니다.** 허브의 「전달하도록 허용」이 꺼져 있으면 스포크의 「원격 게이트웨이 사용」은 **저장 자체가 거부됩니다.** Azure가 강제하는 순서입니다.
>
> ## 🛑 이 단계를 건너뛰면 Lab 6에서 반드시 막힙니다
>
> VPN Gateway는 기본적으로 **허브 VNet 대역만** 압니다. 이 두 체크박스를 켜야 스포크 대역(`10.**.8.0/22`)이 게이트웨이에 전파됩니다.
>
> 안 켠 채로 Lab 6을 진행하면 — **터널은 `UP`, 연결 상태는 「연결됨」, 데이터 입출력 바이트도 양쪽 다 증가합니다. 그런데 ping만 실패합니다.** AWS에서 돌아온 패킷이 스포크로 가는 길을 못 찾아 **게이트웨이에서 조용히 버려지기** 때문입니다.
>
> 모든 지표가 정상으로 보이기 때문에 **원인을 찾는 데 가장 오래 걸리는 고장**입니다. 지금 확실히 켜두세요.

* [ ] 게이트웨이 상태가 "성공"인가
* [ ] VPN 종류가 "경로 기반"인가
* [ ] 공용 IP가 **2개** 할당되었는가
* [ ] **두 IP를 메모했는가** → Lab 6에서 AWS에 입력 ★
* [ ] **허브 피어링에 「전달하도록 허용」을 켰는가** ★
* [ ] **스포크 피어링에 「원격 게이트웨이 사용」을 켰는가** ★

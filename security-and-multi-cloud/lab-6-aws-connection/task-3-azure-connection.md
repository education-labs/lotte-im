# Task 3 - Azure 측 연결 구성 🟦

> 🟦 **이제 Azure 포털로 전환합니다.** 브라우저 탭을 바꾸세요.
>
> ⚠️ **게이트웨이를 새로 만들지 않습니다.** Lab 5에서 만든 `vgw-hub-krc-prod-user**`에 **연결(Connection)을 하나 더 추가**하는 작업입니다.

```
vgw-hub-krc-prod-user**  (Lab 5에서 만든 게이트웨이 — 그대로 사용)
   ├─ 연결 ①  conn-to-idc-user**      → lng-onprem-idc   (선택 사항 · Lab 5 Task 2)
   └─ 연결 ②  conn-azure-to-aws-user** → lng-aws-vpc-user**     ← 지금 추가하는 것
```

## 3-1. LNG 생성 — AWS를 "상대편"으로 등록

1. 포털에서 "로컬 네트워크 게이트웨이" 검색 → [만들기].
2. 리소스 그룹 `rg-network-prod-user**`, 이름 `lng-aws-vpc-user**`, 지역 `Korea Central` 입력.
3. 엔드포인트 **IP 주소** → **③번 값(AWS 터널 1 외부 IP)** 입력.
4. 주소 공간에 **`172.16.**.0/24`**(AWS VPC 대역) 입력.
5. BGP: 사용 안 함 → [검토 + 만들기] → [만들기].

<table header-row="true">
<tr>
<td>항목</td>
<td>값</td>
<td>출처</td>
</tr>
<tr>
<td>이름</td>
<td>lng-aws-vpc-user**</td>
<td>—</td>
</tr>
<tr>
<td>IP 주소</td>
<td>③ AWS 터널 1 외부 IP</td>
<td>Task 2에서 메모 ★</td>
</tr>
<tr>
<td>주소 공간</td>
<td>172.16.**.0/24</td>
<td>AWS VPC 대역</td>
</tr>
</table>

> 💡 Local Network Gateway는 "상대편"을 정의하는 리소스입니다. 여기서는 AWS를 정의합니다. "Local Network Gateway = 상대편"이라는 원칙이 그대로 적용됩니다.
>
> ⚠️ IP 주소에는 **터널의 외부 IP**를 넣습니다. AWS 콘솔의 VGW ID나 VPC ID가 아닙니다.

## 3-2. Lab 5 게이트웨이에 연결 추가

1. 포털에서 **`vgw-hub-krc-prod-user**`** 로 이동합니다 (Lab 5에서 만든 것).
2. 왼쪽 메뉴 [연결] 클릭 → [추가] 클릭.
3. 이름 `conn-azure-to-aws-user**`, 연결 형식 **사이트 간(IPsec)** 선택.
4. 가상 네트워크 게이트웨이가 `vgw-hub-krc-prod-user**`로 고정된 것을 확인합니다.
5. 로컬 네트워크 게이트웨이에서 `lng-aws-vpc-user**` 선택.
6. 공유 키(PSK)에 **④번 값**을 입력합니다 — AWS에서 지정한 값과 **정확히 동일**해야 합니다.
7. IKE 프로토콜 **IKEv2** 선택 후 [확인].
8. 1~3분 뒤 연결 상태가 **"연결됨"** 으로 바뀌는지 확인합니다.

> ⚠️ **PSK 복사 시 앞뒤 공백이 딸려오는 경우가 많습니다.** 붙여넣고 나서 끝에 커서를 두고 공백이 없는지 확인하세요. 이게 안 맞으면 IKE Phase 1에서 실패하고, 증상은 그냥 "연결 중"에서 멈춥니다.

## 3-3. 🟧 AWS로 돌아가 터널 확인

1. 🟧 **AWS 콘솔 탭으로 전환** → VPC → [Site-to-Site VPN 연결] → [터널] 탭.
2. 터널 1의 상태가 **UP** 으로 바뀌었는지 확인합니다.

> 💡 **양쪽에서 모두 확인하는 습관을 들이세요.** 한쪽만 보고 "연결됐다"고 판단하면 안 됩니다. 실무에서 Azure는 "연결됨"인데 AWS는 Down인 상태도 있습니다.

- [ ] Azure 연결 상태가 **"연결됨"** 인가
- [ ] AWS 터널 상태가 **UP** 인가
- [ ] 게이트웨이에 AWS 연결(`conn-azure-to-aws-user**`)이 있는가

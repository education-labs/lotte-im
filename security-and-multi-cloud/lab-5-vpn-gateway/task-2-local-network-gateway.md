# Task 2 - Local Network Gateway 구성 (온프레미스 가정)

실제 IDC 장비가 없으므로 **구성 절차를 익히는 것이 목적**입니다. 값은 가상이고, 연결은 붙지 않습니다.

1. 포털에서 "local network gateway"를 검색해 \[만들기] 클릭.

<figure><img src="../../.gitbook/assets/image (1123).png" alt=""><figcaption></figcaption></figure>





2. 리소스 그룹 `rg-user**`, 이름 `lng-onprem-user**`, 지역 `Korea Central` 입력. \
   엔드포인트 유형: **IP 주소** 선택. \
   IP 주소에 `203.0.113.10`을 입력합니다 (문서용 예약 대역 — 실제 장비 아님). \
   주소 공간에 `192.168.**.0/24`을 입력합니다 (가상의 사내망 대역)

<figure><img src="../../.gitbook/assets/image (1124).png" alt="" width="471"><figcaption></figcaption></figure>



3. BGP 설정: **사용 안 함**. \
   \[검토 + 만들기] → \[만들기].

<figure><img src="../../.gitbook/assets/image (1125).png" alt="" width="375"><figcaption></figcaption></figure>





4. 생성 후 \[구성]에서 값이 올바른지 다시 확인합니다.

<table><thead><tr><th width="150.9090576171875">항목</th><th width="264.45452880859375">값</th><th>의미</th></tr></thead><tbody><tr><td>이름</td><td>lng-onprem-user**</td><td>—</td></tr><tr><td>엔드포인트</td><td>IP 주소</td><td>상대 장비를 IP로 지정</td></tr><tr><td>IP 주소</td><td>203.0.113.10</td><td>상대편(IDC) 장비의 공인 IP</td></tr><tr><td>주소 공간</td><td>192.168.**.0/24</td><td>상대편(IDC)의 내부 대역</td></tr><tr><td>BGP</td><td>사용 안 함</td><td>정적 라우팅</td></tr></tbody></table>

> ⚠️ **이름에 속지 마세요.** "Local"이 붙었지만 **내 것이 아니라 상대편**을 정의하는 리소스입니다. 주소 공간에는 상대편(IDC) 대역을 넣습니다. 여기에 실수로 Azure 대역(`10.**.0.0/16`)을 넣으면 게이트웨이가 자기 자신에게 트래픽을 보내려 해 통신이 깨집니다.
>
> 💡 사내망 대역이 여러 개면 **전부 등록**해야 합니다. 빠뜨린 대역은 통신되지 않고, "일부 서버만 안 된다"는 형태로 나타납니다.

## 연결 만들어 보기

1. `vgw-hub-user**` → \[연결] → \[추가] 클릭

<figure><img src="../../.gitbook/assets/image (1126).png" alt="" width="563"><figcaption></figcaption></figure>



2. 아래 처럼 입력 또는 선택한 뒤 다음을 클릭합니다.

* 연결 형식 : **사이트 간(IPsec)**
* 이름 : `conn-to-onprem-user**`
* 지역 : Korea Central

<figure><img src="../../.gitbook/assets/image (1127).png" alt="" width="373"><figcaption></figcaption></figure>





3. 아래  처럼 입력 및 선택 한뒤 다음 클릭, 다음을 계속 클릭하고 검토 만들기 클릭하여 생성합니다.

* 가상 네트워크 게이트웨이 : vgw-hub-user\*\*
* 로컬 네트워크 게이트웨이 : lng-onprem-user\*\*
* 인증 방법 : 공유키
* 공유키 (PSK) : azuretoonprem



<figure><img src="../../.gitbook/assets/image (1128).png" alt="" width="563"><figcaption></figcaption></figure>





2. 연결 상태가 **"Updating" "알수 없음"** 에 머무르는 것을 확인합니다.

<figure><img src="../../.gitbook/assets/image (1129).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1130).png" alt="" width="563"><figcaption></figcaption></figure>



> 💡 **"연결 중"에서 안 넘어가는 게 정상입니다.** `203.0.113.10`에는 응답할 장비가 없으니까요. 이 상태를 직접 보는 것이 학습 포인트입니다 — 실무에서 터널이 안 붙을 때 보게 될 바로 그 화면입니다.

* [ ] LNG가 생성되었는가
* [ ] 주소 공간이 **IDC 대역(`192.168.x`)** 인가 — Azure 대역을 넣지 않았는가
* [ ] 연결이 "연결 중"에 머무르는 것을 확인했는가

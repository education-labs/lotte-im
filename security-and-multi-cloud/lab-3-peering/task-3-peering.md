# Task 3 - Peering 구성과 검증

1. 허브 VNet → \[피어링] → \[추가]

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>



2. 원격 가상 네트워크를 설정합니다. (인터페이스 아래쪽에 위치할 수도 있습니다.)

* 피어링 링크 이름  :  `peer-spoke-to-hub-user**` &#x20;
* 가상네트워크 : 스포크 VNet

<figure><img src="../../.gitbook/assets/image (1112).png" alt="" width="483"><figcaption></figcaption></figure>



3. 원격 가상 네트워크 피어링  설정에는 아래 항목을 체크합니다.

**① 액세스 허용(기본 체크됨)**

**② 전달된 트래픽 수신 허용**&#x20;

<figure><img src="../../.gitbook/assets/image (1113).png" alt="" width="295"><figcaption></figcaption></figure>





3. 로컬 가상 네트워크를 설정합니다.&#x20;

* 피어링 링크 이름 : `peer-hub-to-spoke-user**`
* **① 액세스 허용(기본 체크됨) ② 전달된 트래픽 수신 허용 체크**

<figure><img src="../../.gitbook/assets/image (1114).png" alt=""><figcaption></figcaption></figure>



3. **두 VNet 모두**에서 피어링 목록을 열어 상태가 "연결됨"인지 확인합니다. \
   새로 만드는 것이 아니라 **결과를 확인**하는 단계입니다.\
   한 번으로 양쪽이 만들어집니다.<br>

<figure><img src="../../.gitbook/assets/image (1115).png" alt=""><figcaption></figcaption></figure>



4. VM1의 네트워크인터페이스에서 유효 경로를 확인합니다.

<figure><img src="../../.gitbook/assets/image (1088).png" alt=""><figcaption></figcaption></figure>



> 📷 **피어링 목록 — 상태 "연결됨"** — 양쪽 VNet 모두에서 피어링이 보이는지 확인합니다.
>
> 💡 **한 번에 양방향이 만들어집니다.** 2·3번에서 이름을 두 개 입력하는 것이 그 때문입니다. 추가 버튼을 누르면 허브에 `peer-hub-to-spoke-user**`, 스포크에 `peer-spoke-to-hub-user**` 가 각각 생깁니다. **반대쪽 VNet에 가서 또 만들지 마세요** — 중복이 되어 오류가 납니다.
>
> ⚠️ **체크박스 네 개 중 ②는 기본이 꺼져 있습니다.** 「액세스 허용」하나만 체크된 채 넘어가기 쉬운데, 허브에 방화벽을 두는 이 구조에서는 **「전달된 트래픽 수신 허용」이 켜져 있어야** AWS·온프레미스에서 돌아오는 패킷을 스포크가 받아줍니다. 그런 패킷은 출발지가 두 VNet 어느 쪽에도 없는 주소라, 이 설정이 없으면 조용히 버려집니다.
>
> 아래 두 개는 **게이트웨이 관련**이라 지금은 켤 수 없습니다. 바로 앞 Task 2에서 배포를 걸었지만 **아직 끝나지 않았기 때문**입니다. **Lab 5 Task 1에서 배포 완료를 확인한 뒤 이 화면으로 돌아와 켭니다.** 이걸 안 켜면 터널이 붙어도 AWS에서 돌아오는 트래픽이 스포크를 찾지 못합니다.

* [ ] 양쪽 피어링 상태가 "연결됨"인가
* [ ] **두 링크 모두 「전달된 트래픽 수신 허용」이 켜져 있는가** ★
* [ ] 유효 경로에 허브 대역이 추가되었는가
* [ ] 그 경로의 원본이 `VNetPeering` 인가
* [ ] (Lab 5 배포 완료 후) 게이트웨이 전송 설정을 켤 것 — 지금은 아직입니다

> ⚠️ 피어링을 걸어도 스포크끼리는 통신되지 않습니다. Peering은 전이되지 않기 때문이며, Lab 4에서 UDR로 해결합니다.

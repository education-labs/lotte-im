# Task 2 - 방화벽 규칙 구성

네트워크 규칙과 애플리케이션 규칙을 각각 만듭니다.

1. 방화벽 정책(`afwp-corp-base-user**`) → \[네트워크 규칙] → \[규칙 컬렉션 추가]

<figure><img src="../../.gitbook/assets/image (1101).png" alt="" width="563"><figcaption></figcaption></figure>



2. 스포크 간 통신 허용 규칙을 만듭니다. — 컬렉션 이름 `rc-net-spoke`, **우선순위 `200`**, 작업 `허용`, 규칙컬렉션그룹 DefaultNetworkRuleCollectionGroup 을 먼저 입력 및 선택한 뒤 규칙을 추가합니다.

<table><thead><tr><th width="87.6363525390625">종류</th><th width="617.7273559570312">내용</th></tr></thead><tbody><tr><td>네트워크</td><td> 이름 : rule,  원본 : 10.**.8.0/22, 프로토콜 : 모두, 대상 포트 : *, 대상 : 10.**.8.0/22</td></tr></tbody></table>

<figure><img src="../../.gitbook/assets/image (1102).png" alt=""><figcaption></figcaption></figure>





3. \[애플리케이션 규칙] → \[규칙 컬렉션 추가] \
   아웃바운드를 특정 FQDN만 허용하도록 구성합니다. — 컬렉션 이름 `rc-app-outbound`, **우선순위 `300`**, 작업 `허용`&#x20;

**규칙 내용**

<table><thead><tr><th width="129.09088134765625">종류</th><th>내용</th></tr></thead><tbody><tr><td>애플리케이션</td><td>이름 : rule1, 원본 : 10.**.8.0/22, 프로토콜 : https:443, 대상 : *.ubuntu.com</td></tr><tr><td>애플리케이션</td><td>이름 : rule2, 원본 : 10.**.8.0/22, 프로토콜 : https:443, 대상 : *.canonical.com</td></tr></tbody></table>

<figure><img src="../../.gitbook/assets/image (1103).png" alt=""><figcaption></figcaption></figure>





> ⚠️ 네트워크 규칙에서 443을 넓게 허용하면 애플리케이션 규칙(FQDN)이 평가되지 않습니다. 도메인 필터링을 검증하려면 네트워크 규칙을 좁게 유지하세요.
>
> ⚠️ **입력할 때 주의 사항 3가지**
>
> 1. **우선순위는 규칙 컬렉션 단위로 입력**합니다 (100\~65000). 빈칸으로 두면 저장이 안 됩니다. 네트워크 200 · 애플리케이션 300으로 간격을 두면 나중에 사이에 규칙을 끼워 넣을 수 있습니다.
> 2. **프로토콜을 반드시 지정**합니다. 네트워크 규칙은 `Any`(또는 TCP/UDP/ICMP 선택), 애플리케이션 규칙은 **`https:443`** 형식으로 입력해야 합니다. 비워두면 규칙이 만들어지지 않습니다.
> 3. **대상 FQDN은 한 줄에 하나씩** 입력합니다. 콤마로 이어 붙이지 마세요.
>
> ```
> *.ubuntu.com
> *.canonical.com
> ```

* [ ] 규칙 컬렉션 2개가 생성되었는가
* [ ] 우선순위 값을 확인했는가
* [ ] 네트워크 규칙이 과도하게 넓지 않은가


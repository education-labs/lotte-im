# Task 4 - NSG 생성과 규칙 구성

1. "네트워크 보안 그룹"을 검색해 서비스로 이동 후 [만들기]
2. `nsg-web-userNN` · `nsg-app-userNN` · `nsg-db-userNN` 세 개를 만듭니다.
3. 각 NSG의 [인바운드 보안 규칙]에서 규칙을 추가합니다.

**규칙 구성**

<table header-row="true">
<tr>
<td>NSG</td>
<td>우선순위</td>
<td>출발지</td>
<td>포트</td>
<td>동작</td>
</tr>
<tr>
<td>nsg-web-userNN</td>
<td>110</td>
<td>Internet</td>
<td>443</td>
<td>허용</td>
</tr>
<tr>
<td>nsg-web-userNN</td>
<td>120</td>
<td>내 IP</td>
<td>22</td>
<td>허용</td>
</tr>
<tr>
<td>nsg-app-userNN</td>
<td>110</td>
<td>10.NN.8.0/24</td>
<td>8080</td>
<td>허용</td>
</tr>
</table>

4. 규칙을 만든 뒤 [서브넷] 메뉴에서 각 서브넷에 연결합니다.
   `nsg-web-userNN → snet-web`, `nsg-app-userNN → snet-app`, `nsg-db-userNN → snet-db`

> ⚠️ ICMP를 허용해야 ping 테스트가 됩니다. 실습용으로 각 NSG에 **ICMP 허용 규칙**을 하나씩 추가하세요 (우선순위는 위 표의 규칙들보다 뒤 번호로, 예: `130`).

- [ ] NSG 3개가 각 서브넷에 연결되었는가
- [ ] 기본 규칙 65500 `DenyAllInBound` 가 보이는가
- [ ] ICMP 허용 규칙을 추가했는가

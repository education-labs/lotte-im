# Task 1 - 대역 설계표 작성

리소스를 만들기 전에 워크시트를 먼저 작성합니다.

### ⭐ 내 대역 배정표 — 따로 정리해두고 시작합니다

아래 표의 **문서 예시**를 보고 **내 값** 칸을 채우세요. `NN` 자리에 배정받은 번호를 넣으면 됩니다 (user01 → `1`, user12 → `12`).

<table header-row="true">
<colgroup>
<col>
<col>
<col width="148">
</colgroup>
<tr>
<td>항목</td>
<td>내 값 (형식)</td>
<td>내 값 예시</td>
</tr>
<tr>
<td>내 번호</td>
<td>userNN</td>
<td>user16</td>
</tr>
<tr>
<td>Azure 상위 대역</td>
<td>10.NN.0.0/16</td>
<td>10.16.0.0/16</td>
</tr>
<tr>
<td>Hub VNet</td>
<td>10.NN.0.0/24</td>
<td>10.16.0.0/24</td>
</tr>
<tr>
<td>Spoke VNet</td>
<td>10.NN.8.0/22</td>
<td>10.16.8.0/22</td>
</tr>
<tr>
<td>snet-web</td>
<td>10.NN.8.0/24</td>
<td>10.16.8.0/24</td>
</tr>
<tr>
<td>snet-app</td>
<td>10.NN.9.0/24</td>
<td>10.16.9.0/24</td>
</tr>
<tr>
<td>snet-db</td>
<td>10.NN.10.0/24</td>
<td>10.16.10.0/24</td>
</tr>
<tr>
<td>snet-mgmt</td>
<td>10.NN.11.0/24</td>
<td>10.16.11.0/24</td>
</tr>
<tr>
<td>방화벽 사설 IP (Lab 3)</td>
<td>10.NN.0.4</td>
<td>10.16.0.4</td>
</tr>
<tr>
<td>AWS VPC (Lab 5)</td>
<td>172.16.NN.0/24</td>
<td></td>
</tr>
<tr>
<td>AWS 서브넷 (Lab 5)</td>
<td>172.16.NN.0/25</td>
<td></td>
</tr>
<tr>
<td>IDC(가정)</td>
<td>192.168.NN.0/24</td>
<td></td>
</tr>
</table>

- [ ] 세 곳(Azure·AWS·IDC)의 대역이 서로 겹치지 않는가
- [ ] 허브에 특수 서브넷을 담을 여유가 있는가

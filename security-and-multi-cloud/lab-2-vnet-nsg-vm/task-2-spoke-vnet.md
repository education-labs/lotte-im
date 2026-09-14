# Task 2 - 스포크 VNet과 서브넷 생성

1. 포털 상단 검색창에 "가상 네트워크"를 입력하고 서비스로 이동합니다.
2. [만들기]를 클릭합니다.
3. 리소스 그룹을 새로 만듭니다. (`rg-network-prod-user**`)
4. 이름과 지역을 입력합니다.
5. [IP 주소] 탭에서 주소 공간을 `10.**.8.0/22` 로 설정합니다.
6. 서브넷을 네 개 추가합니다. (web / app / db / mgmt)
7. [검토 + 만들기] → [만들기]

**입력값**

<table header-row="true">
<tr>
<td>항목</td>
<td>값</td>
</tr>
<tr>
<td>리소스 그룹</td>
<td>rg-network-prod-user**</td>
</tr>
<tr>
<td>이름</td>
<td>vnet-spoke-app-krc-prod-user**</td>
</tr>
<tr>
<td>지역</td>
<td>Korea Central</td>
</tr>
<tr>
<td>주소 공간</td>
<td>10.**.8.0/22</td>
</tr>
<tr>
<td>서브넷</td>
<td>snet-web 10.**.8.0/24 · snet-app 10.**.9.0/24 · snet-db 10.**.10.0/24 · snet-mgmt 10.**.11.0/24</td>
</tr>
</table>

- [ ] 스포크 VNet과 서브넷 4개가 생성되었는가

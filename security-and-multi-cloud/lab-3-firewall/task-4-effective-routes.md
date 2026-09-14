# Task 4 - 유효 경로 변화 확인

Lab 1 Task 6에서 캡처한 화면과 비교합니다.

1. VM1 → 네트워크 인터페이스 → [유효 경로] 를 다시 엽니다.
2. `0.0.0.0/0` 의 다음 홉 유형을 확인합니다.
3. Lab 1 Task 6 캡처와 비교합니다.

**무엇이 바뀌었나**

<table header-row="true">
<tr>
<td>경로</td>
<td>UDR 전 (Lab 1)</td>
<td>UDR 후 (지금)</td>
</tr>
<tr>
<td>0.0.0.0/0</td>
<td>다음 홉 : Internet · 원본 : Default</td>
<td>다음 홉 : VirtualAppliance 10.NN.0.4 · 원본 : User</td>
</tr>
<tr>
<td>10.NN.8.0/22</td>
<td>다음 홉 : VirtualNetwork</td>
<td>변화 없음</td>
</tr>
<tr>
<td>10.NN.0.0/24</td>
<td>다음 홉 : VNetPeering</td>
<td>변화 없음</td>
</tr>
</table>

> 📷 **[유효 경로] — UDR 적용 후** — `0.0.0.0/0` 의 원본이 `User`, 다음 홉이 `VirtualAppliance` 로 바뀐 것을 확인합니다. Lab 1 Task 6 캡처와 나란히 놓고 비교하세요.

- [ ] `0.0.0.0/0` 의 원본이 `User` 로 바뀌었는가
- [ ] 다음 홉이 `VirtualAppliance` 인가
- [ ] 주소가 방화벽 사설 IP와 일치하는가
- [ ] 허브·VNet 경로는 그대로인가

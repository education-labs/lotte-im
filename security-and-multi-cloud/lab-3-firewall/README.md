# Lab 3 - Hub-Spoke + Azure Firewall + 경로 테이블

> ⚠️ **이 페이지의 이름·대역은 예시입니다.** `afw-hub-krc-prod-userNN` → `afw-hub-내번호`, `10.10.x` → `10.내번호.x` 로 **바꿔서 입력**하세요.

Lab 1~2에서 만든 Hub-Spoke 구조를 실제로 동작시킵니다.

1. Azure Firewall 배포 — 허브의 `AzureFirewallSubnet`
2. 방화벽 정책 구성 — 네트워크 규칙 · 애플리케이션 규칙
3. 경로 테이블 생성 — `0.0.0.0/0` → 방화벽 사설 IP
4. 서브넷에 연결 — 스포크 서브넷들
5. 허용·차단 검증 — ping · curl · 로그 확인

> ⚠️ Azure Firewall은 배포에 10~20분이 걸리고 시간당 요금이 발생합니다. 먼저 배포를 시작한 뒤 방화벽 정책 내용을 검토하세요. 실습 종료 후에도 삭제하지 마세요 — Lab 5까지 사용합니다.

**완성 후 트래픽 흐름**

<table header-row="true">
<tr>
<td>흐름</td>
<td>경로</td>
<td>통제 지점</td>
</tr>
<tr>
<td>스포크 → 인터넷</td>
<td>VM → UDR → Azure Firewall → 인터넷</td>
<td>애플리케이션 규칙 (FQDN)</td>
</tr>
<tr>
<td>스포크 → 스포크</td>
<td>VM → UDR → Azure Firewall → 상대 VM</td>
<td>네트워크 규칙</td>
</tr>
<tr>
<td>스포크 → 허브</td>
<td>피어링 시스템 경로 (방화벽 미경유)</td>
<td>—</td>
</tr>
</table>

## Task 목록

* [Task 1 - Azure Firewall 배포](task-1-firewall-deploy.md)
* [Task 2 - 방화벽 규칙 구성](task-2-firewall-rules.md)
* [Task 3 - 경로 테이블 생성과 연결](task-3-route-table.md)
* [Task 4 - 유효 경로 변화 확인](task-4-effective-routes.md)
* [Task 5 - 허용·차단 검증](task-5-verify.md)

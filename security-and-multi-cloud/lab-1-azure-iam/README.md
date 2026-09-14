# Lab 1 - Azure IAM

교재 **Chapter 2. Azure IAM** 의 내용을 포털에서 직접 확인하는 실습입니다. 「누가 접속하는가(Entra ID)」와 「무엇을 할 수 있는가(Azure RBAC)」를 나눠서 다룹니다.

> ⚠️ **이 Lab의 이름은 예시입니다.** `user**`, `sec-user**` 등에서 `**` 자리에 배정받은 번호를 넣어 만드세요. 여러 명이 같은 테넌트·구독을 공유하므로 이름이 겹치면 서로의 리소스를 건드리게 됩니다.

## 이 Lab에서 확인할 것

Azure에는 **완전히 별개인 권한 시스템이 두 개** 있습니다. 이 Lab의 모든 Task는 결국 이 한 장의 그림을 확인하는 과정입니다.

<table header-row="true">
<tr>
<td>구분</td>
<td>Entra 역할 (디렉터리 역할)</td>
<td>Azure RBAC 역할</td>
</tr>
<tr>
<td>관리 대상</td>
<td>Entra ID 자체 — 사용자·그룹·앱·도메인</td>
<td>Azure 리소스 — VM·스토리지·네트워크·구독</td>
</tr>
<tr>
<td>예시 역할</td>
<td>전역 관리자, 사용자 관리자</td>
<td>소유자, 기여자, 읽기 권한자</td>
</tr>
<tr>
<td>범위</td>
<td>테넌트 / 관리 단위(AU)</td>
<td>관리 그룹 / 구독 / RG / 리소스</td>
</tr>
</table>

> ❗ **'구독 소유자'여도 새 사용자 계정은 못 만듭니다** — 그건 Entra 역할(사용자 관리자)의 일입니다. 반대로 전역 관리자여도 기본적으로는 구독 리소스를 못 봅니다. AWS에서는 IAM 하나로 다 했기 때문에 이 이중 구조가 처음엔 가장 헷갈리는 지점입니다.

## 사전 준비

- Azure 포털 로그인 (`https://portal.azure.com`)
- 실습용 **리소스 그룹** `rg-iam-user**` (Korea Central) — Task 3에서 사용
- 사용자·그룹을 만들려면 **사용자 관리자(User Administrator)** 이상의 Entra 역할이 필요합니다. 권한이 없으면 강사가 만들어 둔 계정으로 Task 3부터 진행하세요.

## Task 목록

* [Task 1 - Entra ID 테넌트 둘러보기](task-1-entra-id-tour.md)
* [Task 2 - 사용자와 그룹 만들기](task-2-users-and-groups.md)
* [Task 3 - RBAC 역할 할당 (3요소)](task-3-rbac-assignment.md)
* [Task 4 - 역할 차이와 상속 검증](task-4-role-verification.md)
* [Task 5 - 관리 평면 vs 데이터 평면 (Reader의 함정)](task-5-control-vs-data-plane.md)
* [Task 6 - 앱 등록 · 서비스 주체 · 관리 ID](task-6-app-and-managed-identity.md)

# Task 3 - RBAC 역할 할당 (3요소)

**역할 할당**이란 권한을 부여하기 위해 **「누구에게 + 어떤 역할을 + 어느 범위에서」** 세 가지를 정해 묶는 묶음입니다.

<table header-row="true">
<tr>
<td>요소</td>
<td>무엇을 고르나</td>
<td>이번 Task에서</td>
</tr>
<tr>
<td>① 누구에게 (보안 주체)</td>
<td>사용자 · 그룹 · 서비스 주체 · 관리 ID</td>
<td>sec-dev-user** / sec-view-user** 그룹</td>
</tr>
<tr>
<td>② 어떤 역할 (역할 정의)</td>
<td>'기여자', '읽기 권한자' 등 허용 동작의 묶음</td>
<td>기여자 / 읽기 권한자</td>
</tr>
<tr>
<td>③ 어느 범위 (범위)</td>
<td>관리 그룹 · 구독 · 리소스 그룹 · 개별 리소스</td>
<td>rg-iam-user** (리소스 그룹)</td>
</tr>
</table>

## 3-1. 실습용 리소스 그룹 준비

1. 포털에서 `리소스 그룹` → [만들기].
2. 이름 `rg-iam-user**`, 지역 `Korea Central` → [검토 + 만들기].

## 3-2. 그룹에 역할 할당

1. `rg-iam-user**` → 왼쪽 메뉴 **[액세스 제어(IAM)]** → [추가] → **역할 할당 추가**.
2. 아래 두 건을 각각 만듭니다.

<table header-row="true">
<tr>
<td>① 누구에게</td>
<td>② 어떤 역할</td>
<td>③ 어느 범위</td>
</tr>
<tr>
<td>sec-dev-user** (그룹)</td>
<td>기여자 (Contributor)</td>
<td>rg-iam-user**</td>
</tr>
<tr>
<td>sec-view-user** (그룹)</td>
<td>읽기 권한자 (Reader)</td>
<td>rg-iam-user**</td>
</tr>
</table>

3. [액세스 제어(IAM)] → **[역할 할당]** 탭에서 방금 만든 두 건이 보이는지 확인합니다.

> 💡 **개인이 아니라 그룹에 할당했습니다.** 사람이 바뀌면 그룹 멤버만 갈아끼우면 되고, 역할 할당은 건드릴 필요가 없습니다. 이것이 권한을 그룹에 주는 이유입니다.

## 3-3. 자주 쓰는 기본 역할 확인

[역할 할당 추가] 화면에서 역할 목록을 검색해 아래 역할들의 설명을 읽어봅니다. 역할은 수백 개지만 실무의 대부분은 이 몇 개입니다.

<table header-row="true">
<tr>
<td>역할</td>
<td>무엇을 할 수 있나</td>
<td>AWS에서는?</td>
</tr>
<tr>
<td>소유자 (Owner)</td>
<td>리소스 관리 + 다른 사람에게 권한 주기(위임)까지 가능</td>
<td>AdministratorAccess</td>
</tr>
<tr>
<td>기여자 (Contributor)</td>
<td>리소스는 다 만들고 바꾸지만, 권한은 못 준다</td>
<td>PowerUserAccess</td>
</tr>
<tr>
<td>읽기 권한자 (Reader)</td>
<td>보기만 가능</td>
<td>ReadOnlyAccess</td>
</tr>
<tr>
<td>사용자 액세스 관리자</td>
<td>리소스는 못 보고, 권한 할당만 담당</td>
<td>IAM 전담자</td>
</tr>
<tr>
<td>가상 머신 기여자 등</td>
<td>특정 서비스에만 한정된 역할 (VM만 / 네트워크만 …)</td>
<td>서비스 한정 정책</td>
</tr>
</table>

- [ ] `rg-iam-user**` 에 역할 할당 2건이 만들어졌는가
- [ ] 할당 대상이 **개인 사용자가 아니라 그룹**인가
- [ ] 소유자와 기여자의 차이(권한 위임 가능 여부)를 설명할 수 있는가

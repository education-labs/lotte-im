# Task 5 - 관리 평면 vs 데이터 평면

> **"'읽기 권한자'를 줬는데 스토리지 안의 파일이 안 열린다?"**
>
> Azure 권한은 **'리소스 설정을 보는 권한'** 과 **'리소스 안의 데이터를 보는 권한'** 이 별개입니다.

<table><thead><tr><th width="131.0909423828125">구분</th><th>관리 평면 (Control Plane)</th><th>데이터 평면 (Data Plane)</th></tr></thead><tbody><tr><td>무엇을 보나</td><td>스토리지 계정이 존재한다 / 설정이 어떻다 — 여기까지만</td><td>Blob 파일 내용, Key Vault 비밀 값 등 '안에 든 것'</td></tr><tr><td>해당 역할</td><td>Reader, Contributor, Owner</td><td>Storage Blob Data Reader / Contributor 등 별도 역할</td></tr></tbody></table>

## 5-1. 스토리지 계정과 Blob 준비 (관리자 계정)

1. `rg-iam-user**` 안에 **스토리지 계정** `stiamuser**` 을 만듭니다 (이름은 소문자+숫자만, 하이픈 불가).

<figure><img src="../../.gitbook/assets/image (1029).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1030).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1031).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1032).png" alt=""><figcaption></figcaption></figure>



2. \[데이터 스토리지] → \[컨테이너] → \[+ 컨테이너] → 이름 `demo` 로 생성.

<figure><img src="../../.gitbook/assets/image (1033).png" alt=""><figcaption></figcaption></figure>





2. `demo` 컨테이너에 아무 텍스트 파일 하나를 업로드합니다.

<figure><img src="../../.gitbook/assets/image (1034).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1035).png" alt=""><figcaption></figcaption></figure>

##

## 5-2. 함정 재현 — Reader로는 안 열린다

1. 시크릿 창에서 `view-user**` (읽기 권한자) 으로 로그인.
2. `stiamuser**` 스토리지 계정으로 이동 → **계정 자체와 설정은 잘 보입니다.** (= 관리 평면 권한 있음)

<figure><img src="../../.gitbook/assets/image (1036).png" alt=""><figcaption></figcaption></figure>



3. \[컨테이너] → `demo` 클릭 → 파일 목록에서 **권한 오류**가 뜨는 것을 확인합니다.

<figure><img src="../../.gitbook/assets/image (1037).png" alt=""><figcaption></figcaption></figure>





## 5-3. 데이터 평면 역할 부여 후 재확인

1. 관리자 계정으로 돌아와 `stiamuser**` → \[액세스 제어(IAM)] → \[역할 할당 추가]

<figure><img src="../../.gitbook/assets/image (1038).png" alt="" width="563"><figcaption></figcaption></figure>





2. **Storage Blob 데이터 Reader** 역할을 `sec-view-user**` 그룹에 할당합니다 (범위: 이 스토리지 계정).

<figure><img src="../../.gitbook/assets/image (1039).png" alt="" width="563"><figcaption></figcaption></figure>



3. 다시 `view-user**` 으로 로그인 → `demo` 컨테이너의 파일 목록이 **보이고**, 파일 내용도 열립니다.

<figure><img src="../../.gitbook/assets/image (1040).png" alt=""><figcaption></figcaption></figure>

> ⚠️ **반영에 1\~3 분 걸릴 수 있습니다.** 바로 안 되면 로그아웃 후 재로그인하세요.
>
> 💡 **Key Vault · Cosmos DB · Service Bus도 똑같이 두 층으로 나뉩니다.** 데이터가 필요하면 데이터 평면 역할을 잊지 마세요. 예를 들어 Key Vault의 비밀 값을 읽으려면 `Key Vault Secrets User` 가 필요합니다.

* [ ] Reader로 스토리지 **계정 설정은 보이는데** 컨테이너 내용은 안 보였는가
* [ ] `Storage Blob 데이터 Reader` 부여 후 파일이 열렸는가
* [ ] 관리 평면 / 데이터 평면을 구분해 설명할 수 있는가

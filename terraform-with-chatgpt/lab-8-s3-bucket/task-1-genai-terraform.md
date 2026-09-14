# Task 1 - GenAI 기반 Terraform  코드 생성

1. 디렉토리 생성

```
mkdir ~/environment/s3
```

{% hint style="info" %}
사전 구성 리소스

없음. S3는 리전 단위 리소스이고 VPC에 속하지 않으므로, 사전 조건 없이 이 Lab 하나만으로 완결됩니다.
{% endhint %}

2. 다음 조건을 모두 만족하는 S3 버킷을 구성

{% hint style="info" icon="label" %}
* 버킷 이름은 `user##-assets-bucket-<계정ID>` 형식으로, 전역에서 유일해야 한다(계정 ID는 `data "aws_caller_identity"`로 조회).
* 버전 관리(Versioning)가 활성화되어 있어야 한다.
* 기본 암호화(SSE-S3, `AES256`)가 설정되어 있어야 한다.
* ACL을 통한 공개는 계속 차단하되(`block_public_acls`, `ignore_public_acls`는 `true`), **버킷 정책을 통한 공개만** 허용하도록 Public Access Block을 부분적으로 완화한다(`block_public_policy`, `restrict_public_buckets`를 `false`로).
* 버킷 정책에 두 가지 규칙을 함께 넣는다: ① HTTPS가 아닌 요청은 전부 `Deny`, ② 객체에 대한 `s3:GetObject`는 익명 사용자에게 `Allow`.
* 버킷 정책은 Public Access Block이 먼저 완화된 뒤에 생성되도록 `depends_on`을 명시한다.
* 이미지 파일 1개(`banner.jpg`)를 업로드한다(`content_type = "image/jpeg"`).
* 버전 관리로 쌓이는 이전 버전 객체가 90일 후 자동 삭제되는 라이프사이클 규칙을 둔다.
* 업로드한 이미지의 퍼블릭 URL을 output으로 노출한다.
{% endhint %}

{% hint style="success" %}
상세스펙
{% endhint %}

<table><thead><tr><th width="180">항목</th><th>요구사항</th></tr></thead><tbody><tr><td>버킷 이름</td><td><code>user##-assets-bucket-&#x3C;account_id></code></td></tr><tr><td>Versioning</td><td><code>Enabled</code></td></tr><tr><td>기본 암호화</td><td>SSE-S3(<code>AES256</code>)</td></tr><tr><td>Public Access Block</td><td><code>block_public_acls=true</code>, <code>ignore_public_acls=true</code>, <code>block_public_policy=false</code>, <code>restrict_public_buckets=false</code></td></tr><tr><td>Bucket Policy</td><td>① <code>aws:SecureTransport=false</code> 전체 Deny / ② <code>Principal:*</code>, <code>Action:s3:GetObject</code>, 버킷 내 전체 객체에 Allow</td></tr><tr><td>업로드 객체</td><td>로컬 <code>banner.jpg</code>를 <code>aws_s3_object</code>로 업로드, <code>content_type=image/jpeg</code></td></tr><tr><td>Lifecycle Rule</td><td>id <code>expire-old-versions</code>, 이전 버전 90일 후 만료</td></tr><tr><td>태그</td><td><code>Name=user##-assets-bucket</code>, <code>Environment=lab</code></td></tr><tr><td>output</td><td><code>image_url</code> — 퍼블릭 URL</td></tr></tbody></table>

3. 생성형 AI에게 질의하여 코드를 생성

{% hint style="warning" %}
**주의:** 스펙을 통째로 복사-붙여넣기 하지 말고, 자연어로 재구성해서 프롬프트를 작성해보세요.

"퍼블릭 액세스 차단을 풀어줘"라고만 하면 ChatGPT가 4개 옵션을 전부 꺼버리는 경우가 있습니다. ACL 관련 2개(`block_public_acls`, `ignore_public_acls`)는 계속 막아야 한다고 정확히 지정하세요.

"최신 AWS Provider(v5 이상) 문법으로, `versioning`/`server_side_encryption_configuration`을 `aws_s3_bucket` 안에 인라인으로 넣지 말고 별도 리소스로 분리해줘"라는 조건도 반드시 포함하세요. 안 그러면 ChatGPT가 구버전 문법으로 답을 줄 수 있습니다.
{% endhint %}

4. `banner.jpg`(아무 jpg 파일이나 무방)를 `~/environment/s3` 폴더에 준비
5. 생성한 코드를 C9의 파일로 생성
6. 코드 내용을 넣고, Ctrl + S 키를 입력
7. 파일명을 임의로 지정하고, **s3 디렉토리를 선택**한 뒤 Save 클릭

{% hint style="info" %}
코드 내 user## 가 있다면 ## 에는 유저넘버를 입력합니다.
{% endhint %}


---
hidden: true
---

# 항목 설명

**버킷 이름에 계정 ID를 붙이는 이유**: S3 버킷 이름은 AWS 전체에서 유일해야 합니다(특정 계정 안에서만 유일하면 되는 다른 리소스와 다름). `user05-assets-bucket`처럼 단순한 이름은 이미 다른 AWS 고객이 쓰고 있을 가능성이 있어 `BucketAlreadyExists` 오류가 날 수 있습니다. `data "aws_caller_identity"`로 조회한 계정 ID를 이름 뒤에 붙이면 사실상 전역 유일성이 보장됩니다.

**Versioning·암호화가 버킷 리소스 밖으로 분리된 이유**: 예전 AWS Provider(v3 이하)에서는 `aws_s3_bucket` 리소스 안에 `versioning { enabled = true }`, `server_side_encryption_configuration { ... }`를 인라인 블록으로 넣었습니다. 하지만 v4 이후부터는 이 설정들이 각각 `aws_s3_bucket_versioning`, `aws_s3_bucket_server_side_encryption_configuration`이라는 **별도의 리소스**로 분리됐습니다. ChatGPT는 학습 데이터에 구버전 문법이 많이 섞여 있어 종종 예전 방식으로 답을 주는데, 최신 Provider에서는 이 인라인 인자들이 무시되거나 오류가 납니다.

**퍼블릭인데 왜 계속 보안 설정을 유지하는가**: "이미지를 공개한다"는 것과 "버킷을 아무렇게나 방치한다"는 것은 다릅니다. 버전 관리·암호화·HTTPS 강제는 "누가 접근할 수 있는가"와는 무관하게 항상 켜두는 게 좋은 기본기입니다. 반면 접근 범위(누구에게 무엇을 허용할지)는 목적에 맞게 최소한으로 열어야 합니다. 이번 Lab에서는 ACL 경로는 계속 막아두고, 버킷 정책으로 `s3:GetObject`(읽기)만 정확히 허용해서 "필요한 만큼만 연다"는 원칙을 지킵니다.

**한 정책 안에 Deny와 Allow를 같이 넣는 이유**: AWS 정책 평가에서는 **명시적 Deny가 항상 Allow보다 우선**합니다. 그래서 "HTTPS가 아니면 전부 거부"와 "HTTPS로 오는 GetObject는 누구에게나 허용"을 같은 정책 문서에 Statement 두 개로 넣어도 충돌하지 않습니다 — HTTP로 오면 첫 번째 Deny에 걸려 무조건 차단되고, HTTPS로 온 GetObject만 두 번째 Allow로 통과합니다.

**Public Access Block과 Bucket Policy의 생성 순서(`depends_on`)**: `block_public_policy = true`인 상태에서 퍼블릭 읽기를 허용하는 버킷 정책을 붙이려고 하면, AWS가 "BlockPublicPolicy 설정 때문에 퍼블릭 정책을 거부한다"는 오류를 반환합니다. 즉 Public Access Block이 먼저 완화된 뒤에 정책이 적용되어야 합니다. Terraform은 두 리소스가 같은 버킷을 가리킨다는 것만으로는 이 순서를 자동으로 추론하지 못하므로, `aws_s3_bucket_policy`에 `depends_on = [aws_s3_bucket_public_access_block...]`을 명시해야 합니다.

**이전 버전(noncurrent version) 라이프사이클 규칙을 두는 이유**: 버전 관리를 켜두면 `banner.jpg`를 새로 업로드할 때마다 이전 파일이 지워지지 않고 "이전 버전"으로 계속 쌓입니다. 이걸 방치하면 실제로 쓰지도 않는 옛날 이미지들이 스토리지 비용을 계속 잡아먹습니다. `noncurrent_version_expiration`으로 일정 기간(여기서는 90일)이 지난 이전 버전을 자동 삭제하도록 설정해두면 버전 관리의 이점(실수로 덮어썼을 때 복구 가능)은 유지하면서 비용은 통제할 수 있습니다.

**이미지가 Lab8에서 쓰이는 방식**: Lab8의 타겟그룹 EC2는 부팅 스크립트(`user_data`)에서 이 버킷의 퍼블릭 URL로 `curl`(또는 `wget`)만 하면 이미지를 받아올 수 있습니다. IAM 자격 증명이나 AWS CLI 설정 없이 일반 HTTPS GET 요청으로 끝나기 때문에 EC2 쪽 코드가 매우 단순해집니다.

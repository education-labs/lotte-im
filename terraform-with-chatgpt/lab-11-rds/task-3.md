# Task 3 - 리소스 삭제 및 심화

1. 아래 명령으로 기존 생성한 리소스 삭제

```
terraform destroy -auto-approve
```

{% hint style="info" %}
`skip_final_snapshot=true`, `deletion_protection=false`로 만들어뒀기 때문에 별도 옵션 없이 바로 삭제됩니다. 다만 RDS는 삭제도 몇 분 걸릴 수 있습니다.
{% endhint %}

2. GenAI 에게 코드 내용을 분리하여 파일 생성

{% hint style="info" %}
main.tf : VPC 모듈 호출 + RDS 리소스 블럭

provider.tf : 프로바이더 설정 블럭

variables.tf : 변수 선언(비밀번호 포함)

outputs.tf : 출력 값
{% endhint %}

이때, outputs 에는 `db_endpoint`, `db_instance_id`, `db_subnet_group_name`, `db_parameter_group_name` 을 출력합니다.





3. GenAI 에게 vars 파일 활용하도록 rds.tfvars(파일명은 예시) 파일 생성
4. 해당 코드를 활용하는 간단한 README.md 파일 생성 (GenAI)
5. 심화의 내용으로 만든 코드들도 Task2 를 참고하여 리소스 생성

```
terraform init
terraform fmt -recursive
terraform validate
terraform plan
terraform apply
```

{% hint style="info" %}
apply 시 변수 적용 하여 실행 (`db_password`는 `-var` 또는 tfvars로 전달)
{% endhint %}

6. CLI/콘솔에서 확인
7. 리소스 정리

```
terraform destroy -auto-approve
```

{% hint style="info" %}
이 Lab은 독립 폴더이므로 여기서 정리해도 다른 Lab에는 영향이 없습니다.
{% endhint %}

{% hint style="info" icon="puzzle" %}
도전!

**Multi-AZ로 전환**: `multi_az = true`로 바꿔서 apply해보고, `describe-db-instances`의 `MultiAZ` 값과 콘솔의 대기(standby) 인스턴스 정보를 확인해보세요. 비용이 약 2배로 늘어난다는 점도 함께 정리해보세요.
{% endhint %}


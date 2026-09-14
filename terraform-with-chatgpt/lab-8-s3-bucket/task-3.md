# Task 3 - 리소스 삭제 및 심화

1. 아래 명령으로 기존 생성한 리소스 삭제

```
terraform destroy -auto-approve
```

2. GenAI 에게 코드 내용을 분리하여 파일 생성

{% hint style="info" %}
main.tf : 리소스 블럭

provider.tf : 프로바이더 설정 블럭

variables.tf : 변수선언

outputs.tf : 출력 값
{% endhint %}

이때, outputs 에는 `bucket_id`, `bucket_arn`, `bucket_regional_domain_name`, `image_url` 을 출력합니다.

3. GenAI 에게 vars 파일 활용하도록 s3.tfvars(파일명은 예시) 파일 생성
4. 해당 코드를 활용하는 간단한 README.md 파일 생성 (GenAI)
5. 심화의 내용으로 만든 코드들도 Task2 를 참고하여 리소스 생성

```
terraform init
terraform fmt
terraform validate
terraform plan
terraform apply
```

{% hint style="info" %}
apply 시 변수 적용 하여 실행
{% endhint %}

6. CLI/콘솔에서 확인
7. 리소스 정리는 하지않는다. (이후 실습에 활용 예정)


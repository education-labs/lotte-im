# Task 3 - 리소스 삭제 및 심화

## Task 3 - 리소스 삭제 및 심화

1. 아래 명령으로 기존 생성한 리소스 삭제

```
terraform destroy -auto-approve
```

{% hint style="info" %}
여기서 destroy되는 것은 이 Lab 폴더(`lb/`)의 VPC·EC2·ALB뿐입니다. Lab8에서 만든 S3 버킷은 별도 폴더의 리소스이므로 영향을 받지 않습니다.
{% endhint %}

2. GenAI 에게 코드 내용을 분리하여 파일 생성

{% hint style="info" %}
variables.tf : 변수 선언

storage.tf : S3 data 소스 조회 + provider 설정

compute.tf : VPC 모듈 호출 + EC2 3대

lb.tf : ALB / Target Group / Listener

outputs.tf : 출력 값
{% endhint %}

이때, outputs 에는 `alb_dns_name`, `target_group_arn`, `vpc_id`, `image_url` 을 출력합니다.

3. GenAI 에게 vars 파일 활용하도록 lb.tfvars(파일명은 예시) 파일 생성
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
apply 시 변수 적용 하여 실행
{% endhint %}

6. CLI/콘솔에서 확인
7. 리소스 정리

```
terraform destroy -auto-approve
```

{% hint style="info" %}
다음 Lab(Lab10 AutoScaleGroup)에서는 EC2를 고정 3대가 아니라 트래픽에 따라 자동으로 늘고 줄게 만듭니다. Lab8의 S3 버킷은 계속 켜둔 채로 넘어가세요.
{% endhint %}


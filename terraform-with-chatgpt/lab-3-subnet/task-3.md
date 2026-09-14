# Task 3 - 리소스 삭제 및 심화

1. 아래 명령으로 기존 생성한 리소스 삭제

```
terraform destroy -auto-approve
```



2. &#x20;GenAI 에게코드 내용을 분리하여 파일 생성

{% hint style="info" %}
main.tf : 리소스 블럭

provider.tf : 프로바이더 설정 블럭

variables.tf : 변수선언

outputs.tf : 출력 값
{% endhint %}

이때, outputs 에는 `public_subnet_ids`, `private_subnet_ids` 를 출력합니다.



3. GenAI 에게 vars 파일 활용하도록 subnet.tfvars (파일명은 예시) 파일 생성



4. 해당 코드를 활용하는 간단한 README.md 파일 생성



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





6. 콘솔에서 확인







7. 리소스 정리&#x20;

```
terraform destroy -auto-approve
```





{% hint style="info" icon="puzzle" %}
도전!&#x20;

`for_each`나 `count`와 `cidrsubnet()` 함수를 활용해 서브넷 4개를 반복문으로 생성
{% endhint %}


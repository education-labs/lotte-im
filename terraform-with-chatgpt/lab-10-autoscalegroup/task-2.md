# Task 2 - 리소스 생성 및 확인

1. 해당 디렉토리로 이동

```
cd ~/environment/lb
```



2. 아래 순서대로 명령 실행

```
terraform init
terraform fmt
terraform validate
terraform plan
```

{% hint style="info" %}
terraform init : 프로바이더플러그인 다운로드\
terraform fmt : 코드스타일정리\
terraform validate : 문법오류확인\
terraform plan : 실제 AWS 상태와 비교하여 생성/변경/삭제 되는 내용 미리보기
{% endhint %}





3. 아래 명령으로 AWS 리소스 생성

```
terraform apply -auto-approve
```





4. AWS Console 에서 해당 리소스가 생성되었는지 확인


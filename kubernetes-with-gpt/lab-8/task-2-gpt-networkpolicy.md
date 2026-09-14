# Task 2 - GPT 활용 NetworkPolicy



1. ChatGPT 사이트에 접속

{% embed url="https://chatgpt.com/" %}

2. 아래 내용을 만족하도록 GPT에게 질의하고 명령을 수행

1\) 아래 조건을 만족하는 NetworkPolicy 및 테스트용 Pod YAML 파일 생성

```
Namespace A            : backend-ns
Namespace B            : client-ns
backend Pod            : backend (role=backend) → http-echo 서버
client Pod             : client (role=client) → curl 사용
접근 허용 조건         : backend Pod은 label이 team=dev인 네임스페이스만 접근 허용
```

2\) 통신 테스트 명령어 생성

3\) 적용 상태 확인 방법 질의

4\) NetworkPolicy 삭제 명령 질의

5\) 생성한 오브젝트(Pod,NS) 삭제 명령 질의&#x20;

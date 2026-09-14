# Task 3 - GPT 활용 Service

1. ChatGPT 사이트에 접속

{% embed url="https://chatgpt.com/" %}

2. 아래 내용을 만족하도록 GPT에게 질의하고 명령을 수행

1\) 아래있는 조건을 만족하는 Deployment 의 yaml 파일 생성

```
Deployment Name : gpt-deploy
Pod Image       : nginx  
Container Port  : 80  
Pod Numbers      : 2  
Label           : app=gpt  
```

2\) 아래있는 조건을 만족하는 Service 의 yaml 파일 생성

```
Service Name  : gpt-svc  
Type          : ClusterIP  
Selector      : app=gpt  
Target Port   : 80  
Service Port  : 8080
```

\
3\) 해당 yaml 을 기반으로 Deployment 와 Service 를 생성하는 명령생성

4\) 각각 상태와 상세 정보를 확인하는 명령생성

5\) Service 접근 테스트 명령 생성

6\) 리소스 삭제 명령 생성

+@) 외부에서 접근 가능한 Service로 변경


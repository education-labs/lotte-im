# Task 3 - GPT 활용 Volume



1. ChatGPT 사이트에 접속

{% embed url="https://chatgpt.com/" %}

2. 아래 내용을 만족하도록 GPT에게 질의하고 명령을 수행

1\) 아래있는 조건을 만족하는 Deployment 의 yaml 파일 생성

```
Deployment Name : volume-test
Pod Image       : nginx
Container Port  : 80
Pod Numbers     : 2
Volume Type     : hostPath
Mount Path      : /usr/share/nginx/html 
Host Path       : /data/html # 단, /data/html 경로는 노드에 미리 존재해야 함
Update Strategy : RollingUpdate # (무중단 배포 방식)
```

\
3\) 해당 yaml 을 기반으로 Deployment 를 생성하는 명령생성

4\) 각각 상태와 상세 정보를 확인하는 명령생성

5\) Pod에 접속하여 Volume 에 아무 텍스트 파일을 저장하는 명령생성

6\) 리소스 삭제 명령 생성

+@) Volume type을 Emptydir로 구성하여 생성

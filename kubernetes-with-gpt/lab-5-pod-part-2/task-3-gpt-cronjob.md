# Task 3 - GPT 활용 Cronjob

1. ChatGPT 사이트에 접속

{% embed url="https://chatgpt.com/" %}

2. 아래 내용을 만족하도록 GPT에게 질의하고 명령을 수행

1\) 아래있는 조건을 만족하는 Cronjob 의 yaml 파일 생성

```
CronJob Name : gpt-cron
Pod Image    : busybox
Command      : 현재 시간과 "Hello GPT CronJob" 출력
Schedule     : 매 1분마다 실행
RestartPolicy: OnFailure
```

2\) 해당 yaml 을 기반으로 Cronjob 를 생성하는 명령생성

3\) 상세 정보를 확인하는 명령 생성

4\) Cronjob 를 삭제 하는 명령 생성


# Task 3 - GPT 활용 RQ & LR



1. ChatGPT 사이트에 접속

{% embed url="https://chatgpt.com/" %}

2. 아래 내용을 만족하도록 GPT에게 질의하고 명령을 수행

1\) 아래있는 조건을 만족하는 ResourceQuota, LimitRange 의 yaml 파일 생성

```
네임스페이스: gpt-ns

ResourceQuota
최대 Pod 수: 5
전체 CPU 요청: 1 vCPU
전체 Memory 요청: 2Gi

LimitRange
기본 요청: CPU 100m / Memory 256Mi
기본 제한: CPU 500m / Memory 512Mi
최대 제한: CPU 1 / Memory 1Gi
```

2\) 해당 yaml 을 기반으로 ResourceQuota, LimitRange 를 생성하는 명령생성

3\) 상세 정보를 확인하는 명령 생성

4\) 제한을 초과하는 Pod를 만들어보고 실패 확인

5\) 제한을 초과하지 않는 Pod를 만드는 yaml 과 명령 생성

4\) ResourceQuota, LimitRange 를 삭제 하는 명령 생성

# Task 4 - GPT 활용 Taint



1. ChatGPT 사이트에 접속

{% embed url="https://chatgpt.com/" %}

2. 아래 내용을 만족하도록 GPT에게 질의하고 명령을 수행

1\) 아래있는 조건을 만족하는 taint 추가명령과 Pod yaml 파일 생성

```
노드 이름       : k8s-dp1, k8s-dp2
Taint Key       : key
Taint Value     : noschedule
Effect          : NoSchedule

Pod A 이름      : pod-no-toleration
Pod A 내용      : toleration 없이 생성 → 스케줄 실패 예상

Pod B 이름      : pod-with-toleration
Pod B 내용      : Taint를 toleration으로 허용 → 스케줄 성공 예상
```

2\) taint 설정 확인 방안 질의

3\) Pod 스케줄 결과 확인 방법 질의

4\) Taint 제거 방법 질의

5\) 생성한 오브젝트(Pod) 삭제 명령 생성

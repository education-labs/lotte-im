# Task 2 - GPT 활용 Pod

1. ChatGPT 사이트에 접속

{% embed url="https://chatgpt.com/" %}

2. 질문을 통해 Pod 를 배포하는 yaml 얻기 (답변이 다를 수 있습니다.)

```
쿠버네티스에서 Pod를 만드는 Yaml 파일을 만들어줘
```

<figure><img src="../../.gitbook/assets/image (12) (1) (1).png" alt=""><figcaption></figcaption></figure>

3. 추가 질문을 통해 해당 yaml 코드를 파일로 만드는 명령 생성

```
만들어준 yaml 코드를 파일로 만드는 명령을 만들어줘
```

<figure><img src="../../.gitbook/assets/image (13) (1).png" alt=""><figcaption></figcaption></figure>

4. 알려준 명령을 사용하여 CP 노드에서 파일생성

<figure><img src="../../.gitbook/assets/image (14) (1).png" alt=""><figcaption></figcaption></figure>

5. 추가 질문을 통해 yaml 파일 리소스를 생성, 상태 확인, 상세 정보 확인, 리소스 삭제 명령생성

```
만들어준 yaml 파일을 사용하여 리소스를 만드는 명령, 
만든 리소스의 상태 확인하는 명령,
상세 정보를 확인하는 명령, 
만든 리소스를 삭제하는 명령을 만들어줘
```

<figure><img src="../../.gitbook/assets/image (15) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (17) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (18) (1).png" alt=""><figcaption></figcaption></figure>

6. 답변에 따라 리소스 생성

```
kubectl apply -f pod.yaml
```

<figure><img src="../../.gitbook/assets/image (19) (1).png" alt=""><figcaption></figcaption></figure>

7. 답변에 따라 리소스 확인

```
kubectl get pods
kubectl get pod example-pod
kubectl describe pod example-pod
```

8. 답변에 따라 리소스 삭제

```
kubectl delete pod example-pod
```

<figure><img src="../../.gitbook/assets/image (20) (1).png" alt=""><figcaption></figcaption></figure>

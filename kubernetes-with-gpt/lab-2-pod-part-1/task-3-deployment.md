# Task 3 - Deployment



1. Deployment 생성

```
kubectl create deploy no-yaml-dp --image=nginx --replicas=3
```

2. Deployment 확인

```
kubectl get deploy
```

3. 상세 정보 확인

```
kubectl describe deploy no-yaml-dp
```

4. Deployment 에 의해 생성된 Replicaset 확인

```
kubectl get rs
```

5. 상세 정보 확인

```
kubectl describe rs
```

6. Pod 확인

```
kubectl get pod
```

7. Pod 중 아무 Pod 하나 삭제

```
kubectl delete pod <Any Pod Name>
```

8. 다시 Pod를 확인하여 자동 복구됨을 확인

```
kubectl get pod
```

9. 모든 Pod 삭제

```
kubectl delete pod --all
```

10. 자동 복구 확인

```
kubectl get pod
```

11. Deployment 삭제

```
kubectl delete deploy no-yaml-dp
```

12. yaml 생성

```
cat <<EOF > lab2-deploy.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deploy
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.0
        ports:
        - containerPort: 80
EOF
```

13. yaml 기반 deploy 생성

```
kubectl create -f lab2-deploy.yaml
```

14. 생성 확인

```
kubectl get deploy,pod
```

15. c9에서 터미널을 하나 더 오픈하고, MasterNode에 접속한 뒤 아래 명령을 수행하여 모니터링용 터미널 구성

```
sudo -i
watch -n 0.1 kubectl get pod
```

16. 해당 터미널을 아래와 같이 구성하는 것을 권장

<figure><img src="../../.gitbook/assets/image (29) (1).png" alt=""><figcaption></figcaption></figure>

17. 컨테이너 이미지 1.15.0으로 버전 업데이트

```
kubectl set image deploy my-deploy nginx=nginx:1.15.0 --record=true
```

`이때 명령어 수행 직후 모니터링 터미널로 동작 확인`

`record=true 값으로 해야 히스토리 확인시 어떤 내용인지 확인 가능`

18. 업데이트 내역 확인

```
kubectl describe pod
kubectl describe deploy
```

19. 업데이트 방식 변경

```
kubectl edit deploy my-deploy
```

```
strategy:
  rollingUpdate:               # 줄삭제 
    maxSurge: 25%              # 줄삭제 
    maxUnavailable: 25%        # 줄 삭제
  type: RollingUpdate         # 내용 변경
```

```
strategy:
  type: Recreate # 이처럼 변경
```

`vi 편집기 사용법과 동일합니다.`

20. 컨테이너 이미지 1.16.0 으로 버전 업데이트

```
kubectl set image deploy my-deploy nginx=nginx:1.16.0 --record=true
```

`이때 명령어 수행 직후 모니터링 터미널로 동작 확인`

21. 업데이트 내역 확인

```
kubectl describe pod
kubectl describe deploy
```

22. 배포 기록 확인

```
kubectl rollout history deploy my-deploy
```

23. 직전 버전으로 롤백

```
kubectl rollout undo deploy my-deploy
```

24. 롤백된 버전 확인

```
kubectl describe deploy
```

25. 리비전넘버 지정하여 롤백

```
kubectl rollout undo deploy my-deploy --to-revision=1
```

26. 버전 확인

```
kubectl describe deploy
```

27. 스케일 아웃

```
kubectl scale deploy my-deploy --replicas=10
```

28. Pod 증가 된 것을 확인

```
kubectl get pod
```

29. 스케일 인

```
kubectl scale deploy my-deploy --replicas=1
```

30. Pod 감소 된 것을 확인

```
kubectl get pod
```

31. 실습 리소스 삭제

```
kubectl delete deploy my-deploy
```

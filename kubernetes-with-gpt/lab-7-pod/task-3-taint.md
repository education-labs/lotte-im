# Task 3 - Taint



1. taint 생성

```
kubectl taint nodes k8s-worker1 key=noschedule:NoSchedule
```

2. Deployment yaml 생성

```
cat <<EOF > lab7-taint-deploy.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deploy-noschedule
spec:
  replicas: 6
  selector:
    matchLabels:
      app: noschedule-demo
  template:
    metadata:
      labels:
        app: noschedule-demo
    spec:
      containers:
      - name: nginx
        image: nginx
EOF
```

```
kubectl create -f lab7-taint-deploy.yaml
```

3. Pod 배치 확인

```
kubectl get pod -o wide
```

4. Deployment 삭제

```
kubectl delete deploy deploy-noschedule
```

5. Taint 삭제

```
kubectl taint nodes k8s-worker1 key=noschedule:NoSchedule-
```

6. Deployment 다시 생성

```
kubectl create -f lab7-taint-deploy.yaml
```

7. Pod 배치확인

```
kubectl get pod -o wide
```

8. Taint 설정

```
kubectl taint nodes k8s-worker1 key=expel:NoExecute
```

9. Pod 방출 확인

```
kubectl get pod -o wide
```

10. Taint 삭제 및 리소스 삭제

```
kubectl taint nodes k8s-worker1 key=expel:NoExecute-
kubectl delete deployment --all
```

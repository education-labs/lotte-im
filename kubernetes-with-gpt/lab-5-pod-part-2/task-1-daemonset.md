# Task 1 - Daemonset



1. yaml 생성 (Daemonset)

```
cat <<EOF > lab5-daemonset-1.yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: my-deamonset-1
spec:
  selector:
    matchLabels:
      type: app
  template:
    metadata:
      labels:
        type: app
    spec:
      containers:
      - name: container
        image: nginx
        ports:
        - containerPort: 8080
EOF
```

2. yaml 기반 리소스 생성

```
kubectl create -f lab5-daemonset-1.yaml
```

3. 생성 확인

```
kubectl get ds
```

4. 상세정보 확인

```
kubectl describe ds
```

5. ds가 배포한 pod 확인

```
kubectl get pod -o wide
```

6. nodeselector 기능을 확인하기위해 각 워커노드에 labeling

```
kubectl label nodes k8s-dp1 os=centos
kubectl label nodes k8s-dp2 os=ubuntu
```

7. yaml 생성 (Daemonset)

```
cat <<EOF > lab5-daemonset-2.yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: my-deamonset-2
spec:
  selector:
    matchLabels:
      type: app
  template:
    metadata:
      labels:
        type: app
    spec:
      nodeSelector:
        os: centos
      containers:
      - name: container
        image: nginx
        ports:
        - containerPort: 8080
EOF
```

8. yaml 기반 리소스 생성

```
kubectl create -f lab5-daemonset-2.yaml
```

9. 생성 확인

```
kubectl get ds
```

10. 상세정보 확인

```
kubectl describe ds
```

11. ds가 배포한 pod 확인

```
kubectl get pod -o wide
```

12. 워커노드에 설정했던 Label 삭제

```
kubectl label nodes k8s-worker1 os-
kubectl label nodes k8s-worker2 os-
```

13. Daemonset과 pod 확인

```
kubectl get ds,pod
```

14. 리소스 삭제

```
kubectl delete ds --all
```

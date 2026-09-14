# Task 1 - Pod

1. Pod 생성

```
kubectl run no-yaml-pod-1 --image=nginx --port=80
kubectl run no-yaml-pod-2 --image=nginx --port=80
```

2. Pod 확인

```
kubectl get pod
```

3. 상세정보 확인

```
kubectl describe pod
```

4. Pod 지정하여 상세정보 확인

```
kubectl describe pod <POD-NAME>
```

5. yaml 생성

```
cat <<EOF > lab2-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    app: my-app
spec:
  containers:
    - name: my-container
      image: nginx:1.25.3
      ports:
        - containerPort: 80
EOF
```

6. yaml 기반 Pod 생성

```
kubectl apply -f lab2-pod.yaml
```

7. Pod 확인

```
kubectl get pod
```

8. Pod 상세 정보 확인

```
kubectl describe pod
```

9. Pod 삭제

```
kubectl delete pod my-pod
```


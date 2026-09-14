# Task 1 - NodeName

1. nodename 설정을 이용한 rs yaml 생성

```
cat <<EOF > lab7-nodename.yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nodename
  template:
    metadata:
      labels:
        app: nodename
    spec:
      nodeName: k8s-worker1
      containers:
        - name: nginx
          image: nginx
EOF
```

2. yaml 기반 리소스생성

```
kubectl create -f lab7-nodename.yaml
```

3. 위 결과로 생성된 Pod가 어떤 노드에 할당되었는지 확인

```
kubectl get pod -o wide
```

4. 생성된 Replicaset 를 Scale Out

```
kubectl scale rs my-rs --replicas=5
```

5. 새롭게 생성된 Pod의 할당 확인

```
kubectl get pod -o wide
```

6. 리소스 삭제

```
kubectl delete rs my-rs
```


# Task 2 - Affinity

1. node에 Label 셋팅

```
kubectl label nodes k8s-worker1 color=blue
kubectl label nodes k8s-worker2 color=red
```

2. label 확인

```
kubectl get nodes --show-labels
```

3. hard Affinity 설정을 사용하는 Deployment yaml 생성 및 yaml 기반 리소스 생성

```
cat <<EOF > lab7-affinity-deploy-1.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-affinity-hard
spec:
  replicas: 3
  selector:
    matchLabels:
      app: hard
  template:
    metadata:
      name: hard
      labels:
        app: hard
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - name: http
          containerPort: 80
          protocol: TCP
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: color
                operator: In
                values:
                - blue
                - green
EOF
```

```
kubectl create -f lab7-affinity-deploy-1.yaml
```

4. Pod의 배치 확인

```
kubectl get pod -o wide
```

5. soft Affinity 설정을 사용하는 Deployment yaml 생성 및 yaml 기반 리소스 생성

```
cat <<EOF > lab7-affinity-deploy-2.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-affinity-soft
spec:
  replicas: 3
  selector:
    matchLabels:
      app: soft
  template:
    metadata:
      name: soft
      labels:
        app: soft
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - name: http
          containerPort: 80
          protocol: TCP
      affinity:
        nodeAffinity:
          preferredDuringSchedulingIgnoredDuringExecution:
          - weight: 70
            preference:
              matchExpressions:
              - key: color
                operator: In
                values:
                - red
          - weight: 30  
            preference:
              matchExpressions:
              - key: color
                operator: In
                values:
                - blue
EOF
```

```
kubectl create -f lab7-affinity-deploy-2.yaml
```

6. Pod의 배치 확인

```
kubectl get pod -o wide
```


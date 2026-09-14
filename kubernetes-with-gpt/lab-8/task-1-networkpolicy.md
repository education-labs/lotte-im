# Task 1 - NetworkPolicy

1. Namespace 생성

```
kubectl create ns net-test
```

2. Frontend Pod yaml 생성

```
cat <<EOF > lab8-front-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: frontend
  namespace: net-test
  labels:
    role: frontend
spec:
  containers:
  - name: curl
    image: curlimages/curl
    command: ["sleep", "3600"]
EOF
```

3. Backend Pod yaml 생성

```
cat <<EOF > lab8-back-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: backend
  namespace: net-test
  labels:
    role: backend
spec:
  containers:
  - name: http
    image: hashicorp/http-echo
    args: ["-text=Hello from backend"]
EOF
```

4. 각 Pod 생성

```
kubectl apply -f lab8-front-pod.yaml
kubectl apply -f lab8-back-pod.yaml
```

5. networkpolicy yaml 생성

```
cat <<EOF > lab8-networkpolicy.yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-frontend-to-backend
  namespace: net-test
spec:
  podSelector:
    matchLabels:
      role: backend
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          role: frontend
EOF
```

6. Yaml 기반 리소스 생성

```
kubectl create -f lab8-networkpolicy.yaml
```

7. Pod ip 확인

```
kubectl get pod -o wide
```

7. 접근 테스트 (Frontend 에서 backend)

```
kubectl exec -n net-test frontend -- curl <backendPodIP>:5678
```

8. 다른 Pod yaml 생성

```
cat <<EOF > lab8-other-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: other
  namespace: net-test
  labels:
    role: other
spec:
  containers:
  - name: curl
    image: curlimages/curl
    command: ["sleep", "3600"]
EOF
```

9. yaml 기반 리소스 생성

```
kubectl create -f lab8-other-pod.yaml
```

10. other pod 에서 backend 로 접근시도

```
kubectl exec -n net-test other -- curl <backendPodIP>:5678
```

접근이 막히는 것을 확인



11. 리소스삭제

```
kubectl delete networkpolicy,pod --all -n net-test
kubectl delete ns net-test
```


# Task 2 - Ingress

1. nginx ingress controller 설치

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.0.0/deploy/static/provider/baremetal/deploy.yaml
```

2. yaml 생성 (Homepage app - svc,pod)

```
cat <<EOF > lab3-homepage.yaml
apiVersion: v1
kind: Pod
metadata:
  name: hp-pod
  labels:
    category: homepage
spec:
  containers:
  - name: container
    image: ghcr.io/wsjang619/home
---
apiVersion: v1
kind: Service
metadata:
  name: hp-svc
spec:
  selector:
    category: homepage
  ports:
  - port: 8080
    targetPort: 80
EOF
```

3. yaml 기반 리소스 생성 (Homepage app - svc,pod)

```
kubectl create -f lab3-homepage.yaml
```

4. yaml 생성 (Customer app - svc,pod)

```
cat <<EOF > lab3-customer.yaml
apiVersion: v1
kind: Pod
metadata:
  name: ct-pod
  labels:
    category: customer
spec:
  containers:
  - name: container
    image: ghcr.io/wsjang619/customer
---
apiVersion: v1
kind: Service
metadata:
  name: ct-svc
spec:
  selector:
    category: customer
  ports:
  - port: 8080
    targetPort: 80
EOF
```

5. yaml 기반 리소스 생성 (Customer app - svc,pod)

```
kubectl create -f lab3-customer.yaml
```

6. yaml 생성 (Schedule app - svc,pod)

```
cat <<EOF > lab3-schedule.yaml
apiVersion: v1
kind: Pod
metadata:
  name: sc-pod
  labels:
    category: schedule
spec:
  containers:
  - name: container
    image: ghcr.io/wsjang619/schedule
---
apiVersion: v1
kind: Service
metadata:
  name: sc-svc
spec:
  selector:
    category: schedule
  ports:
  - port: 8080
    targetPort: 80
EOF
```

7. yaml 기반 리소스 생성 (Schedule app - svc,pod)

```
kubectl create -f lab3-schedule.yaml
```

8. 리소스 확인

```
kubectl get pod,svc
```

9. yaml 생성 (ingress)

```
cat <<EOF > lab3-ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: lb-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: hp-svc
            port:
              number: 8080
      - path: /customer
        pathType: Prefix
        backend:
          service:
            name: ct-svc
            port:
              number: 8080
      - path: /schedule
        pathType: Prefix
        backend:
          service:
            name: sc-svc
            port:
              number: 8080
EOF
```

10. yaml 기반 리소스 생성

```
kubectl create -f lab3-ingress.yaml
```

11. Ingress svc 확인

```
kubectl get svc -n ingress-nginx
```

12. Worker Node IP 확인

```
kubectl get node -o wide
```

13. 11, 12서 확인한 내용을 활용하여 접근 시도

```
curl <WorkerIP>:<NodePortNumber>
curl <WorkerIP>:<NodePortNumber>/customer
curl <WorkerIP>:<NodePortNumber>/schedule
```

14. 리소스 삭제

```
kubectl delete pod,svc,ingress --all
```


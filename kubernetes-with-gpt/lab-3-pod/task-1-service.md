# Task 1 - Service



1. yaml 생성 (ClusterIP SVC)

```
cat <<EOF > lab3-svc1.yaml
apiVersion: v1
kind: Service
metadata:
  name: svc-clusterip
spec:
  selector:
    app: svc
  ports:
  - port: 9090
    targetPort: 80
  type: ClusterIP
EOF
```

2. yaml 기반 리소스 생성

```
kubectl create -f lab3-svc1.yaml
```

3. yaml 생성 (Pod)

```
cat <<EOF > lab3-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: svc-pod
  labels:
    app: svc
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
EOF
```

4. yaml 기반 리소스 생성

```
kubectl create -f lab3-pod.yaml
```

5. 리소스 확인

```
kubectl get pod
kubectl get svc
```

6. curl 명령을 통한 통신 확인

```
curl <svc의 ClusterIP>:9090
```

`5에서 확인한 IP를 입력합니다.`

7. yaml 생성 (NodePort SVC)

```
cat <<EOF > lab3-svc2.yaml
apiVersion: v1
kind: Service
metadata:
  name: svc-nodeport
spec:
  selector:
    app: svc
  ports:
  - port: 9090
    targetPort: 80
    nodePort: 30000
  type: NodePort
EOF
```

8. yaml 기반 리소스 생성

```
kubectl create -f lab3-svc2.yaml
```

9. 리소스 확인

```
kubectl get svc
```

10. curl 명령을 통한 통신 확인

```
curl <AnyWorkerNodeIP>:30000
```

11. yaml 생성 (LoadBalancer SVC)

```
cat <<EOF > lab3-svc3.yaml
apiVersion: v1
kind: Service
metadata:
  name: svc-loadbalancer
spec:
  selector:
    app: svc
  ports:
  - port: 9090
    targetPort: 80
  type: LoadBalancer
EOF
```

12. yaml 기반 리소스 생성

```
kubectl create -f lab3-svc3.yaml
```

13. 리소스 확인

```
kubectl get svc
```

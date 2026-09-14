# Task 2 - 환경변수

1. yaml 생성 (Configmap)

```
cat <<EOF > lab4-configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-configmap
data:
  APP_ENV: production
  LOG_LEVEL: debug
EOF
```

2. yaml 기반 리소스 생성

```
kubectl create -f lab4-configmap.yaml
```

3. yaml 생성 (Secret)

```
cat <<EOF > lab4-secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  DB_USER: dXNlcm5hbWU=        # "username" (Base64 인코딩)
  DB_PASSWORD: cGFzc3dvcmQ=    # "password" (Base64 인코딩)
EOF
```

4. yaml 기반 리소스 생성

```
kubectl create -f lab4-secret.yaml
```

5. 생성 확인

```
kubectl get configmap,secret
```

6. 상세정보 확인

```
kubectl describe configmap
kubectl describe secret
```

7. configmap과 secret의 변수를 사용하는 pod yaml 생성

```
cat <<EOF > lab4-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-var-pod
spec:
  containers:
  - name: app
    image: busybox
    command: ["sh", "-c", "env && sleep 3600"]
    env:
    - name: ENV_MODE
      valueFrom:
        configMapKeyRef:
          name: my-configmap
          key: APP_ENV
    - name: LOG_LEVEL
      valueFrom:
        configMapKeyRef:
          name: my-configmap
          key: LOG_LEVEL
    - name: DB_USER
      valueFrom:
        secretKeyRef:
          name: my-secret
          key: DB_USER
    - name: DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: my-secret
          key: DB_PASSWORD
EOF
```

8. yaml 기반 리소스 생성

```
kubectl create -f lab4-pod.yaml
```

9. 생성 확인

```
kubectl get pod
```

10. 로그를 확인하여 변수 확인

```
kubectl logs my-var-pod
```

11. 리소스 삭제

```
kubectl delete pod,configmap,secret --all
```


# Task 2 - LimitRange



1. yaml 생성 (RQ + NS)

```
cat <<EOF > lab6-lr-ns.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: lr-ns
---
apiVersion: v1
kind: LimitRange
metadata:
  name: my-lr
  namespace: lr-ns
spec:
  limits:
  - type: Container
    min:
      memory: 0.1G
    max:
      memory: 0.4G
    maxLimitRequestRatio:
      memory: 3
    defaultRequest:
      memory: 0.1G
    default:
      memory: 0.2G
EOF
```

2. yaml 기반 리소스생성

```
kubectl create -f lab6-lr-ns.yaml
```

3. 생성확인

```
kubectl get ns
kubectl get limitrange -n lr-ns
```

4. 상세정보확인

```
kubectl describe ns
kubectl describe limitrange -n lr-ns
```

5. 테스트 Pod yaml 생성 및 yaml 기반 생성

```
cat <<EOF > lab6-lr-pod1.yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-lr-pod1
  namespace: lr-ns
spec:
  containers:
  - name: container
    image: ubuntu
    resources:
      requests:
        memory: 0.1G
      limits:
        memory: 0.5G
EOF
```

```
kubectl create -f lab6-lr-pod1.yaml
```

`lr 제한에 위배되어 생성되지 않음을 확인`

6. 두번째 Pod yaml 생성 및 리소스 생성

```
cat <<EOF > lab6-lr-pod2.yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-lr-pod2
  namespace: lr-ns
spec:
  containers:
  - name: container
    image: ubuntu
    resources:
      requests:
        memory: 0.1G
      limits:
        memory: 0.2G
EOF
```

```
kubectl create -f lab6-lr-pod2.yaml
```

7. Pod, LimitRange 확인

```
kubectl get pod,limitrange -n lr-ns
kubectl describe limitrange -n lr-ns
```

8. 리소스 삭제 (NS를 삭제하면 내부 오브젝트도 함께 일괄 삭제)

```
kubectl delete ns lr-ns
```

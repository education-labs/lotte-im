# Task 1 - ResourceQuota

1. yaml 생성 (RQ + NS)

```
cat <<EOF > lab6-rq-ns.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: rq-ns
---
apiVersion: v1
kind: ResourceQuota
metadata:
  name: my-rq1
  namespace: rq-ns
spec:
  hard:
    requests.memory: 1Gi
    limits.memory: 1Gi
EOF
```

2. yaml 기반 리소스생성

```
kubectl create -f lab6-rq-ns.yaml
```

3. 생성확인

```
kubectl get ns
kubectl get quota -n rq-ns
```

4. 상세정보확인

```
kubectl describe ns
kubectl describe quota -n rq-ns
```

5. 테스트 Pod yaml 생성 및 yaml 기반 생성

```
cat <<EOF > lab6-rq-pod1.yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-rq1-pod1
  namespace: rq-ns
spec:
  containers:
  - name: container
    image: ubuntu
EOF
```

```
kubectl create -f lab6-rq-pod1.yaml
```

`rq 설정에 있는 조건설정이 아예 없어, 생성 실패`

6. 두번째 테스트 Pod yaml 생성 및 yaml 기반 생성

```
cat <<EOF > lab6-rq-pod2.yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-rq1-pod2
  namespace: rq-ns
spec:
  containers:
  - name: container
    image: nginx
    resources:
      requests:
        memory: 0.5Gi
      limits:
        memory: 0.5Gi
EOF
```

```
kubectl create -f lab6-rq-pod2.yaml
```

`정상 생성을 확인`

7. ResourceQuota 현재 상태 확인

```
kubectl describe quota -n rq-ns
```

8. ResourceQuota 삭제

```
kubectl delete quota my-rq1 -n rq-ns
```

9. 두번째 ResourceQuota yaml 생성 및 yaml 리소스 생성

```
cat <<EOF > lab6-rq2.yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: my-rq2
  namespace: rq-ns
spec:
  hard:
    pods: 2
EOF
```

```
kubectl create -f lab6-rq2.yaml
```

10. ResourceQuota 확인

```
kubectl describe quota -n rq-ns
```

11. pod 추가 생성 시도

```
kubectl create deploy rq-dp --image=nginx --replicas=3 -n rq-ns
```

12. pod 확인

```
kubectl get pod -n rq-ns
```

13. 세부 정보 확인

```
kubectl describe deploy rq-dp -n rq-ns
kubectl describe rs rq-dp -n rq-ns
```

14. 리소스 삭제 (NS를 지움으로써 NS 내부 오브젝트도 함께 삭제)

```
kubectl delete ns rq-ns
```


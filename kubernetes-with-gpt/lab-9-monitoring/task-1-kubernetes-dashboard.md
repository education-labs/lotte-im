# Task 1 - Kubernetes Dashboard



### Kubernetes Dashboard 설치

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
```

### 2. Service 타입을 NodePort로 변경

```bash
kubectl -n kubernetes-dashboard edit svc kubernetes-dashboard
```

```yaml
spec:
  type: NodePort 
```

### 3. ServiceAccount 및 ClusterRoleBinding 생성

```yaml
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user-binding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
EOF
```

4. 로그인용 토큰 확인

```bash
kubectl -n kubernetes-dashboard create token admin-user
```

5. 접속

```plaintext
https://<NodePublicIP>:<NodePort>
```

노드 PublicIP 확인 명령

```
curl ifconfig.io
```

Nodeport Number 확인 명령

```
kubectl get svc kubernetes-dashboard -n kubernetes-dashboard 
```



6. 위 4번에서 확인한 토큰으로 로그인

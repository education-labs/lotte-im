# Task 1 - Volume



1. yaml 생성 (EmptyDir Volume)

```
cat <<EOF > lab4-emptydir-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-emptydir-pod
spec:
  containers:
  - name: redis
    image: redis
    volumeMounts:
    - name: emptydir
      mountPath: /mount1
  - name: nginx
    image: nginx
    volumeMounts:
    - name: emptydir
      mountPath: /mount2
  volumes:
  - name : emptydir
    emptyDir: {}
EOF
```

2. yaml 기반 리소스 생성

```
kubectl create -f lab4-emptydir-pod.yaml
```

3. 위 2에서 만든 Pod 내부의 컨테이너 redis로 접속합니다.

```
kubectl exec -it my-emptydir-pod --container redis -- /bin/bash
```

4. 마운트 된 디렉토리로 이동 후 파일생성

```
cd /mount1
echo hello emptydir >> test.txt
cat test.txt
exit
```

5. 위에서 만든 Pod 내부의 컨테이너 nginx로 접속합니다.

```
kubectl exec -it my-emptydir-pod --container nginx -- /bin/bash
```

6. 디렉토리 이동 후 3에서 생성한 파일 확인

```
cd /mount2
ls
cat test.txt
exit
```

7. yaml 생성 (HostPath Volume)

```
cat <<EOF > lab4-hostpath-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-hostpath-pod
spec:
  containers:
  - name: redis
    image: redis
    volumeMounts:
    - name: hostpath
      mountPath: /mount1
  volumes:
  - name : hostpath
    hostPath:
      path: /tmp
      type: Directory
EOF
```

8. yaml 기반 리소스생성

```
kubectl create -f lab4-hostpath-pod.yaml
```

9. 위에서 만든 Pod 내부의 컨테이너 nginx로 접속합니다.

```
kubectl exec -it my-hostpath-pod --container redis -- /bin/bash
```

10. 마운트 된 디렉토리로 이동 후 파일생성

```
cd /mount1
echo hello hostpath >> test.txt
cat test.txt
exit
```

11. 위에서 생성한 Pod 가 어떤 노드에 생성되었는지 확인

```
kubectl get pod -o wide
```

12. 위에서 확인한 노드의 터미널로 이동하여 파일 생성 확인

```
cd /tmp
ls
cat test.txt
```

13. 리소스 삭제

```
kubectl delete pod --all
```

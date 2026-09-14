# Task 2 - Job & Cronjob

1. yaml 생성 (Job)

```
cat <<EOF > lab5-job-1.yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: my-job-1
spec:
  template:
    spec:
      containers:
      - name: pi
        image: perl:5.34.0
        command: ["perl",  "-Mbignum=bpi", "-wle", "print bpi(2000)"]
      restartPolicy: Never
  backoffLimit: 4
EOF
```

2. yaml 기반 리소스 생성

```
kubectl create -f lab5-job-1.yaml
```

3. 생성 확인

```
kubectl get job
kubectl get pod
kubectl describe job my-job-1
```

4. 1분 정도 대기 후 재확인

```
kubectl get job
kubectl get pod
kubectl describe job my-job-1
```

5. 실행한 결과 확인

```
kubectl logs <PodName>
```

6. yaml 생성 (Job)

```
cat <<EOF > lab5-job-2.yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: my-job-2
spec:
  template:
    spec:
      containers:
      - name: pi
        image: perl:5.34.0
        command: ["perl",  "-Mbignum=bpi", "-wle", "print bpi(2000)"]
      restartPolicy: Never
  backoffLimit: 4
EOF
```

7. yaml 기반 리소스 생성

```
kubectl create -f lab5-job-2.yaml
```

8. 생성 확인

```
kubectl get job
kubectl get pod
kubectl describe job my-job-2
```

9. 30초 정도 뒤에 재확인

```
kubectl get job
kubectl get pod
kubectl describe job my-job-2
```

10. job 리소스 삭제

```
kubectl delete job --all
```

11. yaml 생성 (Cronjob)

```
cat <<EOF > lab5-cronjob.yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: my-cronjob
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          restartPolicy: Never
          containers:
          - name: container
            image: ubuntu
EOF
```

12. yaml 기반 리소스 생성

```
kubectl create -f lab5-cronjob.yaml
```

13. 생성확인

```
kubectl get cronjob,job,pod
```

\
14\. 다시 1분정도 뒤에 재확인하여 동작 확인

```
kubectl get cronjob,job,pod
```

15. edit 명령을 사용하여 cronjob 중지

```
kubectl edit cronjob my-cronjob
```

```
# suspend 값을 false에서 true로 변경합니다.

:%s/false/true/g
:wq
```

16. cronjob 중지된것을 확인

```
kubectl get cronjob
```

17. patch 명령을 사용하여 위에서 변경한 true 값을 false로 복구

```
kubectl patch cronjob my-cronjob -p '{"spec" : {"suspend" : false }}'
```

18. cronjob을 확인하여 재시작됨을 확인

```
kubectl get cronjob
```

19. 리소스 삭제

```
kubectl delete cronjob my-cronjob
```


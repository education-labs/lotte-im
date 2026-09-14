# Task 2 - Prometheus & Grafana

1. helm 다운로드

```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
```

2. 권한 수정

```
chmod 700 get_helm.sh
```

3. 헬름 실행

```
./get_helm.sh
```

4. ns 생성

```
kubectl create namespace monitoring
```

5. 헬름 repo 추가

```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

6. helm 을 이용하여 prometheus & grafana 설치

```
helm install prometheus prometheus-community/kube-prometheus-stack --namespace monitoring
```

7. grafana svc 를 NoePort 타입으로 수정

```
kubectl edit svc prometheus-grafana -n monitoring
```

```
맨 하단 type: NodePort 로 수정
```

8. grafana svc의 dns 주소 확인 및 웹브라우저 접속

```
kubectl get svc prometheus-grafana -n monitoring
```

9. 접속

```
http://<NodePublicIP>:<위에서확인한Nodeport>
```

```
로그인 계정 
admin/prom-operator
```

{% hint style="info" %}
혹시 패스워드가 맞지 않다면 아래 명령으로 패스워드 확인

{% code overflow="wrap" %}
```
kubectl get secret --namespace monitoring prometheus-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```
{% endcode %}
{% endhint %}

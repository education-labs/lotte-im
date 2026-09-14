# Task 2 - Cluster init, join

1. 각 노드에서 hostname을 변경

CP Node

```
sudo -i
sudo hostnamectl set-hostname k8s-cp
sudo -i
```

```
sudo -i
```



worker1 Node

```
sudo -i
sudo hostnamectl set-hostname k8s-worker1
sudo -i
```

```
sudo -i
```



worker2 Node

```
sudo -i
sudo hostnamectl set-hostname k8s-worker2
sudo -i
```

```
sudo -i
```





2. 클러스터 선언 (Only CP Node)

```
kubeadm init
```





3. 2 명령 수행이 완료되면 맨 하단에 join 명령어가 출력, 해당 명령을 복사하여 메모장에 저장

<figure><img src="../../.gitbook/assets/image (24) (1).png" alt=""><figcaption></figcaption></figure>





4. CP 노드 인증 (Only CP Node)

```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
```

```
sudo chown $(id -u):$(id -g) $HOME/.kube/config
export KUBECONFIG=/etc/kubernetes/admin.conf
```





5. kubectl 자동완성 적용 (Only CP Node)

```
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc
source /etc/bash_completion
echo alias k=kubectl >> ~/.bashrc
source ~/.bashrc
complete -F __start_kubectl k
```





6. 네트워크 플러그인 설치 (Only CP Node)

```
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.31.2/manifests/calico.yaml
```





7. 정상상태 확인 (Only CP Node)

```
kubectl get pods -n kube-system
```

<figure><img src="../../.gitbook/assets/image (25) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
모든 Pod의 상태가 1/1 Running 이 되야 정상입니다.

1\~2분 후 명령어를 다시 실행하여 확인합니다.
{% endhint %}



<figure><img src="../../.gitbook/assets/image (26) (1).png" alt=""><figcaption></figcaption></figure>





8. worker 1, worker 2 노드 에서 위 3단계에서 저장했던 Join 명령 수행

{% hint style="danger" %}
CP 노드가 아니라 Worker 노드에서 진행합니다.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (27) (1).png" alt=""><figcaption></figcaption></figure>





9. Join 명령을 진행했다면 다시 CP 노드에서 노드확인

{% hint style="success" %}
CP 노드에서 진행합니다.
{% endhint %}

```
kubectl get node
```

<figure><img src="../../.gitbook/assets/image (584).png" alt=""><figcaption></figcaption></figure>

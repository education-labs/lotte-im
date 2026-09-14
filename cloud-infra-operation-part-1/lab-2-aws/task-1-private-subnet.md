# Task 1 - Private Subnet

1. Lab 1 > Task 2 를 참고하여 아래와같은 서브넷 2개를 생성

{% code overflow="wrap" %}
```
서브넷 1 
- 서브넷 이름 : user00-pri-subnet-1
- 가용 영역 : ap-northeast-2a
- IPv4 서브넷 CIDR 블록 : 10.0.10.0/24

서브넷 2
- 서브넷 이름 : user00-pri-subnet-2
- 가용 영역 : ap-northeast-2c
- IPv4 서브넷 CIDR 블록 : 10.0.20.0/24
```
{% endcode %}

{% hint style="info" %}
이때 위 서브넷 2개는 퍼블릭 IPv4 주소 자동 할당 활성화 옵션은 활성화 하지 않습니다
{% endhint %}







2. 서브넷 목록을 확인하여 총 4개의 서브넷이 구성됨을 확인

<figure><img src="../../.gitbook/assets/image (395).png" alt=""><figcaption></figcaption></figure>


# Task 1 - GenAI 기반 Terraform  코드 생성

1. 디렉토리 생성

```
mkdir ~/environment/vpc
```

<figure><img src="../../.gitbook/assets/image (999).png" alt=""><figcaption></figcaption></figure>



2. 다음 조건을 모두 만족하는 VPC를 구성

{% hint style="info" icon="label" %}
* 리전은 서울(`ap-northeast-2`)이어야 한다.
* VPC 이름(Name 태그)은 `user##-vpc`여야 한다.
* 기본(Primary) CIDR은 `10.0.0.0/16`이어야 한다.
* 보조(Secondary) CIDR `10.1.0.0/16`이 같은 VPC에 추가로 연결되어 있어야 한다.
* DNS Support가 켜져 있어야 한다.
* DNS Hostnames가 켜져 있어야 한다.
* Instance Tenancy는 `default`로 명시되어 있어야 한다.
* `domain-name = lab.internal`, `domain-name-servers = AmazonProvidedDNS` 값을 가진 커스텀 DHCP 옵션셋이 존재해야 한다.
* 위 DHCP 옵션셋이 VPC 기본값(`default` 옵션셋)이 아니라 `user##-vpc`에 실제로 연결(association)되어 있어야 한다.
{% endhint %}



3. 생성형 AI에게 질의하여 코드를 생성

{% hint style="warning" %}
**주의:** 스펙을 통째로 복사-붙여넣기 하지 말고, 자연어로 재구성해서 프롬프트를 작성해보세요.
{% endhint %}



4. 생성한 코드를 C9의 파일로 생성

<figure><img src="../../.gitbook/assets/image (1000).png" alt=""><figcaption></figcaption></figure>



5. 코드 내용을 넣고, Ctrl + S 키를 입력

<figure><img src="../../.gitbook/assets/image (1001).png" alt=""><figcaption></figcaption></figure>





6. &#x20;파일명을 임의로 지정하고, 디렉토리를 선택한 뒤 Save 클릭

<figure><img src="../../.gitbook/assets/image (1002).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
코드 내 user## 가 있다면 ## 에는 유저넘버를 입력합니다.
{% endhint %}


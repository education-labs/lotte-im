# Task 1 - VPC 구성

1. 아래 정보를 참고하여 VPC 구성

{% code overflow="wrap" %}
```
VPC 구성

Region : ap-northeast-2
VPC Name : user**-vpc
VPC CIDR : 10.**.0.0/16
```
{% endcode %}





2. 아래 정보를 참고하여 Subnet 구성

{% code overflow="wrap" %}
```
Subnet 구성

1 Subnet (퍼블릭 서브넷 구성)
Subnet Name : user**-pub-subnet1
Subnet CIDR : 10.**.1.0/24
AZ : ap-northeast-2a

2 Subnet (퍼블릭 서브넷 구성)
Subnet Name : user**-pub-subnet2
Subnet CIDR : 10.**.2.0/24
AZ : ap-northeast-2c

3 Subnet (프라이빗 서브넷 구성)
Subnet Name : user**-pri-subnet1
Subnet CIDR : 10.**.10.0/24
AZ : ap-northeast-2a

4 Subnet (프라이빗 서브넷 구성)
Subnet Name : user**-pri-subnet2
Subnet CIDR : 10.**.20.0/24
AZ : ap-northeast-2c
```
{% endcode %}



3. Internet Gateway, NAT Gateway, Routing Table 구성

{% hint style="info" %}
NACL 은 구성 하지 않습니다.
{% endhint %}


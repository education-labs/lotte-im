# Challenge

1. VPC 및 네트워크 서비스 생성

```
user**-vpc-2
cidr : 192.168.0.0/16

    Pub-Subnet
    cidr : 192.168.**.0/24

    Pri-Subnet
    cidr : 192.168.1**.0/24
```



2. 각 서브넷별 Pub/Pri 구성   <br>
3. NACL 을 통한 인입 트래픽 통제

* 외부통신은 Pub만 가능하며, Pri 서브넷은 Pub 에서만 모든 통신 가능하게 설정



4. VPC 간 통신 연결 (기술 서칭하여 구성)

* Lab 1 에서 생성한 VPC <-> Challenge 에서 생성한 VPC


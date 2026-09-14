---
hidden: true
---

# 항목설명

**기본(Primary) CIDR `10.0.0.0/16`**: VPC를 만들 때 반드시 지정해야 하는 IP 주소 범위입니다. `/16`이면 약 65,536개의 사설 IP를 이 VPC 안에서 쓸 수 있습니다. 이후 Lab2에서 이 범위를 쪼개 서브넷을 만들게 되므로, 지금은 "얼마나 큰 네트워크를 만들 것인가"를 정하는 단계입니다.

**보조(Secondary) CIDR `10.1.0.0/16`**: VPC는 기본 CIDR 하나로 IP가 부족해지면 나중에 CIDR 블록을 추가로 붙일 수 있습니다. 실무에서는 VPC 설계 초기에 IP 대역을 너무 작게 잡았거나, 피어링·온프레미스 연결 때문에 특정 대역을 추가로 확보해야 할 때 이 기능을 씁니다. Terraform에서는 `aws_vpc`의 `cidr_block`에 두 개를 넣을 수 없고, `aws_vpc_ipv4_cidr_block_association`이라는 별도 리소스로 붙여야 한다는 점이 이 스펙의 핵심입니다(ChatGPT가 자주 놓치는 부분).

**DNS Support / DNS Hostnames**: `enable_dns_support`는 VPC 내부에서 AWS가 제공하는 DNS 서버(Route 53 Resolver 등)로 이름을 해석할 수 있게 하는 설정이고, `enable_dns_hostnames`는 이 VPC 안의 인스턴스에 퍼블릭/프라이빗 DNS 호스트네임을 자동으로 부여할지 여부입니다. 둘 다 켜야 이후 Lab(EC2, RDS, ALB 등)에서 도메인 이름으로 리소스에 접근할 수 있어, 대부분의 실무 VPC에서 기본으로 켜두는 값입니다.

**Instance Tenancy `default`**: 이 VPC에서 뜨는 EC2가 공유 하드웨어(`default`)를 쓸지 전용 하드웨어(`dedicated`)를 쓸지 정하는 값입니다. `dedicated`는 물리 서버를 독점해서 비용이 훨씬 비싸며, 주로 라이선스·컴플라이언스 요구사항 때문에 씁니다. 굳이 명시적으로 `default`를 코드에 쓰게 하는 이유는 "생략하면 기본값이 알아서 들어간다"가 아니라 "의도를 코드로 명시한다"는 습관을 들이기 위함입니다.

**DHCP 옵션셋 (`domain-name`, `domain-name-servers`)**: VPC 안의 인스턴스가 부팅할 때 자동으로 받는 네트워크 설정값입니다. `domain_name = "lab.internal"`은 인스턴스에 내부 도메인 접미사를 부여하고, `domain_name_servers = ["AmazonProvidedDNS"]`는 AWS가 기본 제공하는 DNS 리졸버를 쓰겠다는 뜻입니다(직접 구축한 내부 DNS 서버 IP를 넣을 수도 있음). 사내망과 연동하거나 커스텀 도메인 체계를 쓸 때 이 옵션셋을 커스터마이징하게 됩니다.

**DHCP 옵션셋 연결(association)**: 옵션셋을 만들기만 하고 VPC에 연결하지 않으면 아무 효과가 없습니다. AWS는 VPC마다 기본으로 `default` DHCP 옵션셋을 이미 연결해두는데, 커스텀 옵션셋을 실제로 적용하려면 `aws_vpc_dhcp_options_association`으로 명시적으로 갈아 끼워야 합니다. 검증 체크리스트 7번이 바로 이걸 확인하는 항목입니다 — 옵션셋은 만들어졌는데 VPC의 `DhcpOptionsId`가 여전히 `default`인 경우가 흔한 실수이기 때문입니다.

**태그 (`Name`, `Environment=lab`)**: 리소스 자체 기능과는 무관하지만, 실무에서 비용 추적·리소스 필터링·자동화 스크립트의 기준이 되는 메타데이터입니다. 이 실습에서도 Step 3 검증 단계에서 `Name` 태그로 본인 리소스만 걸러서 조회하기 때문에, 태그가 빠지면 검증 자체가 되지 않습니다.

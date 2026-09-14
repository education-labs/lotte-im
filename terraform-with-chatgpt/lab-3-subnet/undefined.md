---
hidden: true
---

# 항목 설명

**VPC를 매 Lab마다 새로 만드는 이유**: Lab마다 폴더(state)가 분리되어 있으면 서로 영향을 주지 않고 독립적으로 apply/destroy할 수 있어 실습 관리가 쉬워집니다. 대신 이번처럼 하위 리소스(서브넷)가 상위 리소스(VPC)에 의존할 때는, 매 Lab에서 상위 리소스까지 함께 코드로 포함시켜야 한다는 트레이드오프가 있습니다. 실무에서는 이 문제를 `data` 소스로 기존 리소스를 조회하거나, `terraform_remote_state`/Terragrunt 같은 도구로 해결하는데, 이 부분은 과정 후반부에서 다룰 수 있습니다.

**AZ를 나누는 이유**: 서브넷을 하나의 AZ에만 몰아두면 그 AZ에 장애가 생겼을 때 전체 서비스가 죽습니다. Public/Private 각각 2개 AZ에 분산시켜두는 것은 이후 Lab8(LB), Lab9(ASG), Lab10(RDS Multi-AZ)에서 고가용성 구성을 하기 위한 최소 전제조건입니다.

**Public/Private CIDR을 `10.0.1~2.0/24`, `10.0.11~12.0/24`로 나눈 이유**: 뒷자리를 붙여서 순서대로 쓰지 않고 일부러 번호를 띄운 이유는, 실무에서 흔히 쓰는 방식대로 티어별로 대역을 구획해두면 나중에 같은 티어 안에서 서브넷을 추가할 여유 공간이 남기 때문입니다.

**보조 CIDR 위의 서브넷과 `depends_on`**: `aws_vpc_ipv4_cidr_block_association`으로 보조 CIDR을 붙이는 작업은 AWS 내부적으로 `associating` → `associated` 상태 전환에 시간이 조금 걸립니다. 이 전환이 끝나기 전에 `aws_subnet`이 그 CIDR 범위를 참조하면 `InvalidSubnetRange` 오류가 나거나, 운이 나쁘면 레이스 컨디션으로 가끔 성공하고 가끔 실패하는 불안정한 코드가 됩니다. Terraform은 이 순서를 자동으로 추론하지 못하므로 `depends_on = [aws_vpc_ipv4_cidr_block_association.secondary]`를 **명시적으로** 코드에 넣어야 합니다.

**`map_public_ip_on_launch`**: 이 값이 `true`인 서브넷에서 띄운 EC2는 퍼블릭 IP를 자동으로 받습니다. 아직 IGW도, 라우팅 테이블도 없는 지금 단계에서는 이 값이 있어도 실제 인터넷 통신은 안 되지만, "이 서브넷은 나중에 퍼블릭 용도로 쓸 것"이라는 설계 의도를 미리 코드에 박아두는 것입니다.

**`Tier` 태그**: 이후 Lab에서 `for_each`나 `data "aws_subnets"` 필터링으로 "Tier가 Public인 서브넷만 골라서 라우팅 테이블에 연결" 같은 자동화를 하게 되므로, 태그로도 티어를 구분해두는 습관을 이번 Lab부터 들입니다.

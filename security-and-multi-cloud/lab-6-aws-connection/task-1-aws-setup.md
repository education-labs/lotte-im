# Task 1 - AWS 측 구성 (VPC · EC2 · VGW · CGW)

> 🟧 **AWS 콘솔**에서 진행합니다. 리전이 **서울(ap-northeast-2)** 인지 먼저 확인하세요.

## 1-1. VPC와 EC2

1. AWS 콘솔 → VPC 서비스 → \[VPC 생성] 클릭, 이름 `aws-vpc-user**`, IPv4 CIDR `172.16.**.0/24` 입력 후 생성

<figure><img src="../../.gitbook/assets/image.png" alt="" width="467"><figcaption></figcaption></figure>

> 계정을 공유하는 실습에서 같은 대역을 쓰면 콘솔 목록에 **같은 대역 VPC가 여러 개** 보여, 뒤 단계에서 EC2·VGW·라우팅 테이블을 **엉뚱한 VPC에 만드는 사고**가 자주 납니다.
>
> → 반드시 이름(`aws-vpc-user**`)으로 구분하고, 이후 모든 화면에서 VPC 선택란을 한 번씩 확인하세요. → 이 대역은 **Lab 2 Task 1 대역 설계표의 'AWS VPC' 행**과 같은 값이어야 합니다. Task 2의 정적 경로(= Azure 대역)와 Task 3의 LNG 주소 공간(= 이 AWS 대역)에 그대로 다시 쓰입니다.

3. \[서브넷] → \[서브넷 생성] → VPC 선택 → 이름 `subnet-private-user**`, CIDR `172.16.**.0/25` 입력 후 생성.

<figure><img src="../../.gitbook/assets/image (1).png" alt="" width="404"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (2).png" alt="" width="350"><figcaption></figcaption></figure>

3. EC2 서비스 → \[인스턴스 시작] → 이름 `ec2-user**` , AMI : **Amazon Linux 2023을** 선택합니다.

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>



4. 인스턴스 유형 **t3.micro** 선택, 키 페어 : key-user\*\* 으로 생성하고, 다운로드합니다. 그리고, 네트워크 설정 \[편집] → VPC와 서브넷을 3번에서 만든 것으로 지정합니다.

<figure><img src="../../.gitbook/assets/image (4).png" alt="" width="375"><figcaption></figcaption></figure>



5. \[인스턴스 시작] → 생성 후 **사설 IP를 메모**합니다 (②번 값)

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>



## 1-2. VGW — AWS 쪽 게이트웨이

1. VPC → \[가상 프라이빗 게이트웨이] → \[가상 프라이빗 게이트웨이 생성]

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>





2. 이름 `vgw-aws-user**`, ASN은 **Amazon 기본 ASN**(64512) 선택 후 생성

<figure><img src="../../.gitbook/assets/image (7).png" alt="" width="563"><figcaption></figcaption></figure>



3. 생성된 VGW 선택 → \[작업] → \[VPC에 연결] → 방금 만든 VPC 선택

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>



3. 상태가 **Attached** 로 바뀌는지 확인합니다.

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

> ⚠️ **VGW는 만들기만 하면 동작하지 않습니다.** 반드시 VPC에 연결(Attach)해야 하며, 상태가 Attached인지 확인하세요. 이 단계를 빠뜨리면 뒤에서 터널이 붙어도 트래픽이 흐르지 않습니다.

## 1-3. CGW — Azure를 "상대편 장비"로 등록

1. VPC → \[고객 게이트웨이] → \[고객 게이트웨이 생성]

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>



2.  이름 `cgw-azure-user**`, BGP ASN `65515` 입력, **IP 주소에 ①번 값(Azure 게이트웨이 공용 IP)** 을 입력 한뒤 \[고객 게이트웨이 생성] 클릭<br>

    <figure><img src="/broken/files/i9qwoEuSixzub6LJpOqM" alt="" width="458"><figcaption></figcaption></figure>



| 항목      | 값                  | 출처                    |
| ------- | ------------------ | --------------------- |
| 이름      | cgw-azure-user\*\* | —                     |
| IP 주소   | ① Azure GW 공용 IP   | Lab 5에서 메모 ★          |
| BGP ASN | 65515              | Azure VPN Gateway 기본값 |

> 💡 **이름이 헷갈리는 지점입니다.** "Customer Gateway"는 AWS 입장에서 **상대편 장비**라는 뜻입니다. 원래는 고객사 IDC 라우터를 가리키는 이름인데, 지금 시나리오에서는 그 자리에 **Azure**가 들어갑니다. Azure 쪽 Local Network Gateway와 정확히 대칭입니다.
>
> ⚠️ **IP를 잘못 넣어도 이 단계에서는 오류가 나지 않습니다.** AWS는 등록만 해줍니다. 문제는 나중에 터널이 계속 Down으로 남는 형태로 나타나니, 지금 한 번 더 확인하세요.
>
> ⚠️ **양측 ASN은 서로 달라야 합니다.** 64512(Amazon)와 65515(Azure)는 다르므로 그대로 쓰면 됩니다.

* [ ] VGW 상태가 **Attached** 인가
* [ ] CGW의 IP가 Azure 게이트웨이 공용 IP와 일치하는가
* [ ] EC2가 실행 중이고 **사설 IP를 메모(②)** 했는가

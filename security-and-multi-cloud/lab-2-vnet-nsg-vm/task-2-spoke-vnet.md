# Task 2 - 스포크 VNet과 서브넷 생성

1. 포털 상단 검색창에 "가상 네트워크"를 입력하고 서비스로 이동합니다.
2. \[만들기]를 클릭합니다.

<figure><img src="../../.gitbook/assets/image (1055).png" alt=""><figcaption></figcaption></figure>

3. 리소스 그룹을 새로 만듭니다. (rg-user\*\*)

<figure><img src="../../.gitbook/assets/image (1056).png" alt="" width="563"><figcaption></figcaption></figure>



3. 이름과 지역을 입력합니다.

<figure><img src="../../.gitbook/assets/image (1057).png" alt=""><figcaption></figcaption></figure>



3. \[IP 주소] 탭에서 주소 공간을 `10.**.8.0/22` 로 설정하고 서브넷의 default 를 클릭합니다.

<figure><img src="../../.gitbook/assets/image (1058).png" alt=""><figcaption></figcaption></figure>



3. 아래와 같이 서브넷을 편집하고 저장을 클릭합니다.

| 항목   | 값              |
| ---- | -------------- |
| 이름   | snet-web       |
| 시작주소 | 10.\*\*.8.0/24 |

<figure><img src="../../.gitbook/assets/image (1059).png" alt="" width="493"><figcaption></figcaption></figure>









3. 수정된 것을 확인하고, 서브넷 추가를 클릭하여 같은 방법으로 세 개를 더 추가합니다. (app / db / mgmt)

<figure><img src="../../.gitbook/assets/image (1060).png" alt=""><figcaption></figcaption></figure>

| 항목   | 값              |
| ---- | -------------- |
| 이름   | snet-app       |
| 시작주소 | 10.\*\*.9.0/24 |

| 항목   | 값               |
| ---- | --------------- |
| 이름   | snet-db         |
| 시작주소 | 10.\*\*.10.0/24 |

| 항목   | 값               |
| ---- | --------------- |
| 이름   | snet-mgmt       |
| 시작주소 | 10.\*\*.11.0/24 |

<figure><img src="../../.gitbook/assets/image (1061).png" alt=""><figcaption></figcaption></figure>





3. \[검토 + 만들기] → \[만들기]







* [ ] 스포크 VNet과 서브넷 4개가 생성되었는가

# Task 5 - Network ACL

0\. 서브넷 항목에서 서브넷 이름에 마우스 커서를 올리면 편집버튼이 활성화

* 아래 처럼 이름을 수정

{% code overflow="wrap" %}
```
user**-subnet-1 -> user**-subnet-1 (Web)
user**-subnet-2 -> user**-subnet-2 (Web)
user**-pri-subnet-1 -> user**-pri-subnet-1 (WAS)
user**-pri-subnet-2 -> user**-pri-subnet-2 (DB)
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (416).png" alt=""><figcaption></figcaption></figure>





1. 좌측 메뉴 네트워크 ACL 클릭, user\*\* 검색, 하단에 출력되는 네트워크 ACL ID 클릭

<figure><img src="../../.gitbook/assets/image (425).png" alt=""><figcaption></figcaption></figure>





2. 인바운드 규칙 > 인바운드 규칙 편집 클릭

<figure><img src="../../.gitbook/assets/image (426).png" alt=""><figcaption></figcaption></figure>



3. 아래와 같이 설정 후 변경 사항 저장 클릭

규칙 번호 100

* 유형 : SSH(22)
* 소스 : 소스에는 [https://www.whatismyip.com/](https://www.whatismyip.com/) 에서 나오는 IPv4 Address로 입력
  * 예) 1.2.3.4 로 확인됬다면, 1.2.3.&#x34;_**/32**_ 로 입력
* 허용/거부 : 허용

<figure><img src="../../.gitbook/assets/image (391).png" alt="" width="347"><figcaption></figcaption></figure>



규칙 번호 200&#x20;

* 유형 : SSH(22)
* 소스 : 0.0.0.0/0
* 허용/거부 : 거부



규칙 번호 300&#x20;

* 유형 : 모든 트래픽
* 소스 : 0.0.0.0/0
* 허용/거부 : 허용

<figure><img src="../../.gitbook/assets/image (488).png" alt=""><figcaption></figcaption></figure>







4. 새로운 WAS 용  NACL 생성

* 이름 : user\*\*-was-nacl
* VPC : user\*\*-vpc

<figure><img src="../../.gitbook/assets/image (429).png" alt=""><figcaption></figcaption></figure>



5. 해당 NACL 인바운드 규칙 편집

<figure><img src="../../.gitbook/assets/image (430).png" alt=""><figcaption></figcaption></figure>





6. 규칙 추가 및 변경사항 저장 클릭

<table><thead><tr><th>규칙 번호</th><th>유형</th><th width="234">소스</th><th>허용/거부</th></tr></thead><tbody><tr><td>100</td><td>모든트래픽</td><td>10.0.1.0/24</td><td>허용</td></tr><tr><td>200</td><td>모든트래픽</td><td>10.0.2.0/24</td><td>허용</td></tr><tr><td>300</td><td>모든트래픽</td><td>10.0.20.0/24</td><td>허용</td></tr></tbody></table>

<figure><img src="../../.gitbook/assets/image (328).png" alt=""><figcaption></figcaption></figure>





7. 아웃바운드 규칙 편집

<table><thead><tr><th>규칙 번호</th><th>유형</th><th width="234">소스</th><th>허용/거부</th></tr></thead><tbody><tr><td>100</td><td>모든트래픽</td><td>0.0.0.0/0</td><td>허용</td></tr></tbody></table>

<figure><img src="../../.gitbook/assets/image (491).png" alt=""><figcaption></figcaption></figure>





8. 서브넷 연결 > 서브넷 연결 편집

<figure><img src="../../.gitbook/assets/image (435).png" alt=""><figcaption></figcaption></figure>



9. WAS 서브넷을 선택 한 뒤 변경사항 저장

<figure><img src="../../.gitbook/assets/image (436).png" alt=""><figcaption></figcaption></figure>





10. 새로운 DB 용 NACL 생성

* 이름 : user\*\*-db-nacl
* VPC : user\*\*-vpc

<figure><img src="../../.gitbook/assets/image (428).png" alt=""><figcaption></figcaption></figure>



11. 인바운드 규칙 편집

<figure><img src="../../.gitbook/assets/image (432).png" alt=""><figcaption></figcaption></figure>





12. 규칙 추가 및 변경사항 저장 클릭

<table><thead><tr><th width="100">규칙 번호</th><th width="168">유형</th><th>포트범위</th><th width="171">소스</th><th>허용/거부</th></tr></thead><tbody><tr><td>100</td><td>사용자지정  TCP</td><td>6379</td><td>10.0.10.0/24</td><td>허용</td></tr><tr><td>101</td><td>사용자지정  TCP</td><td>3306</td><td>10.0.10.0/24</td><td>허용</td></tr></tbody></table>

<figure><img src="../../.gitbook/assets/image (433).png" alt=""><figcaption></figcaption></figure>



13. 아웃바운드 규칙 편집

<table><thead><tr><th>규칙 번호</th><th>유형</th><th width="234">소스</th><th>허용/거부</th></tr></thead><tbody><tr><td>100</td><td>모든트래픽</td><td>0.0.0.0/0</td><td>허용</td></tr></tbody></table>

<figure><img src="../../.gitbook/assets/image (490).png" alt=""><figcaption></figcaption></figure>



14. 서브넷 연결 > 서브넷 연결 편집

<figure><img src="../../.gitbook/assets/image (437).png" alt=""><figcaption></figcaption></figure>





15. DB 서브넷을 선택 한뒤 변경사항 저장 클릭

<figure><img src="../../.gitbook/assets/image (438).png" alt=""><figcaption></figcaption></figure>

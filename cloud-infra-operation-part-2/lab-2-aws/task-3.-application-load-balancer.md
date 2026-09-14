# Task 3. Application Load Balancer



1. EC2 메뉴중 로드밸런서 클릭, 로드밸런서 생성 클릭

<figure><img src="../../.gitbook/assets/image (96).png" alt=""><figcaption></figcaption></figure>



2. Application Load Balancer  생성 클릭&#x20;

<figure><img src="../../.gitbook/assets/image (97).png" alt=""><figcaption></figcaption></figure>



3. 다음과 같이 입력합니다.

* 이름 : user\*\*-alb
* 체계 : 인터넷 경계

<figure><img src="../../.gitbook/assets/image (98).png" alt=""><figcaption></figcaption></figure>

* vpc : user\*\*-vpc
* 가용영역 및 서브넷 : 보이는 Pub 서브넷 모두 선택

<figure><img src="../../.gitbook/assets/image (99).png" alt=""><figcaption></figcaption></figure>

* 보안그룹 : 새 보안그룹을 생성 클릭

<figure><img src="../../.gitbook/assets/image (100).png" alt=""><figcaption></figcaption></figure>



6. 보안그룹 설정을 아래와 같이 설정합니다.

* 이름 : user\*\*-alb-sg
* 설명 : user\*\*-alb-sg
* vpc : user\*\*-vpc
* 인바운드 규칙&#x20;
  * 유형 : http
  * 소스 : Anywhere IPv4

그리고 보안그룹 생성 클릭.

<figure><img src="../../.gitbook/assets/image (101).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (102).png" alt=""><figcaption></figcaption></figure>



7. 다시 ALB 생성 화면으로 이동하여 새로고침 버튼을 클릭하고 위에서 만든 보안그룹을 선택

<figure><img src="../../.gitbook/assets/image (103).png" alt=""><figcaption></figcaption></figure>



8. 아래 대상그룹을 생성 클릭

<figure><img src="../../.gitbook/assets/image (104).png" alt=""><figcaption></figcaption></figure>



9. 아래와 같이 설정

* 대상 유형 : 인스턴스
* 대상 그룹 이름 : user\*\*-tg
* 프로토콜 : HTTP&#x20;
* 포트 : 80

<figure><img src="../../.gitbook/assets/image (105).png" alt=""><figcaption></figcaption></figure>



* vpc : user\*\*-vpc 선택

<figure><img src="../../.gitbook/assets/image (106).png" alt=""><figcaption></figcaption></figure>



다음 클릭&#x20;

<figure><img src="../../.gitbook/assets/image (107).png" alt=""><figcaption></figcaption></figure>



10. lb-vm1, lb-vm2 를 선택하고 아래에 보류 중인 것으로 포함 클릭

<figure><img src="../../.gitbook/assets/image (108).png" alt=""><figcaption></figcaption></figure>



11. 대상 보기에서 추가 된것을 확인후 다음 클릭

<figure><img src="../../.gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>



12. 대상그룹 생성 클릭

<figure><img src="../../.gitbook/assets/image (110).png" alt=""><figcaption></figcaption></figure>



13. 다시 ALB 생성 화면으로 돌아와서 새로고침 버튼을 클릭 후 방금생성한 대상그룹 선택

<figure><img src="../../.gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>



14. 로드밸런서 생성 클릭

<figure><img src="../../.gitbook/assets/image (112).png" alt=""><figcaption></figcaption></figure>



15. EC2 좌측 메뉴중 보안그룹 클릭, 보안 그룹 중에 user\*\*-lb-vm-sg 를 찾아 좌측 해당 보안그룹을 선택, 인바운드 규칙 탭 클릭, 인바운드 규칙 편집 클릭

<figure><img src="../../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>



16. 기존 규칙을 삭제

<figure><img src="../../.gitbook/assets/image (114).png" alt=""><figcaption></figcaption></figure>



17. 규칙 추가 클릭

* 유형 : HTTP&#x20;
* 소스 : 사용자 정의

우측의 돋보기 아이콘 클릭, 보안그룹 중 user\*\*-alb-sg 선택&#x20;

<figure><img src="../../.gitbook/assets/image (115).png" alt=""><figcaption></figcaption></figure>



18. 규칙 저장 클릭

<figure><img src="../../.gitbook/assets/image (116).png" alt=""><figcaption></figcaption></figure>

19. EC2 좌측 메뉴중 대상 그룹으로 이동 한 뒤 생성한 대상그룹의 이름을 클릭

<figure><img src="../../.gitbook/assets/image (117).png" alt="" width="452"><figcaption></figcaption></figure>



20. 아래 두 vm의 상태가 Healthy 상태인것을 확인

<figure><img src="../../.gitbook/assets/image (118).png" alt=""><figcaption></figcaption></figure>



21. EC2 좌측 메뉴중 로드밸런서 클릭, 생성한 로드밸런서의 우측 DNS이름을 복사

<figure><img src="../../.gitbook/assets/image (119).png" alt=""><figcaption></figcaption></figure>





22. 브라우저에서 접속 테스트, 접속이 잘된다면 새로고침하여 두 메시지를 확인

<figure><img src="../../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (121).png" alt=""><figcaption></figcaption></figure>

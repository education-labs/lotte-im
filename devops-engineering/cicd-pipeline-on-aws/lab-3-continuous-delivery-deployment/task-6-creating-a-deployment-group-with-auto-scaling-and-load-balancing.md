# Task 6 - Creating a Deployment Group With Auto Scaling and Load Balancing

실습 소개

로드 밸런서를 생성하고 오토스케일링 그룹을 구성 해봅니다. 배포 그룹을 기존 인스턴스 대신 오토스케일링 그룹으로 설정하여 파이프라인을 수정 해봅니다.



1. EC2 인스턴스에 접속하여 아래 명령으로 my-angular-project 디렉토리의 파일 삭제

```
sudo rm /var/www/my-angular-project/*
```

<figure><img src="../../../.gitbook/assets/image (889).png" alt=""><figcaption></figcaption></figure>



2. 연결 탭을 종료 한 뒤 인스턴스를 중지

<figure><img src="../../../.gitbook/assets/image (890).png" alt=""><figcaption></figcaption></figure>



3. 인스턴스가 중지됨 상태가 되면 선택하고, 작업, 이미지 및 템플릿, 이미지 생성 순서로 클릭

<figure><img src="../../../.gitbook/assets/image (891).png" alt=""><figcaption></figcaption></figure>



4. 이미지 이름을 user##-webserver-ami 로 입력하고 아래 이미지 생성 버튼 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (892).png" alt="" width="563"><figcaption></figcaption></figure></div>



5. AMI 페이지로 이동하여 AMI가 사용가능 상태가 되면, 좌측 메뉴 로드밸런서로 이동

<figure><img src="../../../.gitbook/assets/image (893).png" alt=""><figcaption></figcaption></figure>



6. 로드밸런서 생성을 클릭하고, Application Load Balancer 생성 클릭, 로드밸런서 이름에 user##-alb 입력

<figure><img src="../../../.gitbook/assets/image (894).png" alt=""><figcaption></figcaption></figure>



7. 하단 매핑에 모든 서브넷을 선택한 뒤 리스너 및 라우팅에서 대상그룹 생성 클릭

<figure><img src="../../../.gitbook/assets/image (895).png" alt=""><figcaption></figcaption></figure>



8. 인스턴스를 선택한 뒤 대상 그룹 이름에 user##-alb-target-group 입력 한 뒤, 다음 클릭 한 뒤, 대상그룹 생성 클릭

<figure><img src="../../../.gitbook/assets/image (896).png" alt=""><figcaption></figcaption></figure>



9. 다시 로드밸런서 생성 화면으로 돌아와서 대상그룹 새로고침 버튼 클릭, 그리고 방금 생성했던 타겟그룹을 선택한 뒤 하단 로드밸런서 생성 클릭

<figure><img src="../../../.gitbook/assets/image (897).png" alt=""><figcaption></figcaption></figure>



10. 좌측 메뉴 중 대상그룹으로 이동한 뒤 생성한 타겟 그룹 클릭, 그리고 속성 탭 선택, 편집클릭

<figure><img src="../../../.gitbook/assets/image (898).png" alt=""><figcaption></figcaption></figure>



11.등록 취소지연 시간을 60초로 변경한 뒤 변경 내용 저장 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (899).png" alt="" width="563"><figcaption></figcaption></figure></div>



12. 다시 로드밸런서에서 보안 탭을 클릭하고 보안그룹을 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (900).png" alt="" width="563"><figcaption></figcaption></figure></div>

13. 인바운드 규칙, 인바운드 규칙 편집 클릭

<figure><img src="../../../.gitbook/assets/image (901).png" alt=""><figcaption></figcaption></figure>

아래 규칙을 추가하고 규칙 저장 클릭

<figure><img src="../../../.gitbook/assets/image (902).png" alt=""><figcaption></figcaption></figure>



14. 좌측 메뉴 중 시작 템플릿 선택 후 시작템플릿 생성 클릭

<figure><img src="../../../.gitbook/assets/image (903).png" alt=""><figcaption></figcaption></figure>



15. 시작 템플릿 이름과 설명란에 user##-lt 입력 후 OS이미지를 이전에 생성했던 user##-webserver-ami 를 선택

<figure><img src="../../../.gitbook/assets/image (904).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (905).png" alt=""><figcaption></figcaption></figure>





16. 인스턴스 유형은 t2.micro 를 선택, 키페어는 생성했던 user##-Angular-key 를 선택

<div align="left"><figure><img src="../../../.gitbook/assets/image (906).png" alt="" width="563"><figcaption></figcaption></figure></div>







17. 네트워크 설정에서 보안그룹 생성클릭, 보안그룹 이름과 설명에 user##-lt-sg 입력

<figure><img src="../../../.gitbook/assets/image (908).png" alt=""><figcaption></figcaption></figure>





18. 보안그룹규칙 추가를 클릭하여 아래 처럼 3개의 규칙 추가

룰 1&#x20;

* 유형 : ssh / 소스 유형 : 위치 무관

룰 2&#x20;

* 유형 : http / 소스 유형 : 위치무관

룰 3&#x20;

* 유형 : https / 소스 유형 : 위치무관

<div align="left"><figure><img src="../../../.gitbook/assets/image (909).png" alt="" width="563"><figcaption></figcaption></figure></div>



19. 고급 세부 정보를 클릭하고 IAM 인스턴스 프로파일을 이전에 생성했던 정책을 선택 한 뒤 시작 템플릿 생성 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (910).png" alt="" width="563"><figcaption></figcaption></figure></div>



20. EC2 좌측 메뉴중 Auto Scaling 중 Auto scaling Group 를 클릭하고 Auto scaling Group 생성 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (911).png" alt="" width="563"><figcaption></figcaption></figure></div>



21. 이름란에 user##-asg 입력, 시작템플릿은 이전단계에서 생성한 시작템플릿 선택 한 뒤 다음 클릭

<figure><img src="../../../.gitbook/assets/image (912).png" alt=""><figcaption></figcaption></figure>





22. 가용영역은 출력되는 모든 가용영역 선택한 뒤 다음 클릭

<figure><img src="../../../.gitbook/assets/image (913).png" alt=""><figcaption></figcaption></figure>



23. 기존로드밸런서에 연결 선택, 로드밸런서 대상 그룹에서 선택을 선택, 이전에 생성했던 타겟그룹(대상그룹) 선택

<div align="left"><figure><img src="../../../.gitbook/assets/image (914).png" alt="" width="563"><figcaption></figcaption></figure></div>



24. 상태 확인에서 ELB 상태확인 켜기 체크, 상태 확인 유예기간 1800초로 수정한 뒤 다음 클릭

<figure><img src="../../../.gitbook/assets/image (915).png" alt=""><figcaption></figcaption></figure>



25. 그룹크기를 모두 2로 입력 후 다음 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (916).png" alt="" width="563"><figcaption></figcaption></figure></div>





26. 알림추가, 태그추가 단계는 모두 다음 클릭하여 Auto Scaling 그룹 생성 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (917).png" alt="" width="375"><figcaption></figcaption></figure></div>



27\. CodeDeploy 서비스로 이동하여 좌측 애플리케이션 클릭, user##AngularApp 애플리케이션 클릭

그리고, 배포그룹 생성 클릭

<figure><img src="../../../.gitbook/assets/image (918).png" alt=""><figcaption></figcaption></figure>



28. 배포그룹이름에 user##MyAutoScalingGroup 를 입력하고, 서비스 역할에서 user##CodeDeploy\~\~\~ 을 선택한 뒤, 배포유형에서 현재 위치 선택

<div align="left"><figure><img src="../../../.gitbook/assets/image (919).png" alt="" width="563"><figcaption></figcaption></figure></div>



29\. 환경구성에서 Amazon EC2 Auto Scaling 그룹을 선택하고, user##-asg 선택

그리고 로드밸런서의 대상그룹선택에 user##으로시작하는대상그룹을 선택한 뒤 배포그룹 생성 클릭



<div align="left"><figure><img src="../../../.gitbook/assets/image (920).png" alt=""><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../../.gitbook/assets/image (921).png" alt=""><figcaption></figcaption></figure></div>

<figure><img src="../../../.gitbook/assets/image (922).png" alt=""><figcaption></figcaption></figure>





30. 파이프라인으로 이동 한 뒤 편집 클릭

<figure><img src="../../../.gitbook/assets/image (923).png" alt=""><figcaption></figcaption></figure>





31. Deploy 단계의 스테이지 편집 클릭한 뒤 편집 아이콘 클릭

<figure><img src="../../../.gitbook/assets/image (924).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (925).png" alt=""><figcaption></figcaption></figure>



32. 배포그룹에 user##MyAutoScalingGroup 를 선택하고, 완료 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (926).png" alt=""><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../../.gitbook/assets/image (927).png" alt=""><figcaption></figcaption></figure></div>



33. 완료를 클릭하고, 우측 상단 저장 클릭, 변경사항 릴리스 클릭하여 새롭게 파이프라인이 실행되고 성공 됨을 확인(시간소요)

<figure><img src="../../../.gitbook/assets/image (928).png" alt=""><figcaption></figcaption></figure>

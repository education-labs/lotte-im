# Task 2 - Creating Application and Deployment Group

## 실습 소개

배포 그룹을 생성하여 CodeDeploy로 어플리케이션을 배포하기 위한 구성을 해봅니다.



CodeDeploy 서비스로 이동, 좌측 애플리케이션 클릭, 우측 애플리케이션 생성 클릭

<figure><img src="../../../.gitbook/assets/image (851).png" alt=""><figcaption></figcaption></figure>



2. 애플리케이션 이름 : user##AngularApp , 컴퓨팅 플랫폼 : EC2/온프레미스 선택 후 애플리케이션 생성 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (852).png" alt="" width="563"><figcaption></figcaption></figure></div>



3. IAM 서비스로 이동하여 역할을 클릭, 역할 만들기 클릭, 사용사례 중 CodeDeploy 를 검색하여 CodeDeploy 선택한 뒤 다음 클릭

<figure><img src="../../../.gitbook/assets/image (853).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (854).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (855).png" alt=""><figcaption></figcaption></figure>

4. 다음 클릭



<figure><img src="../../../.gitbook/assets/image (856).png" alt=""><figcaption></figcaption></figure>



5. 역할 이름에 user##CodeDeployEC2ServiceRole 을 입력하고 하단의 역할 생성 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (857).png" alt="" width="563"><figcaption></figcaption></figure></div>



6. 다시 CodeDeploy로 돌아와서 배포그룹 생성을 클릭

<figure><img src="../../../.gitbook/assets/image (986).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (987).png" alt=""><figcaption></figcaption></figure>





7. 배포 그룹 이름 : User##TaggedEC2Instances 서비스 역할 입력 란에 user##을 검색하여 출력하는정책 선택

<div align="left"><figure><img src="../../../.gitbook/assets/image (859).png" alt="" width="563"><figcaption></figcaption></figure></div>



8. 환경구성에서 EC2 인스턴스를 선택하고,

태그 키 : Name

값 : user##-AngularProject

설정

<div align="left"><figure><img src="../../../.gitbook/assets/image (860).png" alt="" width="563"><figcaption></figcaption></figure></div>



9. 로드밸런서에서 로드밸런싱 활성화 체크 해제한 뒤 배포그룹 생성 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (861).png" alt="" width="563"><figcaption></figcaption></figure></div>

---
hidden: true
---

# \[github] Task 6 - Build Stage

실습 소개

기존 Source -> Deploy 단계 대신 Build 단계가 추가된 Source -> Build -> Deploy 파이프라인을 구성 해봅니다.



1. github 에서 새로운 레포지토리를 생성

<figure><img src="../../../.gitbook/assets/image (756).png" alt="" width="375"><figcaption></figcaption></figure>



2. 리포지토리 이름을  User##AngularRepo 로 입력한 뒤 Public을 선택, Create repository 클릭

<figure><img src="../../../.gitbook/assets/image (757).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (758).png" alt="" width="188"><figcaption></figcaption></figure>

3. 리모트 연결 명령을 복사하여 메모장에 저장

<figure><img src="../../../.gitbook/assets/image (759).png" alt=""><figcaption></figcaption></figure>

4. Cloud9으로 돌아가 제공받은 my-angular-project.zip 파일을 /home/ec2-user/environment 디렉토리로 드래그 앤 드랍 하여 업로드

<div align="left"><figure><img src="../../../.gitbook/assets/image (740).png" alt="" width="375"><figcaption></figcaption></figure></div>



5. 압축 해제 후 디렉토리 이동

```
unzip my-angular-project.zip 
cd my-angular-project
```

<div align="left"><figure><img src="../../../.gitbook/assets/image (741).png" alt="" width="563"><figcaption></figcaption></figure></div>



6. 리모트 연결

```
3단계에서 복사한 명령 수행
```

<figure><img src="../../../.gitbook/assets/image (742).png" alt=""><figcaption></figcaption></figure>



8. 푸시 명령어로 소스코드 업로드

```
git push origin master
```

<div align="left"><figure><img src="../../../.gitbook/assets/image (743).png" alt=""><figcaption></figcaption></figure></div>



9. github 레포지토리로 이동하여 푸시된 코드 확인

<figure><img src="../../../.gitbook/assets/image (760).png" alt="" width="366"><figcaption></figcaption></figure>



9. S3 서비스로 이동하여 버킷 생성

버킷 이름 : user##-angular-website

객체 소유권 : ACL 활성화됨

Block all public access : 선택해제

생성 버튼 클릭



11. 생성한 버킷으로 이동 후 속성탭 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (745).png" alt="" width="375"><figcaption></figcaption></figure></div>



12. 최하단으로 스크롤하여 정적 웹사이트 호스팅의 편집 클릭

<figure><img src="../../../.gitbook/assets/image (746).png" alt=""><figcaption></figcaption></figure>



13. 호스팅 활성화를 클릭하고, 인덱스 문서, 오류문서 두개 모두 index.html 로 입력한 뒤 변경사항저장

<div align="left"><figure><img src="../../../.gitbook/assets/image (747).png" alt="" width="375"><figcaption></figcaption></figure></div>



14. Codepipeline으로 이동 하여 파이프라인 생성 클릭

사용자 지정 파이프라인 빌드를 선택   후 다음 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (761).png" alt="" width="563"><figcaption></figcaption></figure></div>



15. 아래와 같이 이름만 설정 후 다음 클릭 (user##-AngularPipeline)

<div align="left"><figure><img src="../../../.gitbook/assets/image (762).png" alt="" width="563"><figcaption></figcaption></figure></div>

16. 소스 공급자는 github(github 앱을 통해) 를 선택하고 Github에 연결 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (763).png" alt="" width="563"><figcaption></figcaption></figure></div>



17. 연결 이름에 user##-angular-github 을 입력하고, Github에 연결 클릭&#x20;

<div align="left"><figure><img src="../../../.gitbook/assets/image (764).png" alt=""><figcaption></figcaption></figure></div>



18. 기존에 생성했던 앱 선택후 하단 연결 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (765).png" alt=""><figcaption></figcaption></figure></div>



19. 리포지토리를 생성했던 user##angularRepo 를 선택하고, 기본브랜치 master 선택한뒤 다음 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (766).png" alt="" width="563"><figcaption></figcaption></figure></div>

20. 빌드 공급자에 기타 빌드공급자를 선택하고, AWS CodeBuild 를 선택, 프로젝트 생성 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (767).png" alt="" width="563"><figcaption></figcaption></figure></div>





21. 프로젝트 이름에는 user##-AngularBuild 을 입력하고, 운영체제 ubuntu, 런타임 Standard, 이미지 aws/codebuild/standard:5.0 을 선택,

<div align="left"><figure><img src="../../../.gitbook/assets/image (768).png" alt="" width="563"><figcaption></figcaption></figure></div>

&#x20;buildspec 파일사용을 선택 한 뒤 하단 Codepipeline 으로 계속 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (769).png" alt="" width="563"><figcaption></figcaption></figure></div>

<div align="left" data-full-width="false"><figure><img src="../../../.gitbook/assets/image (770).png" alt="" width="165"><figcaption></figcaption></figure></div>

19. 그러면 프로젝트가 추가가 되고, 다음 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (752).png" alt="" width="563"><figcaption></figcaption></figure></div>



20. 테스트 스테이지는 건너뛰기 클릭&#x20;

<div align="left"><figure><img src="../../../.gitbook/assets/image (771).png" alt="" width="563"><figcaption></figcaption></figure></div>





21. 아래와 같이 설정 후 다음 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (772).png" alt=""><figcaption></figcaption></figure></div>



20. 파이프라인 생성 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (754).png" alt="" width="290"><figcaption></figcaption></figure></div>



22. Source 단계는 성공, 그러나 Build 에서 실패(3분가량소요), 화살표가가리키는 박스를 클릭하여 로그 확인

<figure><img src="../../../.gitbook/assets/image (773).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (774).png" alt=""><figcaption></figcaption></figure>

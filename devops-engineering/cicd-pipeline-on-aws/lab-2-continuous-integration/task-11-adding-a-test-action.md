# Task 11 - Adding a Test Action

실습 소개

Build 단계에 Unit Test를 추가하여 장애 발생시 소스 업데이트가 되지 않음을 확인해봅니다.



1. Cloud9 에서 buildspec.yml 을 복사, 붙여넣기 하여 복제본을 하나 생성

<div align="left"><figure><img src="../../../.gitbook/assets/image (804).png" alt="" width="318"><figcaption></figcaption></figure></div>



2. 복제본의 파일명을 unit-test-buildspec.yml 로 수정



3. unit-test-buildspec.yml 파일의 내용을 수정한 뒤 저장 (Ctrl+s)

```
- ng test --no-watch --no-progress --browsers=ChromeHeadlessCI
```



전체 코드

```
version: 0.2

phases:
  install:
    runtime-versions:
      nodejs: 12
    commands:
      - npm install -g @angular/cli@9.0.6
  pre_build:
    commands:
      - npm install
  build:
    commands:
      - ng test --no-watch --no-progress --browsers=ChromeHeadlessCI
```

<figure><img src="../../../.gitbook/assets/image (805).png" alt=""><figcaption></figcaption></figure>



4. 파이프라인으로 이동하여 편집을 클릭

<figure><img src="../../../.gitbook/assets/image (806).png" alt=""><figcaption></figcaption></figure>



5. build 스테이지 편집 클릭

<figure><img src="../../../.gitbook/assets/image (807).png" alt=""><figcaption></figcaption></figure>



6. 상단의작업 그룹 추가 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (808).png" alt="" width="502"><figcaption></figcaption></figure></div>



7. 작업이름 : UnitTest, 작업공급자 : AWS CodeBuild, 입력 아티팩트: SourceArtifact를 선택하고 프로젝트 생성을 클릭

<figure><img src="../../../.gitbook/assets/image (809).png" alt=""><figcaption></figcaption></figure>



6. 프로젝트 이름에 User##AngularUnitTest 을 입력하고, 아래와 같이 설정 한 뒤, CodePipeline 으로 계속 을 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (810).png" alt="" width="563"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../../.gitbook/assets/image (811).png" alt="" width="563"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../../.gitbook/assets/image (812).png" alt="" width="563"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../../.gitbook/assets/image (813).png" alt="" width="345"><figcaption></figcaption></figure></div>



7. 프로젝트가 생성된 것을 확인 한 뒤 완료 클릭

<figure><img src="../../../.gitbook/assets/image (814).png" alt=""><figcaption></figcaption></figure>



8. UnitTest 가 추가 된 것을 확인하고 완료 클릭

<figure><img src="../../../.gitbook/assets/image (815).png" alt=""><figcaption></figcaption></figure>



9. 우측 상단 저장 클릭 -> 저장 클릭



10. UnitTest 단계가 추가된것을 확인

<figure><img src="../../../.gitbook/assets/image (816).png" alt=""><figcaption></figcaption></figure>

11\. Cloud9로 이동하여 다시 코드를 수정하여 장애 발생

/home/ec2-user/environment/my-angular-project/src/app/calculator 의\
calculator.component.ts 파일을 오픈하고 , 아래 스샷처럼 수정한 뒤 저장 (Ctrl+S)

<figure><img src="../../../.gitbook/assets/image (817).png" alt=""><figcaption></figcaption></figure>



11. Cloud9의 /home/ec2-user/environment/my-angular-project/src/app 에 있는app.component.html 파일을 수정 (Version: 4.0으로) 한 뒤 저장 (Ctrl+S)

<figure><img src="../../../.gitbook/assets/image (818).png" alt=""><figcaption></figcaption></figure>



12. 터미널에서 git add, commit, push 수행

```
git add .
git commit -m "Version 4.0"
git push origin master
```

<div align="left"><figure><img src="../../../.gitbook/assets/image (819).png" alt="" width="563"><figcaption></figcaption></figure></div>



13. 파이프라인으로 이동하여 대기하여, UnitTest 단계에서 실패확인

<figure><img src="../../../.gitbook/assets/image (820).png" alt="" width="563"><figcaption></figcaption></figure>

14. 웹페이지를 새로고침 (4.0으로 업데이트했지만 테스트에서 실패하여 업데이트 되지않음)

<div align="left"><figure><img src="../../../.gitbook/assets/image (821).png" alt="" width="563"><figcaption></figcaption></figure></div>



15. 계산 테스트 (정상 동작함을 확인)

<figure><img src="../../../.gitbook/assets/image (822).png" alt=""><figcaption></figcaption></figure>



16. Cloud9로 이동하여 /home/ec2-user/environment/my-angular-project/src/app/calculator 의 calculator.component.ts 파일을 오픈하고 , 아래 스샷처럼 수정한 뒤 저장 (Ctrl+S)

<figure><img src="../../../.gitbook/assets/image (823).png" alt=""><figcaption></figcaption></figure>



17. 다시 git commit, push

```
git commit -a -m "Version 4.0 - bug fix"
git push origin master
```

<div align="left"><figure><img src="../../../.gitbook/assets/image (824).png" alt="" width="563"><figcaption></figcaption></figure></div>



18\. 파이프라인으로 이동하여 모든 단계를 성공함을 확인하고 , 웹페이지도 버전 4.0으로 업데이트 된 것과 정상 동작 확인

<figure><img src="../../../.gitbook/assets/image (825).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (826).png" alt=""><figcaption></figcaption></figure>

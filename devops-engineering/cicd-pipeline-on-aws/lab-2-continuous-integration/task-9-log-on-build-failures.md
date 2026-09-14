# Task 9 - Log on Build Failures

## 실습 소개

yml 파일을 수정하여 Build 실패를 발생시켜봅니다. CodeBuild 서비스를 통해 Build 실패 원인을 파악하여 수정해봅니다.



1. Cloud9의 /home/ec2-user/environment/my-angular-project/src/app 에 있는app.component.html 파일을 수정 (Version: 2.0으로) 한 뒤 저장 (Ctrl+S)

<figure><img src="../../../.gitbook/assets/image (785).png" alt=""><figcaption></figcaption></figure>



2. buildspec.yml 파일을 수정

<figure><img src="../../../.gitbook/assets/image (786).png" alt=""><figcaption></figcaption></figure>

<pre><code><strong>build:
</strong>  commands:
    - echo Build error simulation!
    - exit 1
    - ng build --prod
  finally:
    - echo This is the finally block execution!
</code></pre>

전체 파일 내용

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
    - echo Build error simulation!
    - exit 1
    - ng build --prod
  finally:
    - echo This is the finally block execution!
artifacts:
  base-directory: dist/my-angular-project
  files:
    - '**/*'
```



3. 터미널에서 git commit, push

```
git commit -a -m "Build error simulation"
git push origin master
```



4. CodePipeline 서비스의 파이프라인으로 이동하여 실패됨을 확인(2\~5분 소요)

<figure><img src="../../../.gitbook/assets/image (787).png" alt=""><figcaption></figcaption></figure>



5. 웹페이지는 여전히 version 1.0 임을 확인

<div align="left"><figure><img src="../../../.gitbook/assets/image (788).png" alt="" width="563"><figcaption></figcaption></figure></div>



6. 다시 파이프라인으로 이동하여 Build 박스를 클릭

<figure><img src="../../../.gitbook/assets/image (789).png" alt=""><figcaption></figcaption></figure>

7. 자세한 정보를 보기위해 CodeBuild 에서 보기를 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (790).png" alt="" width="375"><figcaption></figcaption></figure></div>

8. 전체 로그를 확인가능

<figure><img src="../../../.gitbook/assets/image (791).png" alt=""><figcaption></figcaption></figure>



9. 요약을 확인가능

<figure><img src="../../../.gitbook/assets/image (792).png" alt=""><figcaption></figcaption></figure>



10. Cloud9으로 이동하여 buildspec.yml 파일의 내용을 아래 내용으로 수정한뒤 저장

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
      - ng build --prod
      - echo "Build completed."

artifacts:
  base-directory: dist/my-angular-project
  files:
    - '**/*'

```



11. 다시 커밋과 푸시

```
git commit -a -m "Build error was fixed"
git push origin master
```





12. 파이프라인으로 이동하여 소스-빌드(2\~5분소요)-배포 단계가 성공함을 확인후 , 홈페이지 버전도 다시 확인

<figure><img src="../../../.gitbook/assets/image (793).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (794).png" alt=""><figcaption></figcaption></figure>

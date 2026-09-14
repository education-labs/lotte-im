# Task 4 - Creating Appspec

실습 소개

Appspec은 CodeDeploy에서 배포를 관리하는데 사용하는 파일입니다. 배포가 정상적으로 될 수 있도록 Appspec 파일을 수정해봅니다.



1. 제공 받은 파일(zip파일과 appspec.yml)을 Cloud9 의 my-angular-project 디렉토리로 업로드 (드래그앤드랍)

<div align="left"><figure><img src="../../../.gitbook/assets/image (870).png" alt="" width="375"><figcaption></figcaption></figure></div>



2. 그리고 터미널에서 압축해제

```
unzip deploy-scripts.zip
```

<div align="left"><figure><img src="../../../.gitbook/assets/image (871).png" alt="" width="563"><figcaption></figcaption></figure></div>



3. 터미널에서 git add, commit, push

```
git add .
git commit -m "Appsec"
git push origin master
```



4. 파이프라인으로 이동하여 5\~10 분 뒤 실패됨을 확인



4. Cloud9 으로 이동하여 buildspec.yml 수정 및 저장 (Ctrl+s)

<div align="left"><figure><img src="../../../.gitbook/assets/image (872).png" alt="" width="563"><figcaption></figcaption></figure></div>

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
      - ng build --prod
      - echo "Build completed."

artifacts:
  files:
    - 'dist/my-angular-project/**/*'
    - appspec.yml
    - 'deploy-scripts/**/*'
```



6. appspec.yml 파일 수정 및 저장 (Ctrl+s)

<div align="left"><figure><img src="../../../.gitbook/assets/image (873).png" alt="" width="563"><figcaption></figcaption></figure></div>

전체 코드

```
version: 0.0
os: linux
files:
  - source: dist/my-angular-project
    destination: /var/www/my-angular-project
permissions:
  - object: /var/www/my-angular-project
    pattern: '**'
    mode: '0755'
    owner: root
    group: root
    type:
      - file
      - directory
hooks:
  ApplicationStart:
    - location: deploy-scripts/application-start-hook.sh
      timeout: 300
```





7. git commit, push 작업

```
git commit -a -m "build artifact"
git push origin master
```





8. 다시 파이프라인으로 돌아가 모든 단계를 성공 확인 , 홈페이지 새로고침해서 확인

<figure><img src="../../../.gitbook/assets/image (874).png" alt="" width="563"><figcaption></figcaption></figure>

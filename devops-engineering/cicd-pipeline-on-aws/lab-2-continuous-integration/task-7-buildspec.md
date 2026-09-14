# Task 7 - Buildspec

## 실습 소개

시도했던 빌드 단계를 yml 파일을 추가하여 재시도해봅니다. 배포 성공을 확인하고 배포된 웹 페이지에 접속해봅니다.



1. 제공 받은 파일(buildspec.yml)을 Cloud9에 my-angular-project 디렉토리에 업로드

<div align="left"><figure><img src="../../../.gitbook/assets/image (775).png" alt="" width="155"><figcaption></figcaption></figure></div>



2. add, commit, push로 코드 업데이트

```
git add . 
git commit -m "buildspec upload"
git push origin master
```

<div align="left"><figure><img src="../../../.gitbook/assets/image (776).png" alt="" width="563"><figcaption></figcaption></figure></div>



3. 다시 Pipeline으로 이동하여 build 단계 확인 (2\~5분소요)

성공화면 (Deploy 까지 성공)

<figure><img src="../../../.gitbook/assets/image (780).png" alt=""><figcaption></figcaption></figure>



4. 정적 웹사이트 호스팅 url로 접속

<div align="left"><figure><img src="../../../.gitbook/assets/image (778).png" alt="" width="563"><figcaption></figcaption></figure></div>



5. 간단히 계산기를 사용 테스트

<div align="left"><figure><img src="../../../.gitbook/assets/image (779).png" alt="" width="563"><figcaption></figcaption></figure></div>

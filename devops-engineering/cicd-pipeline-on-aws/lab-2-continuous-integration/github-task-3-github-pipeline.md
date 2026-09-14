---
hidden: true
---

# \[github] Task 3 - github 기반 Pipeline 구성

실습 소개

Lab 4, 5 에서 구성한 리포지토리로 소스를 푸시하는 github 기반 파이프라인을 생성해봅니다.



1. S3 버킷 중 user##-website-prod 로 이동하여 객체를 삭제

<figure><img src="../../../.gitbook/assets/image (683).png" alt=""><figcaption></figcaption></figure>



2. CodePipeline 으로 이동 후 파이프라인 생성 클릭, 사용자 지정 파이프라인 빌드 선택 후 다음클릭

<figure><img src="../../../.gitbook/assets/image (684).png" alt=""><figcaption></figcaption></figure>



3. 파이프라인 이름 : user##-GithubPipeline 입력 후 다음 클릭

<figure><img src="../../../.gitbook/assets/image (685).png" alt=""><figcaption></figcaption></figure>



4. 소스 공급자에서 Github(Github 앱을 통해) 선택

<figure><img src="../../../.gitbook/assets/image (686).png" alt=""><figcaption></figcaption></figure>



5. Github 에 연결클릭

<figure><img src="../../../.gitbook/assets/image (687).png" alt=""><figcaption></figcaption></figure>



6. 연결 이름에 user##-githubconnect 를 입력한 뒤 Github 에 연결 클릭

<figure><img src="../../../.gitbook/assets/image (688).png" alt=""><figcaption></figcaption></figure>



7. 새 앱 설치 클릭

<figure><img src="../../../.gitbook/assets/image (689).png" alt=""><figcaption></figcaption></figure>



8. All Repositories 선택 후 Install & Authorize 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (690).png" alt="" width="563"><figcaption></figcaption></figure></div>



9. 연결 클릭

<figure><img src="../../../.gitbook/assets/image (691).png" alt=""><figcaption></figcaption></figure>



10. 리포지토리 이름을 클릭하고 cicd\_web 리포지토리를 선택한뒤, 기본 브랜치에 master 선택, 그리고 다음 클릭

<figure><img src="../../../.gitbook/assets/image (692).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (693).png" alt=""><figcaption></figcaption></figure>

<div align="left"><figure><img src="../../../.gitbook/assets/image (694).png" alt="" width="360"><figcaption></figcaption></figure></div>



11. 빌드와 테스트는 건너뛰기 클릭





12. 배포 단계는 아래와 같이 설정 후 다음 클릭, 파이프라인 생성 클릭

```
배포 공급자 : Amazon S3
리전 : 서울
입력 아티팩트 : SourceArtifact
버킷 : user##-website-prod

배포하기 전에 파일 압축 풀기 선택!
추가구성 - 표준 ACL - Public-read 선택!
```

<div align="left"><figure><img src="../../../.gitbook/assets/image (695).png" alt="" width="352"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../../.gitbook/assets/image (696).png" alt="" width="494"><figcaption></figcaption></figure></div>





7. 성공화면, 웹화면도 확인

<figure><img src="../../../.gitbook/assets/image (697).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (698).png" alt=""><figcaption></figcaption></figure>

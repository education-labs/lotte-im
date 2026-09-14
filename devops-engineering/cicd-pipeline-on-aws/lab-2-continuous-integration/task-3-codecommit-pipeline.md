# Task 3 - Codecommit 기반 Pipeline 구성

## 실습 소개

구성한 리포지토리로 소스를 푸시하는 CodeCommit 기반 파이프라인을 생성해봅니다.



1. S3 버킷 중 user##-website-prod 로 이동하여 객체를 삭제

<figure><img src="../../../.gitbook/assets/image (683).png" alt=""><figcaption></figcaption></figure>



2. CodePipeline 으로 이동 후 파이프라인 생성 클릭한 뒤 이름 입력

파이프라인 이름 : user##-CodeCommitPipeline

<div align="left"><figure><img src="../../../.gitbook/assets/image (710).png" alt="" width="563"><figcaption></figcaption></figure></div>



3. 고급 설정 확장후 기본위치, 기본 aws 관리형 키 선택후 다음 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (711).png" alt="" width="563"><figcaption></figcaption></figure></div>



4. 아래와 같이 설정 후 다음 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (712).png" alt="" width="563"><figcaption></figcaption></figure></div>



5. 빌드 단계는 건너뛰기



6. 아래와 같이 설정 후 추가설정 확장한 뒤, 우측과 같이 설정 후 다음 클릭

<figure><img src="../../../.gitbook/assets/image (713).png" alt=""><figcaption></figcaption></figure>



7. 파이프라인 생성 클릭, 성공화면, 웹화면도 확인

<div align="left"><figure><img src="../../../.gitbook/assets/image (714).png" alt="" width="375"><figcaption></figcaption></figure></div>

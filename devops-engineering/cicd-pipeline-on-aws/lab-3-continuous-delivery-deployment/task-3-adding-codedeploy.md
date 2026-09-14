# Task 3 - Adding CodeDeploy

## 실습 소개

기존 S3로 배포했던 작업을 EC2 인스턴스로 배포하기 위해 파이프라인을 수정해봅니다.



1. CodePipeline 으로 이동한 뒤 이전 실습에서 사용했던 user##-AngularPipeline 을 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (862).png" alt="" width="375"><figcaption></figcaption></figure></div>



2. 편집을 클릭, 스테이지 편집 클릭, 기존 있었던 Deploy 삭제

<figure><img src="../../../.gitbook/assets/image (863).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (864).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (865).png" alt=""><figcaption></figcaption></figure>

3. Deploy 단계의 작업그룹 추가를 클릭, 다음과 같이 설정 후 완료 클릭

<figure><img src="../../../.gitbook/assets/image (866).png" alt="" width="458"><figcaption></figcaption></figure>

```
작업이름 : DeployToEC2
작업공급자 : AWS CodeDeploy
리전 : 서울
입력 아티팩트 : BuildArtifact
애플리케이션 이름 : user##AngularApp
배포그룹 : User##TaggedEC2Instances
```





<figure><img src="../../../.gitbook/assets/image (867).png" alt=""><figcaption></figcaption></figure>



4. 완료 클릭

<figure><img src="../../../.gitbook/assets/image (868).png" alt=""><figcaption></figcaption></figure>



5. 우측상단의저장 클릭, 한번 더저장 클릭

<figure><img src="../../../.gitbook/assets/image (869).png" alt=""><figcaption></figcaption></figure>

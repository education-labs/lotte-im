# Task 6 - Build Stage

## 실습 소개

기존 Source -> Deploy 단계 대신 Build 단계가 추가된 Source -> Build -> Deploy 파이프라인을 구성 해봅니다.

1. 서비스 검색창에 Codecommit 검색하고 서비스를 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (659).png" alt="" width="563"><figcaption></figcaption></figure></div>

2. 리포지토리 생성 클릭

<figure><img src="../../../.gitbook/assets/image (660).png" alt=""><figcaption></figcaption></figure>

3. 이름을 설정한 뒤 생성 클릭 ( User##AngularRepo )

<div align="left"><figure><img src="../../../.gitbook/assets/image (738).png" alt="" width="563"><figcaption></figcaption></figure></div>

4. 생성한 리포지토리를 클릭하고, 하단의 리포지토리 복제에서 복사버튼을 클릭, 메모장에 저장

<div align="left"><figure><img src="../../../.gitbook/assets/image (739).png" alt="" width="563"><figcaption></figcaption></figure></div>

5. Cloud9으로 돌아가 제공받은 my-angular-project.zip 파일을 /home/ec2-user/environment 디렉토리로 드래그 앤 드랍 하여 업로드

<div align="left"><figure><img src="../../../.gitbook/assets/image (740).png" alt="" width="375"><figcaption></figcaption></figure></div>

6. 압축 해제 후 디렉토리 이동

```
unzip my-angular-project.zip 
cd my-angular-project
```

<div align="left"><figure><img src="../../../.gitbook/assets/image (741).png" alt="" width="563"><figcaption></figcaption></figure></div>

7. 리모트 연결

```
git remote add origin <4단계에서 복사해뒀던 주소>
```

<figure><img src="../../../.gitbook/assets/image (742).png" alt=""><figcaption></figcaption></figure>

8. 푸시 명령어로 소스코드 업로드

```
git push origin master
```

<div align="left"><figure><img src="../../../.gitbook/assets/image (743).png" alt=""><figcaption></figcaption></figure></div>

9. CodeCommit 서비스로 이동하여 푸시된 코드 확인

<div align="left"><figure><img src="../../../.gitbook/assets/image (744).png" alt="" width="132"><figcaption></figcaption></figure></div>

10. S3 서비스로 이동하여 버킷 생성

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
15. 아래와 같이 이름만 설정 후 다음 클릭 (user##-AngularPipeline)

<div align="left"><figure><img src="../../../.gitbook/assets/image (748).png" alt="" width="563"><figcaption></figcaption></figure></div>

16. 아래와 같이 설정 후 다음 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (749).png" alt="" width="563"><figcaption></figcaption></figure></div>

17. 아래와 같이 설정 후 프로젝트 생성 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (750).png" alt="" width="563"><figcaption></figcaption></figure></div>

18. 프로젝트 이름에는 user##-AngularBuild 을 입력하고, 운영체제 ubuntu, 런타임 Standard, 이미지 aws/codebuild/standard:5.0 을 선택하고 하단 Codepipeline 으로 계속 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (751).png" alt=""><figcaption></figcaption></figure></div>

19. 그러면 프로젝트가 추가가 되고, 다음 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (752).png" alt="" width="563"><figcaption></figcaption></figure></div>

20. 아래와 같이 설정 후 다음 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (753).png" alt="" width="563"><figcaption></figcaption></figure></div>

21. 파이프라인 생성 클릭 (시간 소요)

<div align="left"><figure><img src="../../../.gitbook/assets/image (754).png" alt="" width="290"><figcaption></figcaption></figure></div>

22. IAM 으로 이동하여 아래 역할(파이프라인 생성시 자동생성되는역할) 선택, 권한추가, 정책연결 클릭

{% code overflow="wrap" %}
```
AWSCodePipelineServiceRole-ap-northeast-2-user##-Angular
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (990).png" alt=""><figcaption></figcaption></figure>

23. 아래 정책을 검색하여 연결

{% code overflow="wrap" %}
```
AWSCodeBuildAdminAccess
AWSCodeBuildDeveloperAccess
AWSCodeDeployFullAccess
AmazonSNSFullAccess
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (991).png" alt=""><figcaption></figcaption></figure>

24. Source 단계는 성공, 그러나 Build 에서 실패, CodeBuild 에서 보기를 클릭하여 로그 확인

<figure><img src="../../../.gitbook/assets/image (755).png" alt=""><figcaption></figcaption></figure>

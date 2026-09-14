# Task 1 - S3, 정적 웹사이트 호스팅

## 실습 소개

S3 버킷에 웹 페이지 소스를 업로드하여 정적 웹 사이트 호스팅을 구성해보고, CodePipeline을 통해 배포해봅니다.

1. AWS 에 로그인
2. 상단 검색창에 S3를 검색하여 S3 서비스로 이동

<div align="left"><figure><img src="../../../.gitbook/assets/image (605).png" alt="" width="563"><figcaption></figcaption></figure></div>

3. 버킷 만들기를 클릭

<figure><img src="../../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

4. 아래와 같이 설정 한뒤, 하단으로 스크롤하여 버킷 만들기 클릭

* 버킷 네임스페이스 : 계정 리전 네임스페이스(권장)
* 버킷이름 접두사 : user##-website-source
* 버킷 버전 관리 : 활성화

<figure><img src="../../../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

<div align="left"><figure><img src="../../../.gitbook/assets/image (982).png" alt="" width="287"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../../.gitbook/assets/image (608).png" alt="" width="160"><figcaption></figcaption></figure></div>

5\. 앞 단계를 참고하여 아래 조건으로 버킷을 하나 더 생성

{% code overflow="wrap" %}
```
- 버킷 네임스페이스 : 계정 리전 네임스페이스(권장)
- 버킷 이름 접두사: user##-website-prod
- 객체 소유권 : ACL 활성화 됨 선택
- 이 버킷의 퍼블릭 액세스 차단 설정 : 모든 퍼블릭 액세스 차단 체크 해제
- 현재 설정으로 인해 이 버킷과 그 안에 포함된 객체가 퍼블릭 상태가 될 수 있음을 알고 있습니다 : 동의 체크 
- 버킷 버전 관리 : 비활성화
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

<div align="left"><figure><img src="../../../.gitbook/assets/image (609).png" alt=""><figcaption></figcaption></figure></div>

<figure><img src="../../../.gitbook/assets/image (610).png" alt=""><figcaption></figcaption></figure>

<div align="left"><figure><img src="../../../.gitbook/assets/image (611).png" alt="" width="333"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../../.gitbook/assets/image (608).png" alt="" width="239"><figcaption></figcaption></figure></div>

6. 버킷 목록 중 user##-website-prod 클릭하고 속성 탭 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (612).png" alt="" width="563"><figcaption></figcaption></figure></div>

7. 하단으로 스크롤 하여 정적 웹사이트 호스팅 편집 클릭

<figure><img src="../../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

8. 활성화 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (614).png" alt="" width="332"><figcaption></figcaption></figure></div>

9. 인덱스 문서에 index.html 오류 문서에 error.html 을 입력하고 변경사항 저장 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (615).png" alt="" width="563"><figcaption></figcaption></figure></div>

10. 최하단으로 스크롤하여 버킷 웹사이트 엔드포인트 url을 클릭

<figure><img src="../../../.gitbook/assets/image (616).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
아직 소스가 되는 index.html 파일이 없기때문에 403 에러가 발생합니다.
{% endhint %}

11. 버킷 목록으로 돌아가, user##-website-source 버킷을 클릭, 업로드를 클릭, 제공받은 my-website.zip을 업로드

<div align="left"><figure><img src="../../../.gitbook/assets/image (617).png" alt="" width="563"><figcaption></figcaption></figure></div>

12. AWS 서비스 검색창에 CodePipeline을 검색하고 해당 서비스로 이동 한 뒤, 파이프라인 생성 클릭

<figure><img src="../../../.gitbook/assets/image (618).png" alt=""><figcaption></figcaption></figure>

13. 생성옵션에 사용자 지정 파이프라인 빌드를 선택 후 다음 클릭,\
    파이프라인 이름에 user##-web-pipeline 을 입력 후 다음 클릭

<figure><img src="../../../.gitbook/assets/image (956).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (957).png" alt=""><figcaption></figcaption></figure>

14\. 소스 공급자에 Amazon S3 선택, 버킷에 user##-website-source 선택,

S3 객체 키에 my-website.zip 입력 후 다음 클릭

<figure><img src="../../../.gitbook/assets/image (958).png" alt=""><figcaption></figcaption></figure>

15. 빌드 스테이지와 테스트 스테이지는 둘다 건너뛰기

<figure><img src="../../../.gitbook/assets/image (959).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (960).png" alt=""><figcaption></figcaption></figure>

16. 배포 공급자에 Amazon S3 선택 후 버킷에 user##-website-prod 선택, 배포하기 전에 파일 압축 풀기선택

<figure><img src="../../../.gitbook/assets/image (961).png" alt=""><figcaption></figcaption></figure>

17. 하단 추가 구성 확장 후 표준 ACL에 public-read 을 선택 후 다음 클릭, 최종 검토 후 파이프라인 생성 클릭

<figure><img src="../../../.gitbook/assets/image (962).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (963).png" alt=""><figcaption></figcaption></figure>

18\. 2\~5분 후 파이프라인 인터페이스에서 Source단계와 Deploy 단계가 성공으로 출력됨을 확인하고

다시 10번 단계에서 확인했던 URL을 새로고침

<figure><img src="../../../.gitbook/assets/image (623).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (624).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (625).png" alt=""><figcaption></figcaption></figure>

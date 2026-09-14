---
hidden: true
---

# Task 3 - 배포 단계 추가 (test)

실습 소개

새 리전에 S3 버킷을 추가하여 웹 페이지 소스를 업로드 한 뒤, 기존 파이프라인의 배포 단계에 추가해봅니다.



1. 도쿄리전으로 이동

<div align="left"><figure><img src="../../../.gitbook/assets/image (634).png" alt="" width="389"><figcaption></figcaption></figure></div>

2. S3로 이동하여 버킷을 새로 생성

버킷 이름 : user##-website-prod-jp

<figure><img src="../../../.gitbook/assets/image (964).png" alt=""><figcaption></figcaption></figure>





2. 객체소유권에 ACL 활성화됨 선택

<div align="left"><figure><img src="../../../.gitbook/assets/image (636).png" alt="" width="563"><figcaption></figcaption></figure></div>



3. 모든 퍼블릭 액세스 차단 해제, 아래 박스 체크 그리고, 버킷 만들기 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (637).png" alt="" width="563"><figcaption></figcaption></figure></div>



4. 버킷 목록 중  user##-website-prod-jp 버킷을 클릭, 권한 탭 클릭, 버킷정책 편집 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (638).png" alt="" width="512"><figcaption></figcaption></figure></div>

<figure><img src="../../../.gitbook/assets/image (639).png" alt=""><figcaption></figcaption></figure>



5. 정책 입력 란에 아래 코드 입력

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
			"Resource": "arn:aws:s3:::<버킷명>/*"
		}
	]
}
```



6. 정책입력란 위에 확인되는 버킷명을 정책 코드에 입력한뒤 변경사항 저장 클릭

<figure><img src="../../../.gitbook/assets/image (966).png" alt=""><figcaption></figcaption></figure>

<div align="left"><figure><img src="../../../.gitbook/assets/image (641).png" alt="" width="335"><figcaption></figcaption></figure></div>



7. 버킷의 속성 탭 클릭, 최 하단으로 스크롤하여 정적 웹사이트 호스팅 편집 클릭

<figure><img src="../../../.gitbook/assets/image (967).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (642).png" alt=""><figcaption></figcaption></figure>



5. 정적 웹사이트 호스팅 활성화 선택 후 인덱스 문서를 아래와 같이 입력한 뒤, 변경사항 저장 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (643).png" alt="" width="563"><figcaption></figcaption></figure></div>



6. 최 하단의 버킷 웹사이트 엔드포인트 클릭, 403 에러 확인

<figure><img src="../../../.gitbook/assets/image (644).png" alt=""><figcaption></figcaption></figure>



7. CodePipeline으로 이동하여 편집 클릭 (다시 서울리전!)

<figure><img src="../../../.gitbook/assets/image (645).png" alt=""><figcaption></figcaption></figure>





8. Deploy 단계에서 스테이지 편집 클릭

<figure><img src="../../../.gitbook/assets/image (646).png" alt=""><figcaption></figcaption></figure>



9. 작업 추가 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (647).png" alt="" width="375"><figcaption></figcaption></figure></div>



10. 아래 스크린샷과 같이 설정

* 작업공급자는 배포의 S3를 선택

<figure><img src="../../../.gitbook/assets/image (969).png" alt=""><figcaption></figcaption></figure>



11. 추가구성을 확장하고, 표준 ACL에 public-read 선택 후 완료 클릭

<figure><img src="../../../.gitbook/assets/image (970).png" alt=""><figcaption></figcaption></figure>



12. Deploy 단계에서 추가 된 것을 확인하고 완료 클릭, 상단의 저장 클릭, 한번 더 저장 클릭

<figure><img src="../../../.gitbook/assets/image (971).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (972).png" alt=""><figcaption></figcaption></figure>





13. 기존 단계들은 성공이지만, 추가한 단계는 실행되지 않음을 확인한 뒤 우측 상단 변경사항 릴리스 클릭, 릴리스 클릭

<figure><img src="../../../.gitbook/assets/image (651).png" alt=""><figcaption></figcaption></figure>



14. 오류가 난 단계를 클릭, 도쿄리전의 버킷명을 복사

<figure><img src="../../../.gitbook/assets/image (973).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (974).png" alt=""><figcaption></figcaption></figure>





15. IAM -> 역할 이동

<figure><img src="../../../.gitbook/assets/image (975).png" alt=""><figcaption></figcaption></figure>





16. 아래 역할 검색 후 클릭

{% code overflow="wrap" %}
```
AWSCodePipelineServiceRole-ap-northeast-2-user##-web-pipeline
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (976).png" alt=""><figcaption></figcaption></figure>





17. AWS 로시작하는정책 클릭

<figure><img src="../../../.gitbook/assets/image (977).png" alt=""><figcaption></figcaption></figure>





18. 편집을 클릭

<figure><img src="../../../.gitbook/assets/image (978).png" alt=""><figcaption></figcaption></figure>





19. Resource 내 s3 버킷을 이전(14번)에 복사했던 버킷명 추가(쉼표 추가 중요!)

<figure><img src="../../../.gitbook/assets/image (979).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (980).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (981).png" alt=""><figcaption></figcaption></figure>



20.





16. 단계도 성공된 것을 확인한 뒤, 6번단계에서 확인했던 홈페이지 새로고침

<figure><img src="../../../.gitbook/assets/image (652).png" alt=""><figcaption></figcaption></figure>

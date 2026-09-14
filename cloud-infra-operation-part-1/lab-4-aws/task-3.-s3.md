# Task 3. S3 정적 웹사이트 호스팅

1\. AWS 관리 콘솔 상단 검색창에서 S3를 검색 후 선택합니다.

<figure><img src="../../.gitbook/assets/image (299).png" alt=""><figcaption></figcaption></figure>



2\. 좌측 범용 버킷 클릭, 우측 버킷 만들기 클릭

<figure><img src="../../.gitbook/assets/image (300).png" alt=""><figcaption></figcaption></figure>





3\. 계정 리전 네임스페이스를 선택하고, 버킷이름에 user\*\*-bucket 을 입력

<figure><img src="../../.gitbook/assets/image (500).png" alt=""><figcaption></figcaption></figure>





4. 객체 소유권은 ACL 비활성화됨 선택

<figure><img src="../../.gitbook/assets/image (501).png" alt=""><figcaption></figcaption></figure>





5\. 버킷의 퍼블릭 액세스 차단 설정을 모두 해제하여 퍼블릭 액세스를 허용한 뒤 버킷 만들기 클릭

<figure><img src="../../.gitbook/assets/image (302).png" alt="" width="563"><figcaption></figcaption></figure>

{% hint style="info" %}
버킷의 기본 설정은 모든 퍼블릭 액세스 차단입니다. 기본적으로 체크 되어 있는 모든 퍼블릭 액세스 차단을 풀어준 후 경고창에 있는 체크박스에 "체크"를 하여 퍼블릭 액세스 차단을 비활성화합니다.
{% endhint %}



![](<../../.gitbook/assets/image (303).png>)&#x20;







6. S3 콘솔의  객체 탭에서 업로드 버튼을 클릭

<figure><img src="../../.gitbook/assets/image (305).png" alt=""><figcaption></figcaption></figure>



7. 메모장을 열고 아래 코드를 입력하여 저장합니다.(파일이름 : index.html)

```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8"/>
        <title>
            S3 Website Hosting Example
        </title>
    </head>
    <body>
        <div>
            Hello, World!
        </div>
    </body>
</html>
```





8. s3 버킷 파일 업로드 창에 위 메모장 파일을 드래그 앤 드랍

<figure><img src="../../.gitbook/assets/image (306).png" alt=""><figcaption></figcaption></figure>





9. 하단 업로드 클릭

<figure><img src="../../.gitbook/assets/image (307).png" alt=""><figcaption></figcaption></figure>



10. 다시 버킷 화면으로 이동하여 속성 탭 클릭

<figure><img src="../../.gitbook/assets/image (502).png" alt=""><figcaption></figcaption></figure>





11. 맨 아래 정적 웹사이트 호스팅  우측의 편집 클릭

<figure><img src="../../.gitbook/assets/image (309).png" alt=""><figcaption></figcaption></figure>



12. 활성화 클릭, 인덱스 문서에 index.html을 입력

<figure><img src="../../.gitbook/assets/image (310).png" alt=""><figcaption></figcaption></figure>





13. 아래 변경사항 저장 클릭

<figure><img src="../../.gitbook/assets/image (311).png" alt=""><figcaption></figcaption></figure>



14. 버킷의 속성 탭 클릭, ARN 주소를 복사하여 메모장에 저장

<figure><img src="../../.gitbook/assets/image (503).png" alt=""><figcaption></figcaption></figure>





15. 아래 웹 기반 정책 생성기 사이트에 접속

{% embed url="https://awspolicygen.s3.amazonaws.com/policygen.html" %}



16. 정책 생성기에서 다음과 같이 설정

* Type of Policy : S3 Bucket Policy
* Effect : Allow
* Principal : \*
* Actions : GetObject

<figure><img src="../../.gitbook/assets/image (315).png" alt=""><figcaption></figcaption></figure>



17. 아래 ARN 에 복사했던 ARN을 붙여넣고 하단 Add statement 버튼을 클릭

(Statements added 에 정책이 만들어집니다.)

<figure><img src="../../.gitbook/assets/image (316).png" alt=""><figcaption></figcaption></figure>





18. Generate Policy 버튼을 클릭하여 정책을 생성

<figure><img src="../../.gitbook/assets/image (317).png" alt="" width="563"><figcaption></figcaption></figure>





19 . JSON 형식으로 생성된 정책을 복사(Copy)

<figure><img src="../../.gitbook/assets/image (318).png" alt="" width="375"><figcaption></figcaption></figure>





20. 권한 탭 클릭, 버킷정책 편집 클릭

<figure><img src="../../.gitbook/assets/image (313).png" alt=""><figcaption></figcaption></figure>



21. 복사한 정책 Json 을 붙여넣기

<figure><img src="../../.gitbook/assets/image (262).png" alt=""><figcaption></figcaption></figure>



22. 정책에서 리소스 /\*을 추가하여 해당 버킷 안에 모든 파일에 접근하도록 수정하고 적용

<figure><img src="../../.gitbook/assets/image (263).png" alt=""><figcaption></figcaption></figure>



23. 아래 변경 사항 저장 클릭

<figure><img src="../../.gitbook/assets/image (321).png" alt=""><figcaption></figcaption></figure>



24. 버킷의 속성 탭으로 이동한 뒤, 맨 하단 버킷 웹사이트 엔드 포인트 링크를 복사하여 접속&#x20;

<figure><img src="../../.gitbook/assets/image (322).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (264).png" alt=""><figcaption></figcaption></figure>





25. 브라우저로 접속 (아래와 같이 hello 메시지를 확인했다면 성공!!)

<figure><img src="../../.gitbook/assets/image (265).png" alt=""><figcaption></figcaption></figure>

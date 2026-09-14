# Task 5 - API Gateway

1. API Gateway 서비스로 이동

<figure><img src="../../.gitbook/assets/image (533).png" alt=""><figcaption></figcaption></figure>





2. API 클릭, API 생성 클릭

<figure><img src="../../.gitbook/assets/image (534).png" alt=""><figcaption></figcaption></figure>





3. REST API 구축 클릭

<figure><img src="../../.gitbook/assets/image (535).png" alt=""><figcaption></figcaption></figure>





4. 아래와 같이 설정 후 API 생성 클릭

* 새 API 선택
* API 이름 : user\*\*-WildRydes
* API 엔드포인트 유형 : 엣지 최적화

<figure><img src="../../.gitbook/assets/image (536).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (537).png" alt="" width="375"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (538).png" alt="" width="275"><figcaption></figcaption></figure>





5. 권한 부여자 생성 클릭

<figure><img src="../../.gitbook/assets/image (539).png" alt=""><figcaption></figcaption></figure>





6. 아래와 같이 설정 후 권한 부여자 생성 클릭

* 권한 부여자 이름 : user\*\*-WildRydes
* 권한 부여자 유형 : Conito
* Conito 사용자 풀 : ap-northeast-2 선택, Lab1 > Task3 에서 메모장에 저장해둔 "사용자 풀 이름" 선택
* 토큰 소스 : Authorization

<figure><img src="../../.gitbook/assets/image (540).png" alt=""><figcaption></figcaption></figure>



7. 이전 웹페이지로 접속하여 하위 주소로 /ride.html 입력후 접속하여 아래 토큰 값을 복사

<figure><img src="../../.gitbook/assets/image (541).png" alt=""><figcaption></figcaption></figure>





8. 생성한 권한 부여자 클릭

<figure><img src="../../.gitbook/assets/image (542).png" alt=""><figcaption></figcaption></figure>





9. 하단 토큰 값에 복사한 토큰 값 붙여넣고, 권한 부여자 테스트 클릭

<figure><img src="../../.gitbook/assets/image (543).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (544).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
200 코드와 클레임 메시지 확인
{% endhint %}



10. 리소스 생성 클릭

<figure><img src="../../.gitbook/assets/image (545).png" alt=""><figcaption></figcaption></figure>





11. 리소스 이름에 user\*\*-ride 입력, 오리진 간 리소스 공유 체크 한 뒤 리소스 생성 클릭

<figure><img src="../../.gitbook/assets/image (547).png" alt=""><figcaption></figcaption></figure>





12. 생성한 리소스이름을 선택하고 메서드 생성 클릭

<figure><img src="../../.gitbook/assets/image (548).png" alt=""><figcaption></figcaption></figure>





13. 아래와 같이 설정 후 메서드 생성 클릭

* 메서드 유형 : POST
* 통합 유형 : Lambda 함수
* Lambda 프록시 통합 활성화&#x20;
* Lambda 함수에 출력되는 user\*\*-RequestUnicorn

<figure><img src="../../.gitbook/assets/image (549).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (550).png" alt=""><figcaption></figcaption></figure>



14. POST 클릭, 메서드 요청 탭 클릭, 편집 클릭

<figure><img src="../../.gitbook/assets/image (551).png" alt=""><figcaption></figcaption></figure>





15. 메서드 요청 설정의 권한 부여에 user\*\*-WildRydes 선택 한 뒤 저장 클릭



<figure><img src="../../.gitbook/assets/image (552).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (553).png" alt=""><figcaption></figcaption></figure>





16. 우측 상단 API 배포 클릭

<figure><img src="../../.gitbook/assets/image (554).png" alt=""><figcaption></figcaption></figure>







17. 스테이지는 새 스테이지를 선택하고, 스테이지 이름에 user\*\*-prod 를 입력한 뒤 배포 클릭

<figure><img src="../../.gitbook/assets/image (555).png" alt=""><figcaption></figcaption></figure>







18. URL 호출 주소를 메모장에 저장

<figure><img src="../../.gitbook/assets/image (556).png" alt=""><figcaption></figcaption></figure>





19. CloudShell 에서 URL 호출 주소를 변수에 등록

{% code overflow="wrap" %}
```
export INVOKE_URL='위에서 저장한 주소'
```
{% endcode %}



20. 명령으로 config.js 파일 수정

{% code overflow="wrap" %}
```
sed -i "s|invokeUrl: ''|invokeUrl: '$INVOKE_URL'|g" js/config.js
```
{% endcode %}





21. 수정 내용 확인

{% code overflow="wrap" %}
```
cat js/config.js
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (557).png" alt=""><figcaption></figcaption></figure>



22. 코드 수정 (지도정보 트러블슈팅)

{% code overflow="wrap" %}
```
sed -i "s/basemap: 'gray-vector'/basemap: 'streets'/g" js/esri-map.js
```
{% endcode %}

```
sed -i "s|invokeUrl + '/ride'|invokeUrl + '/user**-ride'|g" js/ride.js
```



22. CodeCommit 에 푸시

{% code overflow="wrap" %}
```
git add js/config.js
git add js/esri-map.js
git add js/ride.js
git commit -m "last commit"
git push
```
{% endcode %}



23. Amplify 콘솔에서 배포가 완료된것을 확인

<figure><img src="../../.gitbook/assets/image (558).png" alt=""><figcaption></figcaption></figure>





24. 아래 주소로 접속하여 아무 장소를 클릭 한 뒤 Request Unicon 클릭

```
<위 도메인주소>/ride.html
```

유니콘이 날라오는 액션과 성공 안내 메시지 확인

<figure><img src="../../.gitbook/assets/image (198).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (199).png" alt=""><figcaption></figcaption></figure>

# Task 3 - 사용자 인증 및 등록

{% hint style="success" %}
실습에 사용되는 서비스&#x20;

Cognito : 웹 및 모바일 앱에 회원가입, 로그인, 접근 제어(인증 및 권한 부여) 기능을 쉽고 안전하게 추가할 수 있는 서비스
{% endhint %}



1. Cognito 서비스로 이동

<figure><img src="../../.gitbook/assets/image (247).png" alt=""><figcaption></figcaption></figure>





2. 사용자 풀 클릭

<figure><img src="../../.gitbook/assets/image (248).png" alt=""><figcaption></figcaption></figure>



3. 사용자 풀 생성 클릭

<figure><img src="../../.gitbook/assets/image (249).png" alt=""><figcaption></figcaption></figure>





4. 아래와 같이 설정 한 뒤 사용자 디렉토리 생성 클릭

* 애플리케이션 유형 : 단일 페이지 애플리케이션(SPA)
* 애플리케이션 이름 지정 : user\*\*-WildRydesWebApp
* 로그인 식별자에 대한 옵션 : 이메일 체크

<figure><img src="../../.gitbook/assets/image (202).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../.gitbook/assets/image (251).png" alt=""><figcaption></figcaption></figure>



6. 생성 완료시 사용자 풀 이름이 출력되는데, 복사하여 메모장에 저장

<figure><img src="../../.gitbook/assets/image (253).png" alt=""><figcaption></figcaption></figure>





7. 좌측 사용자 풀 클릭, 해당하는 사용자 풀 이름 클릭

<figure><img src="../../.gitbook/assets/image (254).png" alt=""><figcaption></figcaption></figure>





8. 사용자 풀 ID를 메모장에 저장

<figure><img src="../../.gitbook/assets/image (255).png" alt=""><figcaption></figcaption></figure>





9. 좌측 앱 클라이언트 클릭, 클라이언트 ID도 메모장에 저장

<figure><img src="../../.gitbook/assets/image (256).png" alt=""><figcaption></figcaption></figure>





10. 변수로 두 값을 지정

{% code overflow="wrap" %}
```
USER_POOL_ID='메모장에 저장한 사용자 풀 ID'
CLIENT_ID='메모장에 저장한 클라이언트 ID'
REGION='ap-northeast-2'
```
{% endcode %}





11. 디렉토리 이동

{% code overflow="wrap" %}
```
cd ~/user**-wildrydes
```
{% endcode %}



12. 코드 수정

{% code overflow="wrap" %}
```
sed -i "s/userPoolId: ''/userPoolId: '$USER_POOL_ID'/g" js/config.js
sed -i "s/userPoolClientId: ''/userPoolClientId: '$CLIENT_ID'/g" js/config.js
sed -i "s/region: ''/region: '$REGION'/g" js/config.js
```
{% endcode %}





13. 수정된 코드 확인

{% code overflow="wrap" %}
```
cat js/config.js
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (261).png" alt=""><figcaption></figcaption></figure>



14. 코드 푸시

{% code overflow="wrap" %}
```
git add js/config.js
git commit -m "configure cognito"
git push
```
{% endcode %}



15. 다시 Amplify의 배포 화면에서 배포가 완료되면 사이트 접속 후 GIDDY UP 클릭

<figure><img src="../../.gitbook/assets/image (200).png" alt=""><figcaption></figcaption></figure>





16. 실제 개인 이메일 주소를 입력하고 패스워드를 입력한 뒤 LET'S RYDE 클릭

<figure><img src="../../.gitbook/assets/image (201).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
암호 설정 시 최소 하나 이상의 대문자와 숫자, 특수문자를 포함해야 합니다.
{% endhint %}





17. 계정 생성 알림 화면

<figure><img src="../../.gitbook/assets/image (203).png" alt=""><figcaption></figcaption></figure>





18. 해당 이메일의 받은편지함에서 확인 코드를 확인하고 인증 수행

<figure><img src="../../.gitbook/assets/image (204).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (205).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (206).png" alt=""><figcaption></figcaption></figure>





19. 로그인 성공 화면

<figure><img src="../../.gitbook/assets/image (207).png" alt=""><figcaption></figcaption></figure>

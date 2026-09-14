# Task 1 - Hello world

1. 람다 서비스로 이동

<figure><img src="../../.gitbook/assets/image (208).png" alt=""><figcaption></figcaption></figure>

2. 함수 생성 클릭

<figure><img src="../../.gitbook/assets/image (210).png" alt=""><figcaption></figcaption></figure>

3. 다음과 같이 설정

* 블루프린트 사용
* 블루프린트 이름 : Hello world function (python3.12)
* 함수 이름 : user\*\*-fn
* 실행 역할 : 기본 역할 사용

<figure><img src="../../.gitbook/assets/image (212).png" alt=""><figcaption></figcaption></figure>

4. 함수 생성 클릭

<figure><img src="../../.gitbook/assets/image (213).png" alt=""><figcaption></figcaption></figure>

5. 테스트 탭 클릭 후 아래와 같이 설정

* 이벤트 작업 테스트 : 새 이벤트 생성
* 이벤트 이름 : user\*\*-fn-test-event
* 이벤트 공유 설정 : 프라이빗
* 템플릿 : Hello world

<figure><img src="../../.gitbook/assets/image (214).png" alt=""><figcaption></figcaption></figure>

6. 아래 이벤트 JSON 편집기에서 key1 에 해당하는 value1 을 아래와같이 수정

{% code overflow="wrap" %}
```
"hello, world"
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (215).png" alt=""><figcaption></figcaption></figure>

7. 우측 상단 테스트 클릭 후 함수 실행 알림 확인

<figure><img src="../../.gitbook/assets/image (216).png" alt=""><figcaption></figcaption></figure>

8. 실행 알림에서 세부정보를 확장하여 아래 로그 확인

<figure><img src="../../.gitbook/assets/image (217).png" alt=""><figcaption></figcaption></figure>

9. 7번을 참고하여 테스트를 몇회 더 시도
10. 모니터링 탭 클릭하여 현지 시간대로 수정하고, 아래 호출, 기간등 그래프 확인

{% hint style="info" %}
데이터 수집에 1\~2분 소요될수 있습니다.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (218).png" alt=""><figcaption></figcaption></figure>

11. 확인이 끝났다면 함수 삭제

<figure><img src="../../.gitbook/assets/image (219).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (220).png" alt=""><figcaption></figcaption></figure>

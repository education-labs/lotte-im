# Task 2 - Cloudwatch 대시보드 구성

1. Cloudwatch 에서 대시보드 > 대시보드 생성 클릭

<figure><img src="../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>



2. 대시보드 이름에는 user\*\*-dashboard 를 입력

<figure><img src="../../.gitbook/assets/image (18).png" alt="" width="363"><figcaption></figcaption></figure>



3. 지표 > 행 > 다음 클릭

{% hint style="info" %}
지표 기반, 로그 기반의 위젯을 추가할 수 있습니다.

다양한 유형의 위젯을 사용할 수 있습니다.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (19).png" alt="" width="563"><figcaption></figcaption></figure>





4. EC2 > 인스턴스별 지표 > 인스턴스 ID 입력 > CPU Utilization 을 선택한 뒤, 위젯 생성 클릭

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (21).png" alt="" width="254"><figcaption></figcaption></figure>





5. 나만의 대시보드에 위젯하나가 추가됨을 확인

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
해당 인스턴스의 Network In/Out 등 몇가지 지표를 더 위젯으로 구성 해봅니다.
{% endhint %}


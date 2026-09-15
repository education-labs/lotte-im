# Task 1 - EC2 모니터링

1. 실행 되고 있는 인스턴스를 선택한 뒤 모니터링 탭 클릭, 세부 모니터링 관리 클릭

<figure><img src="../../.gitbook/assets/image (7) (1) (1).png" alt=""><figcaption></figcaption></figure>

2. 인스턴스 ID를 메모장에 저장한 뒤 세부모니터링 활성화 체크 후 확인 클릭

<figure><img src="../../.gitbook/assets/image (8) (1).png" alt="" width="375"><figcaption></figcaption></figure>

3. 모니터링 탭으로 이동하여 그래프 확인

<figure><img src="../../.gitbook/assets/image (9) (1).png" alt="" width="307"><figcaption></figcaption></figure>

4. 확장 버튼을 클릭하면 확대 그래프를 확인 가능

<figure><img src="../../.gitbook/assets/image (10) (1).png" alt=""><figcaption></figcaption></figure>

5. Cloudwatch 서비스로 이동

<figure><img src="../../.gitbook/assets/image (11) (1).png" alt=""><figcaption></figcaption></figure>

6. 모든 지표 클릭

<figure><img src="../../.gitbook/assets/image (12) (1).png" alt="" width="143"><figcaption></figcaption></figure>

7. 찾아보기 > EC2 클릭

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

8. 인스턴스별 지표 클릭

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

9. 메모장에 저장해놨던 인스턴스ID를 검색창에서 검색

<figure><img src="../../.gitbook/assets/image (15).png" alt="" width="563"><figcaption></figcaption></figure>

10. CPUUtilization 지표를 체크하고 상단의 그래프 출력 확인

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
CPU Utilization: 현재 인스턴스에서 사용하고 있는 컴퓨팅 파워\
Disk Read Ops/Disk Write Ops: 해당 인스턴스에 연결된 모든 로컬 디스크에서 읽은/쓴 오퍼레이션의 수\
Network In/Out: 인스턴스의 네트워크 인터페이스를 통해 들어온/나간 바이트 량\
Status Check Failed: 상태 검사 실패 여부
{% endhint %}

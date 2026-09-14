# Task 7 - Adding a Manual Approval Action to Your Pipeline

실습 소개

Amazon SNS 서비스를 통해 파이프라인에 수동 승인 단계를 추가해봅니다.



1. sns 서비스로 이동

<div align="left"><figure><img src="../../../.gitbook/assets/image (929).png" alt="" width="563"><figcaption></figcaption></figure></div>



2. 주제이름에 user##PipelineNotifications 를 입력하고 다음단계 클릭, 표시 이름에도 동일한 이름 입력 그리고, 주제 생성 클릭

<figure><img src="../../../.gitbook/assets/image (930).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
이때 유형은 표준으로 선택합니다.
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (992).png" alt=""><figcaption></figcaption></figure>



3. 구독생성 클릭한 뒤, 프로토콜을 이메일로 선택, 엔드포인트에 사용가능한 이메일 주소 입력, 구독생성 클릭

<figure><img src="../../../.gitbook/assets/image (931).png" alt=""><figcaption></figcaption></figure>



4. 3에서 입력한 이메일을 확인하고 아래와 같은 메일이 수신되면 Confirm subscription 클릭

우측 하단 aws 창이 출력됨

<figure><img src="../../../.gitbook/assets/image (932).png" alt=""><figcaption></figcaption></figure>



5. 다시 sns 구독에서 새로고침을 하면 상태가 확인됨으로 변경된 것을 확인

<figure><img src="../../../.gitbook/assets/image (933).png" alt=""><figcaption></figcaption></figure>



6. 파이프라인으로 돌아가 편집 클릭, Build 과정의 스테이지 편집클릭

<figure><img src="../../../.gitbook/assets/image (934).png" alt=""><figcaption><p><br></p></figcaption></figure>

7. 하단의 작업그룹추가 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (935).png" alt="" width="375"><figcaption></figcaption></figure></div>



8. 작업이름에 ManualApproval 입력, 작업 공급자에 수동승인 선택, SNS 주제에 이전에 생성했던 SNS 선택, 설명란에 "파이프라인 배포단계 전 승인을 검토하세요." 입력 후 완료 클릭

<figure><img src="../../../.gitbook/assets/image (936).png" alt=""><figcaption></figcaption></figure>



9. 완료 클릭, 상단에 저장 클릭

<figure><img src="../../../.gitbook/assets/image (937).png" alt=""><figcaption></figcaption></figure>



10. Cloud9 으로 이동하여 소스코드 변경

my-angular-project/app.component.html 파일의 version 을 5.0 으로 수정 및 저장(Ctrl+S)

<figure><img src="../../../.gitbook/assets/image (938).png" alt=""><figcaption></figcaption></figure>



11. Code 커밋, 푸시

```
git commit -a -m "Version 5.0"
git push origin master
```

<div align="left"><figure><img src="../../../.gitbook/assets/image (939).png" alt="" width="563"><figcaption></figcaption></figure></div>



12. 다시 Pipeline으로 돌아가 새로 시작됨을 확인한 뒤 수동승인단계에서, 보류중이 될 때까지 대기

<figure><img src="../../../.gitbook/assets/image (940).png" alt=""><figcaption></figcaption></figure>

13. 이전에 입력했던 이메일함을 확인하여 내용을 확인, 파이프라인으로 돌아와서 수동승을 클릭하여 승인진행

<figure><img src="../../../.gitbook/assets/image (941).png" alt="" width="527"><figcaption></figcaption></figure>



13. 승인이 되면 배포단계가 진행됨을 확인

<figure><img src="../../../.gitbook/assets/image (942).png" alt=""><figcaption></figcaption></figure>



13. 배포가 완료되면 해당 웹페이지에 접속하여 Version 5.0으로 업데이트 됨을 확인

<div align="left"><figure><img src="../../../.gitbook/assets/image (943).png" alt="" width="563"><figcaption></figcaption></figure></div>

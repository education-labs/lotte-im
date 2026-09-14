# Task 8 - Creating Notification Rules​

## 실습 소개

파이프라인에 알림 규칙을 생성해보고 SNS 서비스를 통해 알림 메일을 확인 해봅니다.



1. CodePipeline 에서 편집을 클릭

<figure><img src="../../../.gitbook/assets/image (944).png" alt=""><figcaption></figcaption></figure>



2. 좌측 메뉴의 설정 클릭

<figure><img src="../../../.gitbook/assets/image (945).png" alt=""><figcaption></figcaption></figure>



3. 알림 탭 클릭

<figure><img src="../../../.gitbook/assets/image (946).png" alt=""><figcaption></figcaption></figure>



4. 알림 규칙 생성 클릭

<figure><img src="../../../.gitbook/assets/image (947).png" alt=""><figcaption></figcaption></figure>





5. 알림 이름에 user##PipelineNotificationsRule 를 입력, Pipeline execution 항목을 모두 선택

<div align="left"><figure><img src="../../../.gitbook/assets/image (948).png" alt="" width="563"><figcaption></figcaption></figure></div>



4. 대상 선택 란에 user##을 검색하여 출력되는 대상을 클릭한뒤 submit 버튼 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (949).png" alt="" width="563"><figcaption></figcaption></figure></div>



5. 새 탭을 열고 AWS SNS 서비스로 이동 한 뒤 주제를 클릭, user##을 검색하여 출력되는 주제를 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (950).png" alt="" width="563"><figcaption></figcaption></figure></div>



6. 편집 클릭

<figure><img src="../../../.gitbook/assets/image (951).png" alt=""><figcaption></figcaption></figure>



7\. 액세스 정책을 클릭하여 확장 한 뒤 27 Line 에 쉼표를 추가하고, 28 Line에는 아래 코드를 추가 한뒤 저장 클릭

```
	{
      "Sid": "AWSCodeStarNotifications_publish",
      "Effect": "Allow",
      "Principal": {
        "Service": [
          "codestar-notifications.amazonaws.com"
        ]
      },
      "Action": "SNS:Publish",
      "Resource": "<21Line에있던 ARN>"
    }
```

<figure><img src="../../../.gitbook/assets/image (954).png" alt=""><figcaption></figcaption></figure>



9. 1\~2분 뒤 메일을 확인해보면 아래와 같은 알림 메일 확인 또는 pipeline에서 변경사항 릴리즈로 파이프라인 다시 실행 한 뒤 메일 확인

<div align="left"><figure><img src="../../../.gitbook/assets/image (955).png" alt="" width="563"><figcaption></figcaption></figure></div>

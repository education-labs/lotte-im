# Task 5.  인스턴스에 Role 부여 및 접근 테스트

{% hint style="info" %}
기존 계정에서 실습을 진행합니다.
{% endhint %}



1. S3 서비스로 이동한뒤 버킷 생성 클릭

<figure><img src="../../.gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>



2. 계정 리전 네임스페이스를 선택하고, 버킷 이름 접두사에 iam-role-user\*\* 을 입력한뒤, 하단의 버킷 만들기 클릭

<figure><img src="../../.gitbook/assets/image (56).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (57).png" alt="" width="235"><figcaption></figcaption></figure>



3\. 업로드를 클릭

<figure><img src="../../.gitbook/assets/image (58).png" alt="" width="563"><figcaption></figcaption></figure>



4. 임의의 이미지 파일을 업로드

<figure><img src="../../.gitbook/assets/image (59).png" alt=""><figcaption></figcaption></figure>



5. IAM 서비스로 이동하여 정책 > 정책 생성 클릭&#x20;

<figure><img src="../../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>





6. JSON 을 클릭하고 아래 코드를 정책 편집기에 입력한 뒤 다음 클릭

{% hint style="info" %}
16, 17 Line 의 user## 에는 본인의 유저 넘버를 입력
{% endhint %}

<pre class="language-json" data-line-numbers><code class="lang-json">{
    "Version": "2012-10-17",
    "Statement": [
         {
            "Action": ["s3:ListAllMyBuckets", "s3:GetBucketLocation"],
            "Effect": "Allow",
            "Resource": ["arn:aws:s3:::*"]
         },
         {
            "Effect": "Allow",
            "Action": [
                "s3:Get*",
                "s3:List*"
            ],
             "Resource": [
<strong>                "arn:aws:s3:::iam-role-user##*/*",
</strong><strong>                "arn:aws:s3:::iam-role-user##*"
</strong>            ]
        }
        
    ]
}
</code></pre>

<figure><img src="../../.gitbook/assets/image (570).png" alt="" width="563"><figcaption></figcaption></figure>



<figure><img src="../../.gitbook/assets/image (62).png" alt="" width="198"><figcaption></figcaption></figure>





7. 정책이름에 IAMBucketTestPolicy-user\*\* 를 입력한 뒤, 정책 생성 클릭&#x20;

<figure><img src="../../.gitbook/assets/image (63).png" alt="" width="563"><figcaption></figcaption></figure>





8. 역할 > 역할 생성 클릭

<figure><img src="../../.gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure>





9. AWS 서비스 선택, EC2 선택, EC2 선택 한 뒤, 다음 클릭

<figure><img src="../../.gitbook/assets/image (65).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (66).png" alt="" width="205"><figcaption></figcaption></figure>





10. 권한 정책에 IAMBucketTestPolicy-user\*\* 를 검색 및 선택하고 다음 클릭&#x20;



<figure><img src="../../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>





11. 역할 이름에 IAMBucketTestRole-user\*\* 을 입력한 뒤 역할 생성 클릭

<figure><img src="../../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>



12. EC2 인스턴스 콘솔로 이동하여 prod-instance-user\*\* 로 접속

<figure><img src="../../.gitbook/assets/image (69).png" alt=""><figcaption></figcaption></figure>





13. 아래 명령을 사용하여 s3 버킷 리스트 조회 시도

{% code overflow="wrap" lineNumbers="true" %}
```
aws s3 ls
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>



14. 인스턴스 콘솔에서 prod-instance-user\*\* 을 선택하고 작업 > 보안 > IAM 역할 수정 클릭&#x20;

<figure><img src="../../.gitbook/assets/image (71).png" alt=""><figcaption></figcaption></figure>



15. IAM 역할에 이전에 생성한 IAMBucketTestRole-user\*\* 을 선택하고 IAM 역할 업데이트 클릭

<figure><img src="../../.gitbook/assets/image (72).png" alt=""><figcaption></figcaption></figure>



16. 다시 EC2 인스턴스에 연결한 터미널에서 동일하게 S3 버킷리스트 조회

{% code overflow="wrap" lineNumbers="true" %}
```
aws s3 ls
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (569).png" alt=""><figcaption></figcaption></figure>








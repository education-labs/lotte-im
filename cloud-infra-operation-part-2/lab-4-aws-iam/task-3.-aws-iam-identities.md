# Task 3. AWS IAM Identities 생성

1. IAM 서비스로 이동

<figure><img src="../../.gitbook/assets/image (79).png" alt=""><figcaption></figcaption></figure>





2. 정책, 정책 생성 클릭

<figure><img src="../../.gitbook/assets/image (80).png" alt=""><figcaption></figcaption></figure>







3. JSON 을 클릭하고 코드 편집기에 아래 코드 입력 (user\*\* 에는 본인의 넘버 입력), 다음 클릭

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:*",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "ec2:ResourceTag/Env": "dev-user**"
                    }
                }
            },
        {
            "Effect": "Allow",
            "Action": "ec2:Describe*",
            "Resource": "*"
        },
        {
            "Effect": "Deny",
            "Action": [
                "ec2:DeleteTags",
                "ec2:CreateTags"
                ],
            "Resource": "*"
       }
   ]
}
```

<figure><img src="../../.gitbook/assets/image (82).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (83).png" alt="" width="246"><figcaption></figcaption></figure>

4. 정책이름에 DevPolicy-user\*\* 을 입력한뒤 생성 클릭

<figure><img src="../../.gitbook/assets/image (84).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (85).png" alt=""><figcaption></figcaption></figure>





5. 사용자그룹을 클릭하고 그룹 생성 클릭

<figure><img src="../../.gitbook/assets/image (86).png" alt=""><figcaption></figcaption></figure>



6. 사용자 그룹 이름에 dev-group-user\*\* 을 입력하고, 권한 정책 연결에 DevPolicy-user\*\* 을 검색하여 선택한 뒤, 사용자 그룹 생성 클릭

<figure><img src="../../.gitbook/assets/image (87).png" alt=""><figcaption></figcaption></figure>





7. 사용자 클릭, 사용자 생성 클릭

<figure><img src="../../.gitbook/assets/image (88).png" alt=""><figcaption></figcaption></figure>



8. 사용자 이름에 dev-user\*\* 을 입력하고, AWS 콘솔 접근 권한을 체크한 뒤, IAM 사용자를 선택

<figure><img src="../../.gitbook/assets/image (89).png" alt=""><figcaption></figcaption></figure>



9. 사용자 지정  암호를선택, 암호에 Devuser!234 를 입력하고, 암호 표시를 선택하여 확인한 뒤, 실습의 편의상 다음 로그인시 새 암호로 변경하는 기능을 해제 한뒤, 다음 클릭

<figure><img src="../../.gitbook/assets/image (90).png" alt=""><figcaption></figcaption></figure>



10. 권한 옵션에 그룹에 사용자 추가를 선택하고 방금만든 dev-group-user\*\* 그룹을 선택한 뒤 다음 클릭, 사용자 생성 클릭&#x20;

<figure><img src="../../.gitbook/assets/image (92).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (93).png" alt=""><figcaption></figcaption></figure>





11. 생성이 완료되었으면 기존 사용하던 브라우저와 다른 브라우저를 실행하여 aws 콘솔 로그인 페이지로 이동,&#x20;

dev-user\*\* 으로 로그인

<figure><img src="../../.gitbook/assets/image (94).png" alt="" width="359"><figcaption></figcaption></figure>

# Task 1. 오토스케일링

1. EC2 좌측 메뉴중 오토스케일링 그룹 클릭, 우측의 생성 버튼 클릭

<figure><img src="../../.gitbook/assets/image (122).png" alt=""><figcaption></figcaption></figure>



2. 이름을 user\*\*-asg 라고 입력한 뒤 시작템플릿 생성 클릭

<figure><img src="../../.gitbook/assets/image (123).png" alt="" width="563"><figcaption></figcaption></figure>



3. 이름은 user\*\*-tem, 설명에는 user\*\*-asg-tem 라고 입력

<figure><img src="../../.gitbook/assets/image (124).png" alt="" width="563"><figcaption></figcaption></figure>



4. AMI 에서는 Quick Start 클릭, Amazon Linux 클릭, 아래 AMI 를 Amazon Linux 2023 kernel-6.1 AMI 로선&#x20;

<figure><img src="../../.gitbook/assets/image (125).png" alt="" width="563"><figcaption></figcaption></figure>



5. 인스턴스 유형은 t2.micro, 키페어는 user\*\*-key 선택

<figure><img src="../../.gitbook/assets/image (126).png" alt=""><figcaption></figcaption></figure>





6. 네트워크 설정에 아래와같이 입력

* 서브넷 : 시작 템플릿에 포함하지 않음
* 보안그룹 : 보안그룹 생성&#x20;
* 보안그룹 이름 : user\*\*-asg-sg
* 설명 : user\*\*-asg-sg
* VPC : user\*\*-vpc2q

<figure><img src="../../.gitbook/assets/image (127).png" alt=""><figcaption></figcaption></figure>



7. 인바운드 규칙 추가를 클릭하여 아래 2개의 규칙 추가

1 규칙&#x20;

* 유형 : HTTP
* 소스 유형 : 위치 무관

2 규칙&#x20;

* 유형 : SSH
* 소스 유형 : 위치 무관&#x20;

<figure><img src="../../.gitbook/assets/image (128).png" alt=""><figcaption></figcaption></figure>





8.  아래로 드래그하여 고급 세부정보를 확장하고 아래 사용자 데이터란에 아래 코드를 입력

    <figure><img src="../../.gitbook/assets/image (129).png" alt=""><figcaption></figcaption></figure>

    ```
    #!/bin/bash
    sudo yum update -y
    sudo yum install -y httpd
    sudo echo hello ASG server > /var/www/html/index.html
    sudo systemctl start httpd
    sudo systemctl enable httpd
    ```

<figure><img src="../../.gitbook/assets/image (130).png" alt=""><figcaption></figcaption></figure>



9. 시작 템플릿 생성 클릭

<figure><img src="../../.gitbook/assets/image (131).png" alt=""><figcaption></figcaption></figure>



10. 생성이 완료됬다면, ASG 생성화면으로 돌아와서 새로고침 버튼을 클릭하고 방금 생성한 시작템플릿 선택

<figure><img src="../../.gitbook/assets/image (132).png" alt=""><figcaption></figcaption></figure>



그리고 다음 클릭

<figure><img src="../../.gitbook/assets/image (133).png" alt=""><figcaption></figcaption></figure>



11. 네트워크에서는 기존 생성했던 VPC를 선택하고, 서브넷은 퍼블릭서브넷 2개를 선택

<figure><img src="../../.gitbook/assets/image (134).png" alt="" width="563"><figcaption></figcaption></figure>

그리고 다음 클릭

<figure><img src="../../.gitbook/assets/image (135).png" alt=""><figcaption></figcaption></figure>



12. 로드밸런싱 에서 생성하는 옵션을 선택하고, ALB 선택, 이름은 user\*\*-asg-lb, 로드밸런서 체계는 Internet-facing 선택

<figure><img src="../../.gitbook/assets/image (136).png" alt=""><figcaption></figcaption></figure>



13. 리스너 및 라우팅에서 새 대상 그룹 또는 기존 대상 그룹 선택을 클릭하고 대상 그룹 생성 클릭

<figure><img src="../../.gitbook/assets/image (137).png" alt=""><figcaption></figcaption></figure>



14. 새 대상 그룹 이름은 user\*\*-asg-lb-tg 로 입력, 그리고 다음 클릭

<figure><img src="../../.gitbook/assets/image (138).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (139).png" alt=""><figcaption></figcaption></figure>



15. 아래와 같이 설정&#x20;

* 원하는 용량 : 1
* 원하는 최소 용량 : 1
* 원하는 최대 용량 : 5
* 대상 추적 정책 사용 여부 선택 : 대상 추적 크기 조정 정책&#x20;

<figure><img src="../../.gitbook/assets/image (140).png" alt=""><figcaption></figcaption></figure>



* 지표 유형 : 평균 CPU 사용률
* 대상 값 : 30
* 인스턴스 워밍업 : 10

<figure><img src="../../.gitbook/assets/image (141).png" alt="" width="494"><figcaption></figcaption></figure>

그리고 다음 클릭



<figure><img src="../../.gitbook/assets/image (142).png" alt=""><figcaption></figcaption></figure>





16. 알림과 태그 설정은 추가 설정없이 다음클릭 하고, Auto Scaling 그룹 생성 클릭



<figure><img src="../../.gitbook/assets/image (143).png" alt=""><figcaption></figcaption></figure>



17. EC2 인스턴스 목록화면으로 이동하여 user\*\*-asg 를 검색하고 아래 생성된 인스턴스를 선택한뒤, 퍼블릭 ip주소를 메모장에 저장

<figure><img src="../../.gitbook/assets/image (144).png" alt=""><figcaption></figcaption></figure>



18. 이전 실습을 참고하여 해당 인스턴스에 접속(Powershell)&#x20;

```
cd .\Downloads\
ssh -i user**-key.pem ec2-user@<위에서 복사한 퍼블릭ip>
```



19. 스트레스 툴 설치&#x20;

```
sudo yum install -y stress
```

<figure><img src="../../.gitbook/assets/image (145).png" alt=""><figcaption></figcaption></figure>



20. CPU 부하 생성

```
stress -c 2
```

<figure><img src="../../.gitbook/assets/image (146).png" alt=""><figcaption></figcaption></figure>





21. 약 5분뒤 스케일링 되는 인스턴스&#x20;

<figure><img src="../../.gitbook/assets/image (147).png" alt=""><figcaption></figcaption></figure>


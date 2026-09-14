# Task 2 - Web APP 구축

{% hint style="success" %}
실습에 사용되는 서비스&#x20;

CloudShell : 브라우저에서 별도의 설치 없이 즉시 AWS CLI 명령을 실행할 수 있는 웹 기반 셸 환경

Amplify : 모바일 및 웹 애플리케이션의 백엔드 구축부터 프론트엔드 배포까지 빠르게 도와주는 풀스택 개발 도구

CodeCommit : 소스 코드를 안전하게 저장하고 관리할 수 있는 AWS 전용 프라이빗 Git 저장소 서비스
{% endhint %}



1. 코드커밋 서비스로 이동

<figure><img src="../../.gitbook/assets/image (221).png" alt=""><figcaption></figcaption></figure>



2. 리포지토리 생성 클릭

<figure><img src="../../.gitbook/assets/image (222).png" alt=""><figcaption></figcaption></figure>





3. 아래와 같이 설정 후 생성 클릭

* 리포지토리 이름 : user\*\*-wildrydes

<figure><img src="../../.gitbook/assets/image (223).png" alt="" width="563"><figcaption></figcaption></figure>



4. 상단 클라우드 쉘 버튼 클릭&#x20;

<figure><img src="../../.gitbook/assets/image (224).png" alt=""><figcaption></figcaption></figure>



5. CloudShell 에서 자격증명 도우미 활성화&#x20;

{% code overflow="wrap" %}
```
git config --global credential.helper '!aws codecommit credential-helper $@'
git config --global credential.UseHttpPath true
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (226).png" alt=""><figcaption></figcaption></figure>







6. 다시 콘솔로 돌아가서, 리포지토리 복제 명령을 복사한 뒤 클라우드 쉘에서 실행&#x20;

<figure><img src="../../.gitbook/assets/image (225).png" alt=""><figcaption></figcaption></figure>



&#x20;7\. 디렉토리 이동

```
cd user**-wildrydes
```



8. 소스 코드 다운로드

{% code overflow="wrap" %}
```
wget https://github.com/aws-samples/aws-serverless-workshops-kr/archive/refs/heads/master.zip
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (237).png" alt=""><figcaption></figcaption></figure>





9. 압축해제

{% code overflow="wrap" %}
```
unzip master.zip
```
{% endcode %}





10. 실습에 필요한 'website' 폴더 내용물을 현재 위치로 복사

{% code overflow="wrap" %}
```
cp -r aws-serverless-workshops-kr-master/WebApplication/1_StaticWebHosting/website/. ./
```
{% endcode %}





11. 나머지 파일삭제

{% code overflow="wrap" %}
```
rm -rf master.zip aws-serverless-workshops-kr-master
```
{% endcode %}



12. 해당 파일들을 CodeCommit 에 Push

{% code overflow="wrap" %}
```
git add .
git config --global user.email "user**@example.com"
git config --global user.name "user**"
git commit -m "initial checkin of website code"
git push
```
{% endcode %}



13. 검색창에 Amplify 를 검색하고 서비스로 이동

<figure><img src="../../.gitbook/assets/image (228).png" alt=""><figcaption></figcaption></figure>





14. 앱 배포 클릭

<figure><img src="../../.gitbook/assets/image (229).png" alt=""><figcaption></figcaption></figure>





15. 앱 배포 에서 CodeCommit 을 선택 한뒤 다음 클릭

<figure><img src="../../.gitbook/assets/image (230).png" alt=""><figcaption></figcaption></figure>





16. 아래와 같이 설정 후 저장 클릭

* 리포지토리 선택 : user\*\*-wildrydes

<figure><img src="../../.gitbook/assets/image (239).png" alt=""><figcaption></figcaption></figure>





17. 모든 설정을 그대로 둔 채 다음 클릭

<figure><img src="../../.gitbook/assets/image (240).png" alt=""><figcaption></figcaption></figure>





18. 저장 및 배포 클릭

<figure><img src="../../.gitbook/assets/image (241).png" alt=""><figcaption></figcaption></figure>





19. 약 2분 뒤 배포됨을 확인하고, 아래 도메인을 클릭하여 웹 화면 확인

<figure><img src="../../.gitbook/assets/image (243).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (244).png" alt=""><figcaption></figcaption></figure>







20. 코드 수정 (클라우드쉘에서)

{% code overflow="wrap" %}
```
sed -i 's/<title>Wild Rydes<\/title>/<title>Wild Rydes - Rydes of the Future!<\/title>/g' index.html
```
{% endcode %}





21. 코드 푸시

{% code overflow="wrap" %}
```
git add index.html
git commit -m "updated title"
git push
```
{% endcode %}





22. 앱 배포가 다시 진행되고 배포됨을 확인

<figure><img src="../../.gitbook/assets/image (245).png" alt=""><figcaption></figcaption></figure>





23. 타이틀이 변경된것을 확인

<figure><img src="../../.gitbook/assets/image (246).png" alt=""><figcaption></figcaption></figure>


# Task 2 - CodeCommit (컨텐츠 푸시)

## 실습 소개

Cloud9 환경에 웹 페이지 소스를 업로드합니다. 앞서 생성한 리포지토리와 Cloud9 환경을 연동한 뒤 git 명령어를 통해 푸시해봅니다.



1. IAM 서비스로 이동

<div align="left"><figure><img src="../../../.gitbook/assets/image (699).png" alt="" width="563"><figcaption></figcaption></figure></div>



2. 좌측 메뉴중 사용자 클릭, user## 검색, user## 클릭, 보안자격증명 탭 클릭

<figure><img src="../../../.gitbook/assets/image (700).png" alt=""><figcaption></figcaption></figure>



3. API 키 탭 -> API 키 생성 클릭

<figure><img src="../../../.gitbook/assets/image (983).png" alt=""><figcaption></figcaption></figure>

4. CodeCommit 선택 후 API 키 생성 클릭, 키 다운로드 클릭

<figure><img src="../../../.gitbook/assets/image (984).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (985).png" alt=""><figcaption></figcaption></figure>

5\. Cloud9 으로돌아와서 git 버전 확인

<pre><code><strong>git --version
</strong></code></pre>

<div align="left"><figure><img src="../../../.gitbook/assets/image (703).png" alt="" width="375"><figcaption></figcaption></figure></div>



6. 최초 제공 받았던 my-website.zip 파일을 드래그하여 우측 스샷 /home/ec2-user/environment에 드랍

<div align="left"><figure><img src="../../../.gitbook/assets/image (704).png" alt="" width="563"><figcaption></figcaption></figure></div>



7. 압축 풀고 내용 확인

```
unzip my-website.zip
ls
```

<div align="left"><figure><img src="../../../.gitbook/assets/image (705).png" alt="" width="375"><figcaption></figcaption></figure></div>



8. git 초기화

```
git init
```

<figure><img src="../../../.gitbook/assets/image (706).png" alt=""><figcaption></figcaption></figure>



9. git 추가 , 커밋, remote

```
git add .
git commit -m "first"
git remote add origin <리포지토리 주소>
```

<figure><img src="../../../.gitbook/assets/image (707).png" alt=""><figcaption></figcaption></figure>

주소확인은 다음 페이지로



10. 주소확인은 Codecommit 으로 이동 후 이전에 생성했던 리포지토리 클릭하여 3단계에서 주소 부분 확인

<figure><img src="../../../.gitbook/assets/image (708).png" alt=""><figcaption></figcaption></figure>



11. 푸시

```
git push origin master
```

<div align="left"><figure><img src="../../../.gitbook/assets/image (709).png" alt="" width="563"><figcaption></figcaption></figure></div>

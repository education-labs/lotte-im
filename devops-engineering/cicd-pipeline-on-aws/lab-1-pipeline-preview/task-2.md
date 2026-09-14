# Task 2 - 웹사이트 업데이트

## 실습 소개

웹 페이지 소스를 업데이트하여 S3 버킷에 업로드한 뒤, CodePipeline을 통해 수정된 소스를 재배포 해봅니다.



1. 이전에 만든 파이프라인에서 전환 비활성화 클릭, test 라고 입력 후 비활성화 클릭

<figure><img src="../../../.gitbook/assets/image (626).png" alt=""><figcaption></figcaption></figure>

<div align="left"><figure><img src="../../../.gitbook/assets/image (627).png" alt="" width="563"><figcaption></figcaption></figure></div>



2. 제공받은 my-website.zip을 압축해제하여 index.html 의 내용 수정(메모장 사용)

\<h2>Website Version: <mark style="background-color:green;">1</mark>.0\</h2> 을

\<h2>Website Version: <mark style="background-color:green;">2</mark>.0\</h2>로 수정



3. 다시 index.html파일과 error.html 파일을 my-website.zip 으로 압축

{% hint style="info" %}
주의사항 : zip 파일을 열었을때 폴더 없이, 바로 파일이 확인되어야함(아래 스크린샷참고)
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (628).png" alt=""><figcaption></figcaption></figure>



4. Amazon S3 버킷으로 이동, source 버킷 클릭, 새버전의 my-website.zip 파일을 업로드

<div align="left"><figure><img src="../../../.gitbook/assets/image (629).png" alt="" width="563"><figcaption></figcaption></figure></div>



5. 다시 홈페이지를 확인하여 여전히 1.0 버전을 확인

<div align="left"><figure><img src="../../../.gitbook/assets/image (630).png" alt=""><figcaption></figcaption></figure></div>



6. 파이프라인으로 돌아가 전환 활성화를 클릭, 활성화 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (631).png" alt="" width="563"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../../.gitbook/assets/image (632).png" alt="" width="563"><figcaption></figcaption></figure></div>



7. 다시 배포가 성공된 것을 확인하고 홈페이지 새로고침하여 2.0 버전 확인

<div align="left"><figure><img src="../../../.gitbook/assets/image (633).png" alt=""><figcaption></figcaption></figure></div>

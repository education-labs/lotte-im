# Task 4 - Using Git Command for Trigger

## 실습 소개

Cloud9 환경에서 웹 페이지 소스를 수정한 뒤 커밋, 푸시 작업을 수행합니다. 파이프라인에서 재배포를 확인하고 웹 페이지를 새로고침 해봅니다.



1. Cloud9 에서 index.html 을 더블클릭하고 버전을 2.0으로 수정한 뒤 ctrl+s 단축키로 저장

<figure><img src="../../../.gitbook/assets/image (715).png" alt=""><figcaption></figcaption></figure>



2. 커밋, 푸시 작업

```
git commit -a -m "2.0 update"
git push origin master
```

<div align="left"><figure><img src="../../../.gitbook/assets/image (716).png" alt="" width="563"><figcaption></figcaption></figure></div>



3. 파이프라인에서 업데이트 내역 확인

<figure><img src="../../../.gitbook/assets/image (735).png" alt=""><figcaption></figcaption></figure>





3. 웹 사이트 새로고침

<div align="left"><figure><img src="../../../.gitbook/assets/image (718).png" alt="" width="563"><figcaption></figcaption></figure></div>


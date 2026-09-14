# Task 10 - The Need for Automated Testing

## 실습 소개

소스 코드를 잘못 수정할 경우, 배포전 테스트의 필요성을 확인해봅니다.



1. Cloud9의 /home/ec2-user/environment/my-angular-project/src/app 에 있는app.component.html 파일을 수정 (Version: 3.0으로) 한 뒤 저장 (Ctrl+S)

<figure><img src="../../../.gitbook/assets/image (795).png" alt=""><figcaption></figcaption></figure>



2. /home/ec2-user/environment/my-angular-project/src/app/calculator 의 calculator.component.ts 파일을 오픈하고 , 아래 스샷처럼 수정한 뒤 저장 (Ctrl+S)

<figure><img src="../../../.gitbook/assets/image (796).png" alt=""><figcaption></figcaption></figure>



3. git commit, push

```
git commit -a -m "Version 3.0"
git push origin master
```

<div align="left"><figure><img src="../../../.gitbook/assets/image (797).png" alt="" width="563"><figcaption></figcaption></figure></div>



4. 파이프라인으로 이동하여 모두 성공이 되면 웹페이지 새로고침하여 버전 확인

<div align="left"><figure><img src="../../../.gitbook/assets/image (798).png" alt="" width="563"><figcaption></figcaption></figure></div>



5. 40+5 을 계산 테스트 시도

<div align="left"><figure><img src="../../../.gitbook/assets/image (799).png" alt=""><figcaption></figcaption></figure></div>

결과 값이 정상이 아님을 확인



6. 디버깅작업

/home/ec2-user/environment/my-angular-project/src/app/calculator 의 calculator.component.ts 파일을 오픈하고 , 아래 스샷처럼 뒤 저장 (Ctrl+S)

<div align="left"><figure><img src="../../../.gitbook/assets/image (800).png" alt="" width="563"><figcaption></figcaption></figure></div>



7. /home/ec2-user/environment/my-angular-project/src/app 에 있는 app.component.html 파일을 수정 (Version: 3.1 로) 한 뒤 저장 (Ctrl+S)

<figure><img src="../../../.gitbook/assets/image (801).png" alt=""><figcaption></figcaption></figure>



8. 디버깅 작업과 버전 수정을 완료했으면 다시 git commit, push

```
git commit -a -m "bug fix"
git push origin master
```



9. 파이프라인으로 이동하여 모두 성공이 되면 웹페이지 새로고침하여 버전 확인

<div align="left"><figure><img src="../../../.gitbook/assets/image (802).png" alt="" width="563"><figcaption></figcaption></figure></div>



10. 다시 정상으로 계산기가 동작하는지 테스트

<figure><img src="../../../.gitbook/assets/image (803).png" alt=""><figcaption></figcaption></figure>

이렇게 수동으로 코드를 수정하고, 디버깅을 할 수 있지만,&#x20;

배포 전 테스트를 진행할 수 있다. (다음 LAB에서)

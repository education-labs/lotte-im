---
hidden: true
---

# \[github] Task 2 - Code Push (컨텐츠 푸시)

## 실습 소개

Cloud9 환경에 웹 페이지 소스를 업로드합니다. 앞서 생성한 리포지토리와 Cloud9 환경을 연동한 뒤 git 명령어를 통해 푸시해봅니다.

1. Cloud9 머신에 이전에 받았던 my-website.zip 버전 1.0 파일을 업로드

(해당 파일을 user## 폴더에 드래그앤드랍)

<figure><img src="../../../.gitbook/assets/image (666).png" alt=""><figcaption></figcaption></figure>



2. 압축 해제 및 확인&#x20;

```
unzip my-website.zip
ls
```

<figure><img src="../../../.gitbook/assets/image (667).png" alt=""><figcaption></figcaption></figure>



3. git 초기화

```
git init
```

<figure><img src="../../../.gitbook/assets/image (668).png" alt=""><figcaption></figcaption></figure>



4. git 인증 정보 등록

```
git config --global user.name <github 계정명>
git config --global user.email <github 이메일주소>
```

계정명은 아래 스크린샷에서 확인

<figure><img src="../../../.gitbook/assets/image (669).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../../.gitbook/assets/image (670).png" alt=""><figcaption></figcaption></figure>



5. git 원격 연결 (아래 스크린샷의 명령을 복사하여 사용)

```
git remote add origin <원격주소>
```

<figure><img src="../../../.gitbook/assets/image (671).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (672).png" alt=""><figcaption></figcaption></figure>



6. Github 우측 상단 프로필 아이콘을 클릭, 하단의 Settings 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (673).png" alt=""><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../../.gitbook/assets/image (674).png" alt="" width="427"><figcaption></figcaption></figure></div>



7. 좌측 메뉴 중 Developer settings 클릭

<div align="left"><figure><img src="../../../.gitbook/assets/image (675).png" alt="" width="353"><figcaption></figcaption></figure></div>

8. Personal access tokens 클릭, Tokens (classic) 클릭, Generate new token 클릭, Classic 클릭



<figure><img src="../../../.gitbook/assets/image (676).png" alt=""><figcaption></figcaption></figure>





9. Note 에 cicd\_web 입력후 7days 선택, repo 선택 후 하단 Generate 클릭

<figure><img src="../../../.gitbook/assets/image (677).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (678).png" alt=""><figcaption></figcaption></figure>



10. 출력되는 token 키를 로컬 메모장에 저장(주의! Cloud9 또는 github 등 공개된 저장소에 저장하지마세요.)

<figure><img src="../../../.gitbook/assets/image (679).png" alt=""><figcaption></figcaption></figure>



11. 인증정보 메모리에 저장 셋팅 (48시간 동안 캐시메모리에 저장)

```
git config --global credential.helper "cache --timeout=172800"
```

<figure><img src="../../../.gitbook/assets/image (680).png" alt=""><figcaption></figcaption></figure>



12. git add, commit, push&#x20;

(username 에는 4번단계에서 확인했던 계정명 입력/Password 에는 메모장에 저장했던 token키 입력)

```
git add .
git commit -m "1st commit"
git push origin master
```

<figure><img src="../../../.gitbook/assets/image (681).png" alt=""><figcaption></figcaption></figure>



13. github 으로 가서 push 가 되었는지 확인

<figure><img src="../../../.gitbook/assets/image (682).png" alt=""><figcaption></figcaption></figure>

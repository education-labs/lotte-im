# Task 2. EFS 파일시스템 사용





1. EFS 파일시스템을 사용할 인스턴스의 보안그룹에 인바운드 규칙을 하나 추가합니다.

(user\*\*-web-sg, user\*\*-wasserver-sg)

* 유형 : NFS
* 소스 : Anywhere-IPv4

<figure><img src="../../.gitbook/assets/image (283).png" alt=""><figcaption></figcaption></figure>



2. 위 인스턴스가 배치된 서브넷과 연결된 NACL 설정에도 인바운드 규칙 추가

(기본 NACL, user\*\*-was-nacl)

<figure><img src="../../.gitbook/assets/image (493).png" alt=""><figcaption></figcaption></figure>



3. 서비스 검색창에 EFS를 검색하고 EFS를 클릭

<figure><img src="../../.gitbook/assets/image (284).png" alt=""><figcaption></figcaption></figure>



4. 파일시스템 생성 클릭

<figure><img src="../../.gitbook/assets/image (285).png" alt="" width="440"><figcaption></figcaption></figure>



5. 아래와 같이 입력 및 선택 후 파일시스템 생성 클릭

* 이름 : user\*\*-efs
* vpc : user\*\*-vpc 선택

<figure><img src="../../.gitbook/assets/image (286).png" alt=""><figcaption></figcaption></figure>



6. 생성된 파일시스템의 상태가 사용가능이 되면, 이름을 클릭

<figure><img src="../../.gitbook/assets/image (495).png" alt=""><figcaption></figcaption></figure>



7. 하단의 네트워크 탭 클릭, 관리 클릭

<figure><img src="../../.gitbook/assets/image (497).png" alt=""><figcaption></figcaption></figure>



8. 탑재대상의 보안그룹에 1에서 작업한 두 보안그룹을 선택

<figure><img src="../../.gitbook/assets/image (498).png" alt=""><figcaption></figcaption></figure>



9. 아래 가용영역의 보안그룹도 8번처럼 두 보안그룹 선택 후 저장 클릭

<figure><img src="../../.gitbook/assets/image (499).png" alt=""><figcaption></figcaption></figure>



9. 이전 실습을 참고하여 web 인스턴스에 접속하고 efs를 마운트할 디렉토리 생성

```
mkdir ~/efs-mount-point1
```

<figure><img src="../../.gitbook/assets/image (291).png" alt=""><figcaption></figcaption></figure>



10. 이전 실습을 참고하여 was 인스턴스에 접속하고 efs를 마운트할 디렉토리 생성

```
mkdir ~/efs-mount-point2
```

<figure><img src="../../.gitbook/assets/image (292).png" alt=""><figcaption></figcaption></figure>



11. efs 화면에서 우측 상단 연결 클릭

<figure><img src="../../.gitbook/assets/image (293).png" alt=""><figcaption></figcaption></figure>



12. NFS 클라이언트 사용 명령을 복사

<figure><img src="../../.gitbook/assets/image (294).png" alt=""><figcaption></figcaption></figure>



13. 각각의 인스턴스에서 복사한 명령 실행하되 맨 뒤에있는 EFS를 각 인스턴스에서 생성했던 디렉토리이름으로 수정한뒤 실행

예시)

```
sudo mount -t nfs4 -o nfsve~~~~~~~p-northeast-2.amazonaws.com:/ efs-mount-point1
```

<figure><img src="../../.gitbook/assets/image (295).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (296).png" alt=""><figcaption></figcaption></figure>



14. web 인스턴스에서 테스트 파일을 하나 생성

```
cd efs-mount-point1
sudo touch helloefs
```



15. was 인스턴스 에서 생성된 파일을 확인

```
cd efs-mount-point2
ls
```

<figure><img src="../../.gitbook/assets/image (297).png" alt=""><figcaption></figcaption></figure>



16. 확인을 다 마쳤다면 efs 삭제 작업을 진행합니다. 우선 인스턴스에 연결했던것을 해제합니다.

* web 인스턴스

```
cd
sudo umount efs-mount-point1
```



* was 인스턴스

```
cd
sudo umount efs-mount-point2
```





17. EFS 콘솔 화면에서 삭제할 EFS 파일시스템을 선택하고 우측 상단 삭제 클릭

<figure><img src="../../.gitbook/assets/image (298).png" alt=""><figcaption></figcaption></figure>


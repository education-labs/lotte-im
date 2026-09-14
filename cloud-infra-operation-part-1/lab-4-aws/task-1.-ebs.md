# Task 1. EBS 볼륨 추가

1. EC2 콘솔에서 좌측 메뉴중 볼륨을 클릭, 우측 상단 볼륨 생성 클릭

<figure><img src="../../.gitbook/assets/image (266).png" alt=""><figcaption></figcaption></figure>





2. 아래 설정으로 볼륨 설정 입력

* 볼륨 유형 : 범용 SSD(gp3)
* 크기 : 1 GiB
* 가용영역 : ap-northeast-2a

<figure><img src="../../.gitbook/assets/image (267).png" alt="" width="338"><figcaption></figcaption></figure>



* 암호화 : 이 볼륨 암호화 체크&#x20;
* KMS 키 : (기본값) 선택

<figure><img src="../../.gitbook/assets/image (275).png" alt=""><figcaption></figcaption></figure>



* 태그 \
  키 : Name / 값 : user\*\*-ebs

태그 까지 입력을 완료했다면 아래 볼륨 생성 클릭

<figure><img src="../../.gitbook/assets/image (276).png" alt=""><figcaption></figcaption></figure>



3. 볼륨이 사용 가능 상태가 되면 우측 작업 메뉴에서 “볼륨 연결”을 선택

<figure><img src="../../.gitbook/assets/image (268).png" alt=""><figcaption></figcaption></figure>





4. user\*\*-linux-web-2 인스턴스를 선택하고, 디바이스 이름은 /dev/sdb 를 선택 한뒤 볼륨 연결 클릭

<figure><img src="../../.gitbook/assets/image (269).png" alt=""><figcaption></figcaption></figure>



5. 이전 실습을 참고하여 user\*\*-linux-web-2 서버에 접속 (powershell로)







6. lsblk 명령을 통해 볼륨이 가상서버에 연결된 것을 확인합니다. (xvdf로 연결)

```
sudo lsblk
```

<figure><img src="../../.gitbook/assets/image (271).png" alt=""><figcaption></figcaption></figure>



6. mkfs 명령어로 해당 디바이스를 xfs 파일 시스템으로 포멧합니다.

```
sudo mkfs -t xfs /dev/nvme1n1
```

<figure><img src="../../.gitbook/assets/image (272).png" alt=""><figcaption></figcaption></figure>





8. /data를 마운트 포인트로 생성하고, 마운트 합니다.

```
sudo mkdir /data
```

```
sudo mount /dev/nvme1n1  /data
```

<figure><img src="../../.gitbook/assets/image (281).png" alt=""><figcaption></figcaption></figure>



10. df 명령어로 마운트 상태를 확인합니다.

    /dev/nvme1n1 디바이스가 /data로 연결되었고 , 사이즈도 확인합니다.

```
df -h
```

<figure><img src="../../.gitbook/assets/image (492).png" alt=""><figcaption></figcaption></figure>

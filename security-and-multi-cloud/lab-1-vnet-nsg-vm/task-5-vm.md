# Task 5 - VM 2대 생성

1. "가상 머신"을 검색해 [만들기] → [Azure 가상 머신]
2. 1번 VM : `snet-web` 에 배치, **공인 IP 부여** (실습 편의)
3. 2번 VM : `snet-app` 에 배치, **공인 IP 없음**
4. 인증 형식은 SSH 공개 키, 새 키 쌍을 생성해 내려받습니다.
5. [검토 + 만들기] → [만들기] → 키 다운로드
6. 1번 VM에 SSH로 접속합니다.

**입력값**

<table header-row="true">
<tr>
<td>항목</td>
<td>값</td>
</tr>
<tr>
<td>이미지</td>
<td>Ubuntu Server 24.04 LTS</td>
</tr>
<tr>
<td>크기</td>
<td>Standard_B2s</td>
</tr>
<tr>
<td>인증</td>
<td>SSH 공개 키</td>
</tr>
<tr>
<td>VM1 서브넷</td>
<td>snet-web (공인 IP 있음)</td>
</tr>
<tr>
<td>VM2 서브넷</td>
<td>snet-app (공인 IP 없음)</td>
</tr>
<tr>
<td>공인 IP SKU</td>
<td>Standard</td>
</tr>
</table>

```bash
chmod 400 <키파일>.pem
ssh-add <키파일>.pem                                  # 에이전트에 키 등록
ssh -A -i <키파일>.pem azureuser@<VM1 공인 IP>       # -A: 키 전달

# VM1 에서 VM2 로 통신 확인
ping -c 4 10.NN.9.4
ssh azureuser@10.NN.9.4                              # -A 접속이어야 성공
```

- [ ] VM 2대가 실행 중인가
- [ ] VM1에 SSH 접속이 되는가
- [ ] VM1 → VM2 ping이 통과하는가
- [ ] VM2에는 공인 IP가 없는가

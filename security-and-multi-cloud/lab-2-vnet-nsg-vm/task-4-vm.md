# Task 4 - VM 2대 생성

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

7. **VM1의 공인 IP와 VM2의 사설 IP를 포털에서 확인해 메모합니다.** (각 VM → [개요])

> 💡 `snet-app`(`10.**.9.0/24`)의 첫 VM이면 대개 **`10.**.9.4`** 가 할당됩니다. Azure가 각 서브넷의 앞 4개 주소(네트워크·게이트웨이·DNS×2)를 예약하기 때문입니다. **다를 수 있으니 아래 명령에는 실제로 확인한 IP를 넣으세요.**

```bash
chmod 400 <키파일>.pem
ssh-add <키파일>.pem                                  # 에이전트에 키 등록
ssh -A -i <키파일>.pem azureuser@<VM1 공인 IP>       # -A: 키 전달

# VM1 에서 VM2 로 통신 확인 (10.**.9.4 = 메모해 둔 VM2 사설 IP)
ping -c 4 10.**.9.4
ssh azureuser@10.**.9.4                              # -A 접속이어야 성공
```

> ⚠️ **ping이 안 되면** Task 3에서 각 NSG에 **ICMP 허용 규칙**을 넣었는지 확인하세요. 기본 규칙만으로는 VNet 내부 통신이 열려 있어도 ICMP가 막혀 있을 수 있습니다.

- [ ] VM 2대가 실행 중인가
- [ ] VM1에 SSH 접속이 되는가
- [ ] VM1 → VM2 ping이 통과하는가
- [ ] VM2에는 공인 IP가 없는가

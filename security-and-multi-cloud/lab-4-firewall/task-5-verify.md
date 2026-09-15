# Task 5 - 허용·차단 검증

규칙대로 동작하는지 실제 트래픽으로 확인합니다.

```bash
# ① 허용된 도메인 — 성공해야 함
curl -I https://archive.ubuntu.com

# ② 허용되지 않은 도메인 — 차단되어야 함
curl -I --max-time 10 https://www.google.com

# ③ 스포크 내부 통신(VM2) — 성공해야 함
ping -c 4 10.**.9.4
```

<figure><img src="../../.gitbook/assets/image (1122).png" alt=""><figcaption></figcaption></figure>



> 💡 ③의 대상은 **Lab 2 Task 4에서 메모한 VM2 사설 IP** 입니다. 같은 VNet 안 통신이라 UDR(`0.0.0.0/0`)이 아니라 더 구체적인 **시스템 경로(VirtualNetwork)** 를 타므로 방화벽을 거치지 않고 성공합니다 — UDR을 걸어도 내부 통신은 그대로라는 것을 확인하는 항목입니다.

1. VM1에 SSH로 접속합니다. — **접속이 안 되면 Task 3의 SSH 예외 경로(7\~8번)를 빼먹은 것입니다.** 경로 테이블에 `내IP/32 → 인터넷`을 추가하면 바로 복구됩니다.





* [ ] ① 허용 도메인 접속이 성공하는가
* [ ] ② 미허용 도메인이 차단되는가
* [ ] ③ 스포크 내부 통신이 되는가


 ---
 title: 2025-10-21
 author: 강병호
 date: 2025-10-21
 category: TIL/강병호/2025/10 (파일 경로 : TIL/{이름}/{연}/{월})
 layout: post (자유)
 ---

### Connetion Timeout
Connection Timeout은 클라이언트가 서버에 연결을 시도할 때, 일정 시간 내에 연결이 이루어지지 않으면 발생하는 타임아웃입니다. TCP 소켓 통신에서 클라이언트와 서버가 연결될 때, 정확한 전송을 보장하기 위해 사전에 세션을 수립하는데, 이 과정을 3-way-handshake라고 합니다. Connection Timeout은 이 3-way-handshake가 일정 시간 내에 완료되지 않을 때 발생합니다. 즉, 서버의 장애나 응답 지연으로 인해 연결을 맺지 못하면 Connection Timeout이 발생합니다.

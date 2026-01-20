---
title: 2026-01-20
author: 김소희
date: 2026-01-20
category: TIL/김소희/2026/01
layout: post
---

## 핵심 요약

HTTP, Socket, WebSocket, WebRTC의 차이점을 정리했다. 가장 큰 깨달음은 HTTP의 단방향·비상태성과 Socket의 양방향·상태 관리의 차이였다. 실시간 통신이 필요할 때 왜 소켓을 쓰는지 명확해졌다.

## 주요 포인트

### HTTP vs Socket

| 구분 | HTTP          | Socket    |
| ---- | ------------- | --------- |
| 방향 | 단방향        | 양방향    |
| 상태 | Stateless     | Stateful  |
| 연결 | 요청마다 끊김 | 연결 유지 |

### WebSocket

- 웹에서 소켓처럼 양방향 통신 가능
- 초기 핸드셰이크는 HTTP → 이후 TCP 연결 유지
- 채팅, 실시간 알림 등에 활용

### WebRTC

- P2P 통신 지원 기술
- 구성: Signaling Server + STUN Server + TURN Server
- UDP 사용 (속도 우선)

## 결론

STOMP 같은 프레임워크 쓰기 전에 로우 레벨 웹 소켓 프로그래밍을 경험해보고 싶다. 기본 개념을 확실히 알아야 문제 발생 시 대응할 수 있다.

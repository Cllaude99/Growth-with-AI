# WebSocket

## 메타데이터
- **카테고리**: 네트워크
- **난이도**: 중급
- **우선순위**: 중간
- **선수과목**: HTTP & HTTPS
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- WebSocket이 무엇이며, 기존 HTTP 통신과 어떤 점에서 근본적으로 다른가요?

### 꼬리 질문
- HTTP가 단방향(request-response) 통신인데 반해, WebSocket이 양방향(bidirectional) 통신을 가능하게 하는 원리는 무엇인가요?
- WebSocket이 HTTP 포트(80, 443)를 사용하는 이유는 무엇이며, 이것이 방화벽 통과와 어떤 관련이 있나요?

### 예상 이해 수준
WebSocket은 HTTP 업그레이드 메커니즘을 통해 시작되지만, 한번 연결이 수립되면 지속적인 양방향 통신 채널을 유지하는 프로토콜임을 설명할 수 있어야 한다. HTTP처럼 매 통신마다 새로운 연결을 맺지 않고, 하나의 TCP 연결 위에서 서버와 클라이언트가 언제든 메시지를 주고받을 수 있다는 차이점을 이해해야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- 실시간 데이터를 클라이언트에 전달하는 방법으로 Polling, Long Polling, SSE(Server-Sent Events), WebSocket을 비교하고, 각각이 적합한 시나리오를 설명해주세요.

### 꼬리 질문
- Short polling과 Long polling을 사용했을 때 서버에 가해지는 부하의 차이는 무엇이며, 왜 WebSocket이 이를 더 효율적으로 처리할 수 있나요?
- SSE는 WebSocket과 달리 단방향 통신만 가능한데, 실시간 뉴스 피드나 주식 시세처럼 서버에서 클라이언트로만 데이터를 보내면 되는 경우에 SSE가 WebSocket보다 나은 점은 무엇인가요?

### 예상 이해 수준
Polling은 주기적으로 서버에 요청하여 불필요한 요청이 많고, Long polling은 서버가 응답을 지연시켜 효율성을 높이지만 연결 유지 비용이 있음을 설명할 수 있어야 한다. SSE는 HTTP 위에서 동작하여 HTTP/2 멀티플렉싱과 자동 재연결을 지원하지만 단방향이고, WebSocket은 완전한 양방향 통신을 제공하지만 HTTP 인프라(프록시, 로드밸런서) 설정이 더 복잡하다는 트레이드오프를 설명할 수 있어야 한다.

## Level 3: 심화

### 핵심 질문
- WebSocket의 handshake 과정, connection lifecycle, 그리고 heartbeat(ping/pong) 메커니즘이 왜 필요하고 어떻게 동작하는지 설명해주세요.

### 꼬리 질문
- WebSocket 연결이 중간의 프록시나 로드밸런서에 의해 끊기는 문제(connection timeout)가 발생하는 이유와, heartbeat이 이를 어떻게 방지하나요?
- WebSocket 연결이 비정상적으로 끊겼을 때 클라이언트 측에서 자동으로 재연결하는 로직(exponential backoff with jitter)을 어떻게 구현해야 하나요?

### 예상 이해 수준
WebSocket handshake가 HTTP Upgrade 요청(Upgrade: websocket 헤더)을 통해 시작되고, 서버가 101 Switching Protocols로 응답하면 WebSocket 프로토콜로 전환된다는 것을 설명할 수 있어야 한다. WebSocket 프레임 구조(opcode: text/binary/ping/pong/close), connection lifecycle(CONNECTING, OPEN, CLOSING, CLOSED 상태), 그리고 idle connection이 중간 장비에 의해 끊기는 것을 방지하기 위한 heartbeat의 역할을 이해해야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- 수백만 명의 동시 사용자를 지원하는 실시간 채팅 시스템을 설계해주세요. WebSocket 서버의 수평 확장(horizontal scaling) 문제와 이를 해결하는 방법을 포함해주세요.

### 꼬리 질문
- WebSocket 서버를 여러 대로 수평 확장할 때, 서로 다른 서버에 연결된 사용자 간에 메시지를 전달하려면 어떻게 해야 하나요? (Pub/Sub 패턴, Redis)
- 채팅 시스템에서 사용자가 오프라인 상태일 때 보낸 메시지를 나중에 받을 수 있도록 하는 메시지 전달 보장(message delivery guarantee) 메커니즘을 어떻게 설계하나요?

### 예상 이해 수준
WebSocket 서버는 stateful하기 때문에 로드밸런서가 Sticky session(IP 해싱 또는 쿠키 기반)을 사용해야 한다는 점을 설명할 수 있어야 한다. 여러 서버 인스턴스 간 메시지 브로드캐스팅을 위해 Redis Pub/Sub 또는 Kafka를 사용하는 아키텍처를 설계할 수 있어야 한다. 메시지 영속성을 위한 DB 저장, at-least-once delivery를 위한 ACK 메커니즘, 오프라인 사용자를 위한 푸시 알림 연동도 설계에 포함할 수 있으면 좋다.

## 현실 연결
- Slack과 Discord는 WebSocket을 사용하여 실시간 메시지 전달, 사용자 온라인 상태(presence) 업데이트, 타이핑 인디케이터를 구현하며, 수천만 명의 동시 사용자를 지원한다.
- 실시간 주식 차트(증권사 HTS, Bloomberg Terminal)는 WebSocket을 통해 밀리초 단위의 가격 업데이트를 클라이언트에 전달하며, 데이터 전송량 최적화를 위해 이진(binary) 프레임 형식을 사용한다.
- Figma, Google Docs 같은 실시간 협업 도구는 WebSocket을 통해 여러 사용자의 편집 내용을 동기화하며, OT(Operational Transformation) 또는 CRDT 알고리즘을 사용하여 충돌을 해결한다.
- Socket.IO는 WebSocket을 추상화하여 자동 재연결, fallback(Long Polling), 룸/네임스페이스 관리 등의 기능을 제공하는 Node.js 라이브러리로, 실시간 웹 애플리케이션 개발에 널리 사용된다.

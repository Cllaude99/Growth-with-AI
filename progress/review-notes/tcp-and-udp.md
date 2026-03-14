# TCP & UDP - 복습 노트

## 학습일: 2026-03-14
## 판정: 통과 (Level 4)

## Level 1: 기본 이해

### 주요 질문
- TCP와 UDP의 기본 특성과 차이점은?
- TCP가 "연결 지향적"이라는 의미는?
- 3-way handshake의 각 단계와 시퀀스 번호 교환 과정은?
- (검증) DNS 쿼리가 UDP를 사용하는 이유는?

### 다시 공부할 내용
- 특별히 없음. 기본 개념과 handshake 과정을 잘 이해함.

## Level 2: 트레이드오프

### 주요 질문
- 영상 스트리밍에서 TCP vs UDP의 사용자 경험 차이는?
- (검증) 채팅 메시지 전송에 TCP가 적합한 이유는?

### 다시 공부할 내용
- 특별히 없음. 데이터 특성별 프로토콜 선택 기준을 잘 이해함.

## Level 3: 심화

### 주요 질문
- 4-way handshake가 3단계가 아닌 4단계인 이유는?
- TIME_WAIT 상태가 많이 쌓이면 어떤 문제가 발생하나?
- (검증) Flow control vs congestion control의 차이는?

### 다시 공부할 내용
- Sliding window 기반 flow control의 구체적 동작 방식
- Slow start, Congestion avoidance, Fast retransmit, Fast recovery 알고리즘
- TIME_WAIT 상태의 지속 시간(2MSL)과 설정 방법

## Level 4: 시스템 설계

### 주요 질문
- 실시간 게임에서 프로토콜 혼용 설계는?
- 패킷 손실 시 클라이언트 측 위치 보정 방법은?
- (검증) HTTP/3(QUIC)가 TCP 대신 UDP를 사용하는 이유는?

### 다시 공부할 내용
- 보간(interpolation)과 dead reckoning의 구체적 구현 방식
- QUIC의 멀티플렉싱과 스트림별 독립적 신뢰성 보장 메커니즘
- QUIC의 0-RTT 연결 수립 방식

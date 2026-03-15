# 학습 진행 기록

## 요약 통계

| 항목 | 값 |
|------|-----|
| 총 세션 수 | 10 |
| 다룬 토픽 수 | 8 / 52 |
| 연속 학습일 | 8일 |
| 마지막 학습일 | 2026-03-15 |

## 카테고리별 커버리지

### 자료구조 & 알고리즘 (2/10)

| 토픽 | 상태 | 도달 난이도 | 마지막 학습일 |
|------|------|------------|-------------|
| Array & Linked List | 통과 | Level 4 | 2026-03-09 |
| Hash Table | 통과 | Level 4 | 2026-03-10 |
| Tree (BST, AVL) | - | - | - |
| Sorting | - | - | - |
| Stack & Queue | - | - | - |
| Graph | - | - | - |
| Heap | - | - | - |
| Dynamic Programming | - | - | - |
| Searching | - | - | - |
| Greedy | - | - | - |

### 운영체제 (0/7)

| 토픽 | 상태 | 도달 난이도 | 마지막 학습일 |
|------|------|------------|-------------|
| Process & Thread | - | - | - |
| Memory Management | - | - | - |
| CPU Scheduling | - | - | - |
| Synchronization | - | - | - |
| Deadlock | - | - | - |
| Virtual Memory | - | - | - |
| File System | - | - | - |

### 네트워크 (2/7)

| 토픽 | 상태 | 도달 난이도 | 마지막 학습일 |
|------|------|------------|-------------|
| TCP & UDP | 통과 | Level 4 | 2026-03-14 |
| HTTP & HTTPS | 통과 | Level 4 | 2026-03-15 |
| OSI & TCP/IP | - | - | - |
| DNS | - | - | - |
| REST API | - | - | - |
| WebSocket | - | - | - |
| Cookie, Session, JWT | - | - | - |

### 데이터베이스 (0/6)

| 토픽 | 상태 | 도달 난이도 | 마지막 학습일 |
|------|------|------------|-------------|
| Indexing | - | - | - |
| Transaction & ACID | - | - | - |
| SQL Join | - | - | - |
| Normalization | - | - | - |
| NoSQL | - | - | - |
| Query Optimization | - | - | - |

### 시스템 설계 (0/6)

| 토픽 | 상태 | 도달 난이도 | 마지막 학습일 |
|------|------|------------|-------------|
| Caching | - | - | - |
| Load Balancing | - | - | - |
| CAP Theorem | - | - | - |
| Message Queue | - | - | - |
| DB Replication & Sharding | - | - | - |
| Microservices vs Monolith | - | - | - |

### 소프트웨어 공학 (0/6)

| 토픽 | 상태 | 도달 난이도 | 마지막 학습일 |
|------|------|------------|-------------|
| OOP Principles | - | - | - |
| SOLID | - | - | - |
| Design Patterns | - | - | - |
| Testing Strategies | - | - | - |
| Clean Code | - | - | - |
| CI/CD | - | - | - |

### 웹 & 프론트엔드 (4/8)

| 토픽 | 상태 | 도달 난이도 | 마지막 학습일 |
|------|------|------------|-------------|
| Event Loop & 비동기 | 통과 | Level 4 | 2026-03-12 |
| Closure & Scope | 통과 | Level 4 | 2026-03-13 |
| Prototype & Inheritance | 통과 | Level 4 | 2026-03-14 |
| Browser Rendering Pipeline | 통과 | Level 4 | 2026-03-14 |
| Virtual DOM & Reconciliation | - | - | - |
| Web Security (XSS, CSRF, CORS) | - | - | - |
| Frontend Performance & Core Web Vitals | - | - | - |
| Module Systems & Bundling | - | - | - |

### 이력서 면접 (0/2)

| 토픽 | 상태 | 도달 난이도 | 마지막 학습일 |
|------|------|------------|-------------|
| Syncspot | - | - | - |
| Pinoco | - | - | - |

## 세션 히스토리

<!-- 새로운 세션 기록은 이 줄 아래에 역순으로 추가 -->

### 세션 #10 - 2026-03-15
- **모드**: CS학습 (재도전)
- **토픽**: HTTP & HTTPS
- **도달 난이도**: Level 4
- **판정**: 통과
- **핵심 깨달음**: HSTS로 브라우저가 항상 HTTPS로 접속하도록 강제, TOFU(Trust On First Use) 문제와 HSTS Preload List로 최초 접속부터 HTTPS 강제, SSL stripping 공격 방어
- **다음 추천**: OSI & TCP/IP

### 세션 #9 - 2026-03-15
- **모드**: CS학습 (재도전)
- **토픽**: HTTP & HTTPS
- **도달 난이도**: Level 3
- **판정**: 학습중
- **핵심 깨달음**: 비대칭 암호화(안전, 키 교환용) → 대칭 암호화(빠름, 데이터 전송용) 역할 분담 확립, CA 인증서 체계 이해, TLS termination과 CDN ↔ Origin 구간별 HTTPS 암호화 필요성
- **보완 필요**: HSTS(HTTP Strict Transport Security)와 SSL stripping 공격 방어, TLS handshake 세부 단계
- **다음 추천**: HTTP & HTTPS Level 4 재도전

### 세션 #8 - 2026-03-15
- **모드**: CS학습
- **토픽**: HTTP & HTTPS
- **도달 난이도**: Level 2
- **판정**: 학습중
- **핵심 깨달음**: HTTP의 request/response 구조(Header/Body), stateless 특성과 쿠키/세션 필요성, HTTP/1.1(Keep-alive) → HTTP/2(멀티플렉싱, 헤더 압축) → HTTP/3(QUIC, 독립 스트림), 비대칭 암호화로 키 교환 후 대칭 암호화로 데이터 전송하는 TLS 구조 이해
- **보완 필요**: TLS handshake 세부 과정, CA 인증서 체계와 신뢰 체인, CDN의 TLS termination과 구간별 암호화
- **다음 추천**: HTTP & HTTPS 재도전 (Level 3~4)

### 세션 #7 - 2026-03-14
- **모드**: CS학습
- **토픽**: TCP & UDP
- **도달 난이도**: Level 4
- **판정**: 통과
- **핵심 깨달음**: TCP(연결 지향, 신뢰성) vs UDP(비연결, 속도) 특성과 3-way handshake 시퀀스 번호 교환, 데이터 특성별 프로토콜 선택(영상 스트리밍-UDP, 채팅-TCP), 4-way handshake가 필요한 이유(남은 데이터 전송)와 TIME_WAIT 포트 고갈 문제, flow control(수신자 기준) vs congestion control(네트워크 기준), 프로토콜 혼용 설계와 보간/dead reckoning, QUIC이 head-of-line blocking을 해결하는 원리
- **다음 추천**: HTTP & HTTPS

### 세션 #6 - 2026-03-14
- **모드**: CS학습
- **토픽**: Browser Rendering Pipeline
- **도달 난이도**: Level 4
- **판정**: 통과
- **핵심 깨달음**: Critical Rendering Path(DOM → CSSOM → Render Tree → Layout → Paint → Composite), display:none은 Render Tree 제외 vs visibility:hidden은 포함, Reflow가 Repaint를 동반하므로 더 비싼 작업, transform/opacity는 GPU Compositor 레이어에서 처리, async(순서 미보장 즉시 실행) vs defer(파싱 후 순서 보장), CSS가 CSSOM 의존으로 JS 실행을 블로킹, Critical CSS inline + code splitting + SSR/hydration 최적화 전략
- **다음 추천**: Virtual DOM & Reconciliation

### 세션 #5 - 2026-03-14
- **모드**: CS학습
- **토픽**: Prototype & Inheritance
- **도달 난이도**: Level 4
- **판정**: 통과
- **핵심 깨달음**: Prototype chain을 통한 프로퍼티 검색 흐름(객체 → 상위 prototype → Object.prototype → null), class는 prototype의 syntactic sugar이며 new의 4단계 내부 동작, prototype pollution 공격의 원리와 키 필터링/Object.freeze 방어, hasOwnProperty()로 자체 프로퍼티 확인, 상속의 강한 결합 문제와 composition(hooks) 패턴의 유연성
- **다음 추천**: Browser Rendering Pipeline

### 세션 #4 - 2026-03-13
- **모드**: CS학습
- **토픽**: Closure & Scope
- **도달 난이도**: Level 4
- **판정**: 통과
- **핵심 깨달음**: Lexical scoping으로 함수 선언 위치 기준 scope 결정, var(함수 스코프) vs let(블록 스코프)의 차이와 for 루프 closure 문제, closure로 정보 은닉(module 패턴) 및 디바운스 함수의 closure 활용, 메모리 누수 방지를 위한 removeEventListener/cleanup, stale closure 문제와 useRef를 통한 해결
- **다음 추천**: Prototype & Inheritance

### 세션 #3 - 2026-03-12
- **모드**: CS학습
- **토픽**: Event Loop & 비동기
- **도달 난이도**: Level 4
- **판정**: 통과
- **핵심 깨달음**: call stack → Web APIs → callback queue → event loop의 비동기 처리 흐름, microtask(Promise)가 macrotask(setTimeout)보다 우선 처리되며 무한 microtask는 렌더링을 블로킹하는 위험성, await 이후 코드가 microtask로 스케줄링되는 원리와 Promise.all() 병렬 처리의 성능 이점, 대량 데이터 처리 시 chunk 분할 + virtual scrolling + Web Worker + 디바운스 조합 전략
- **다음 추천**: Closure & Scope

### 세션 #2 - 2026-03-10
- **모드**: CS학습
- **토픽**: Hash Table
- **도달 난이도**: Level 4
- **판정**: 통과
- **핵심 깨달음**: Hash function이 key를 인덱스로 변환하는 원리, Chaining vs Open Addressing의 cache locality 기반 트레이드오프, Load factor와 rehashing 개념, randomized hashing으로 hash flooding 공격 방어, Consistent Hashing의 hash ring 원리와 가상 노드를 통한 균등 분산
- **다음 추천**: Tree (BST, AVL)

### 세션 #1 - 2026-03-09
- **모드**: CS학습
- **토픽**: Array & Linked List
- **도달 난이도**: Level 4
- **판정**: 통과
- **핵심 깨달음**: Array의 주소 계산 공식(시작주소 + 자료형크기 * 인덱스)으로 O(1) 접근 원리 이해, cache locality와 데이터 크기에 따른 Array vs Linked List 선택 기준 발견, amortized O(1)의 원리를 2배 확장 vs 1칸 확장 비교로 체감, HashMap + Doubly Linked List 조합으로 LRU Cache 설계
- **다음 추천**: Hash Table

<!-- 형식:
### 세션 #N - YYYY-MM-DD
- **모드**: CS학습 / 테마드릴 / 디버깅 / 복습
- **토픽**: 토픽명
- **도달 난이도**: Level 1-4
- **판정**: 통과 / 학습중
- **핵심 깨달음**: 학습자가 스스로 발견한 포인트
- **다음 추천**: 다음에 공부하면 좋을 토픽
-->

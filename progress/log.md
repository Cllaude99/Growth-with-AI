# 학습 진행 기록

## 요약 통계

| 항목 | 값 |
|------|-----|
| 총 세션 수 | 3 |
| 다룬 토픽 수 | 3 / 52 |
| 연속 학습일 | 3일 |
| 마지막 학습일 | 2026-03-12 |

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

### 네트워크 (0/7)

| 토픽 | 상태 | 도달 난이도 | 마지막 학습일 |
|------|------|------------|-------------|
| TCP & UDP | - | - | - |
| HTTP & HTTPS | - | - | - |
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

### 웹 & 프론트엔드 (1/8)

| 토픽 | 상태 | 도달 난이도 | 마지막 학습일 |
|------|------|------------|-------------|
| Event Loop & 비동기 | 통과 | Level 4 | 2026-03-12 |
| Closure & Scope | - | - | - |
| Prototype & Inheritance | - | - | - |
| Browser Rendering Pipeline | - | - | - |
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

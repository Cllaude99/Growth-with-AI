# Deadlock

## 메타데이터
- **카테고리**: 운영체제
- **난이도**: 고급
- **우선순위**: 높음
- **선수과목**: Synchronization
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Deadlock의 정의는 무엇이며, 발생하기 위한 4가지 필요조건은 무엇인가?

### 꼬리 질문
- 4가지 조건 중 하나라도 성립하지 않으면 Deadlock이 발생하지 않는다는 것을 간단한 예시로 설명하라.
- Deadlock과 Livelock, Starvation은 각각 어떻게 다른가?

### 예상 이해 수준
Deadlock은 두 개 이상의 Process가 서로 상대방이 보유한 자원을 기다리며 영원히 진행되지 못하는 상태다. 4가지 필요조건은 Mutual exclusion(자원은 한 번에 하나의 Process만 사용), Hold and wait(자원을 보유한 채 다른 자원을 기다림), No preemption(자원을 강제로 빼앗을 수 없음), Circular wait(Process 간 자원 대기 사이클 존재)이다. Livelock은 Process들이 계속 상태를 변경하지만 전진하지 못하는 상태이며, Starvation은 특정 Process가 자원을 영원히 할당받지 못하는 현상이라는 것을 명확히 구분할 수 있어야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- Deadlock Prevention, Avoidance, Detection & Recovery의 차이는 무엇이며, 각각의 트레이드오프는 어떠한가?

### 꼬리 질문
- Deadlock Prevention에서 Circular wait 조건을 없애기 위해 자원에 순서를 부여하는 방식의 한계는 무엇인가?
- Deadlock Detection & Recovery에서 어떤 Process를 victim으로 선택하는 기준은 무엇인가?

### 예상 이해 수준
Prevention은 4가지 조건 중 하나를 아예 성립하지 않게 설계하는 방식이다. 예를 들어 Hold and wait를 없애려면 Process가 시작 전에 모든 자원을 한꺼번에 요청해야 하는데, 이는 자원 활용률을 크게 낮춘다. Avoidance는 Banker's algorithm처럼 자원 요청 시 안전 상태 여부를 검사해 unsafe state로의 진입을 막지만, 최대 필요 자원을 미리 알아야 하는 제약이 있다. Detection & Recovery는 자유롭게 자원을 할당하다가 Deadlock 발생 후 감지하고 복구하는 방식으로 오버헤드가 낮지만 복구 비용이 크다. 시스템의 특성과 요구사항에 따라 전략을 선택해야 한다는 것을 설명할 수 있어야 한다.

## Level 3: 심화

### 핵심 질문
- Banker's algorithm은 어떻게 안전 상태를 판단하며, Resource allocation graph에서 Deadlock을 어떻게 탐지하는가?

### 꼬리 질문
- Banker's algorithm에서 "안전 순서열"이 존재한다는 것은 무엇을 의미하는가?
- Resource allocation graph에서 단일 인스턴스 자원과 다중 인스턴스 자원의 Deadlock 탐지 방법이 어떻게 다른가?

### 예상 이해 수준
Banker's algorithm은 자원 요청 시 요청을 허용했을 때의 상태를 시뮬레이션하고, 모든 Process가 최대 필요량을 충족할 수 있는 실행 순서(안전 순서열)가 존재하면 안전 상태로 판단해 요청을 허용한다. Resource allocation graph에서 단일 인스턴스 자원의 경우 사이클이 존재하면 Deadlock이다. 다중 인스턴스 자원의 경우 사이클이 있어도 Deadlock이 아닐 수 있으며, 할당/요청 행렬을 이용한 Wait-for graph나 행렬 기반 알고리즘으로 탐지한다. 알고리즘의 시간 복잡도와 실용적 한계를 논할 수 있어야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- 분산 시스템에서 Deadlock은 단일 노드와 어떻게 다르며, 이를 탐지하고 해결하는 방법은 무엇인가?

### 꼬리 질문
- 분산 Deadlock 탐지에서 Edge chasing 알고리즘과 Centralized 방식의 트레이드오프는 무엇인가?
- Microservice 환경에서 여러 서비스가 서로의 API를 호출할 때 Deadlock과 유사한 상황이 발생할 수 있는가?

### 예상 이해 수준
분산 시스템에서는 각 노드가 부분적인 자원 할당 정보만 가지고 있어 전역 상태를 파악하기 어렵다. Centralized 방식은 중앙 조정자가 전체 Wait-for graph를 관리하지만 단일 장애점이 된다. Edge chasing(Chandy-Misra-Haas 알고리즘)은 탐침(probe) 메시지를 보내 사이클을 탐지하는 분산 방식이다. 분산 환경에서는 Deadlock을 완전히 방지하기 어려우므로, timeout 기반 탐지와 retry 전략, 혹은 낙관적 동시성 제어(Optimistic concurrency control)로 실용적으로 처리하는 방법을 논할 수 있으면 우수하다.

## 현실 연결
- RDBMS(MySQL, PostgreSQL)는 트랜잭션 간 Deadlock을 자동으로 탐지하고, 더 적은 비용의 트랜잭션을 rollback하여 복구한다.
- Dining philosophers 문제는 Deadlock의 고전적 모델로, 철학자들이 양쪽 젓가락을 모두 잡으려 할 때 발생하며, 홀수 철학자는 왼쪽 먼저, 짝수는 오른쪽 먼저 잡는 순서 부여로 해결한다.
- 분산 시스템의 distributed lock(Redis의 Redlock, ZooKeeper)은 여러 노드에 걸친 자원 경쟁 시 Deadlock을 방지하기 위해 TTL(Time-To-Live)과 lease 기반 자동 해제 메커니즘을 사용한다.

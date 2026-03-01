# Transaction and ACID

## 메타데이터
- **카테고리**: 데이터베이스
- **난이도**: 중급
- **우선순위**: 높음
- **선수과목**: 없음
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Transaction이란 무엇이고, ACID 속성 각각은 어떤 의미인가?

### 꼬리 질문
- Atomicity가 보장되지 않으면 어떤 문제가 발생하는가?
- Durability는 어떻게 구현되는가?

### 예상 이해 수준
Transaction은 여러 작업을 하나의 논리적 단위로 묶어 모두 성공하거나 모두 실패하도록 보장하는 메커니즘이다. Atomicity(원자성), Consistency(일관성), Isolation(격리성), Durability(지속성) 각각의 의미를 실제 예시(은행 송금)로 설명할 수 있어야 한다. Durability는 WAL(Write-Ahead Log)로 구현됨을 알면 좋다.

## Level 2: 트레이드오프

### 핵심 질문
- Isolation level 4가지(Read Uncommitted, Read Committed, Repeatable Read, Serializable)의 트레이드오프는 무엇인가?

### 꼬리 질문
- Isolation level을 높일수록 어떤 비용이 발생하는가?
- MySQL InnoDB의 기본 Isolation level은 무엇이고, 왜 그것을 선택했는가?

### 예상 이해 수준
Isolation level이 낮을수록 동시성(throughput)이 높고, 높을수록 데이터 정합성이 보장된다. Read Uncommitted는 dirty read가 발생하고, Read Committed는 non-repeatable read가 발생하며, Repeatable Read는 phantom read가 발생할 수 있다. Serializable은 모든 문제를 방지하지만 성능이 가장 낮다. MySQL InnoDB 기본값은 Repeatable Read이며, MVCC로 phantom read를 부분적으로 방지한다.

## Level 3: 심화

### 핵심 질문
- Dirty read, Non-repeatable read, Phantom read의 차이를 구체적인 시나리오로 설명하라.

### 꼬리 질문
- MVCC(Multi-Version Concurrency Control)는 어떻게 이 문제들을 해결하는가?
- Optimistic locking과 Pessimistic locking의 차이는 무엇이고 언제 각각을 사용하는가?

### 예상 이해 수준
Dirty read는 commit되지 않은 데이터를 읽는 현상, Non-repeatable read는 같은 row를 두 번 읽었을 때 다른 값이 나오는 현상, Phantom read는 같은 조건의 query가 다른 row 집합을 반환하는 현상이다. MVCC는 데이터의 여러 버전을 유지해 읽기 작업이 잠금 없이 일관된 스냅샷을 볼 수 있게 한다. Optimistic locking은 충돌이 드물 때, Pessimistic locking은 충돌이 빈번할 때 적합하다.

## Level 4: 시스템 설계

### 핵심 질문
- 마이크로서비스 환경에서 여러 서비스에 걸친 분산 트랜잭션을 어떻게 구현할 것인가?

### 꼬리 질문
- 2PC(Two-Phase Commit)의 한계는 무엇이고, Saga pattern은 어떻게 이를 보완하는가?
- Saga pattern에서 보상 트랜잭션(compensating transaction)이란 무엇인가?

### 예상 이해 수준
단일 DB에서의 ACID는 마이크로서비스 환경에서 보장하기 어렵다. 2PC는 모든 참여자가 동의해야 commit하는 방식으로 가용성이 낮고 coordinator 장애에 취약하다. Saga pattern은 각 서비스가 로컬 트랜잭션을 처리하고 실패 시 보상 트랜잭션으로 롤백하는 방식으로, Choreography(이벤트 기반)와 Orchestration(중앙 조율) 방식으로 구현된다.

## 현실 연결
- 은행 계좌 이체: A 계좌 출금과 B 계좌 입금이 Atomic하게 처리되어야 하며, 중간에 장애가 나도 데이터 정합성이 유지되어야 한다
- 재고 관리: 주문 생성, 재고 차감, 결제 처리가 하나의 Transaction으로 묶여야 overselling이 방지된다
- Spring @Transactional: 프록시 기반으로 동작하며, 같은 클래스 내 메서드 호출 시 Transaction이 적용되지 않는 self-invocation 문제를 주의해야 한다

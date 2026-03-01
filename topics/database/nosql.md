# NoSQL

## 메타데이터
- **카테고리**: 데이터베이스
- **난이도**: 중급
- **우선순위**: 중간
- **선수과목**: Transaction & ACID
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- NoSQL의 4가지 유형(Document, Key-Value, Column-family, Graph)은 각각 어떤 특성을 가지며 어떤 상황에 적합한가?

### 꼬리 질문
- NoSQL이 SQL보다 "빠르다"는 말이 항상 옳은가? 어떤 맥락에서 이야기해야 하는가?
- Column-family DB(예: Cassandra)가 Row 기반 RDBMS와 다른 점은 무엇인가?

### 예상 이해 수준
Document DB(MongoDB)는 JSON 형태의 유연한 스키마로 계층 데이터에 적합하다. Key-Value DB(Redis, DynamoDB)는 단순한 조회에서 O(1) 성능을 제공한다. Column-family DB(Cassandra)는 쓰기가 매우 빠르고 시계열 데이터에 유리하다. Graph DB(Neo4j)는 관계 중심 탐색에 강하다. 각 유형의 대표 제품과 적합한 유스케이스를 연결할 수 있어야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- SQL과 NoSQL 중 무엇을 선택해야 하는가? 결정 기준은 무엇인가?

### 꼬리 질문
- 스키마가 자주 바뀌는 초기 스타트업에서 NoSQL이 유리한 이유와 위험 요소는 무엇인가?
- 강력한 ACID 트랜잭션이 필요한 금융 서비스에서 NoSQL을 사용하면 어떤 문제가 생기는가?

### 예상 이해 수준
SQL은 복잡한 JOIN, 강력한 ACID, 엄격한 스키마가 필요할 때 적합하다. NoSQL은 수평 확장(horizontal scaling), 유연한 스키마, 대규모 읽기/쓰기 처리량이 필요할 때 유리하다. NoSQL이 JOIN을 지원하지 않는 경우 application 레벨에서 처리해야 하며 이로 인한 복잡성과 데이터 일관성 문제를 이해해야 한다.

## Level 3: 심화

### 핵심 질문
- Eventual consistency란 무엇이고, BASE 속성은 ACID와 어떻게 다른가?

### 꼬리 질문
- CAP theorem에서 NoSQL은 일반적으로 어떤 두 가지를 선택하는가? 그 의미는 무엇인가?
- Eventual consistency를 사용하는 시스템에서 데이터가 일시적으로 불일치할 때 사용자 경험을 어떻게 관리하는가?

### 예상 이해 수준
BASE는 Basically Available, Soft state, Eventually consistent의 약자로 ACID의 강한 일관성 대신 가용성과 성능을 우선시한다. Eventual consistency는 모든 복제본이 결국 동일한 상태에 도달함을 보장하지만 특정 시점에 일시적 불일치를 허용한다. CAP theorem에서 대부분의 NoSQL은 AP(Availability + Partition tolerance)를 선택하며 이는 네트워크 장애 시 오래된 데이터를 반환할 수 있음을 의미한다.

## Level 4: 시스템 설계

### 핵심 질문
- 대규모 서비스에서 여러 종류의 DB를 목적에 맞게 혼용하는 Polyglot persistence를 어떻게 설계할 것인가?

### 꼬리 질문
- 각 DB 간 데이터 동기화는 어떻게 처리하는가? 정합성 문제가 생기면 어떻게 해결하는가?
- Redis를 캐시로 사용할 때 Cache invalidation 전략에는 어떤 것들이 있는가?

### 예상 이해 수준
사용자 세션은 Redis(Key-Value), 상품 카탈로그는 MongoDB(Document), 거래 데이터는 PostgreSQL(RDBMS), 추천 그래프는 Neo4j(Graph)처럼 용도에 맞는 DB를 선택한다. 데이터 동기화는 이벤트 기반(CDC, Kafka)으로 처리하고, 각 DB의 데이터 정합성 수준을 비즈니스 요구사항에 맞게 결정한다. Cache aside, Write-through, Write-behind 전략의 트레이드오프를 설명할 수 있어야 한다.

## 현실 연결
- MongoDB: 상품 상세 정보처럼 필드가 상품마다 다른 경우 유연한 Document 구조로 저장한다
- Redis: 세션 저장, 실시간 순위표(Sorted Set), 분산 락, 캐시 레이어로 광범위하게 활용된다
- Cassandra: 수십억 건의 시계열 IoT 데이터나 메시지 로그를 높은 쓰기 처리량으로 저장한다
- Neo4j: 소셜 네트워크의 친구 추천, 사기 탐지에서 관계 기반 탐색 query에 강점을 보인다
- DynamoDB: AWS의 완전 관리형 Key-Value/Document DB로, 트래픽 급증에도 자동 확장되는 서버리스 서비스에 적합하다

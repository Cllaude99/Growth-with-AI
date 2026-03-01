# CAP Theorem

## 메타데이터
- **카테고리**: 시스템 설계
- **난이도**: 고급
- **우선순위**: 중간
- **선수과목**: NoSQL
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- CAP theorem의 세 가지 속성인 Consistency, Availability, Partition tolerance를 각각 설명해주세요. 왜 분산 시스템에서는 세 가지를 동시에 만족할 수 없나요?

### 꼬리 질문
- Partition tolerance는 왜 분산 시스템에서 사실상 포기할 수 없는 속성인가요?
- CAP theorem에서 말하는 Consistency는 ACID의 Consistency와 같은 개념인가요?

### 예상 이해 수준
Consistency(모든 노드가 같은 시점에 같은 데이터를 보여줌), Availability(모든 요청이 응답을 받음), Partition tolerance(네트워크 분리 상황에서도 시스템이 동작함)를 설명할 수 있어야 합니다. 네트워크 파티션은 현실에서 반드시 발생하므로 P를 포기할 수 없고, 결국 C와 A 중 하나를 선택해야 함을 설명할 수 있어야 합니다. CAP의 Consistency는 linearizability를 의미하며 ACID의 Consistency와 다름을 구분할 수 있어야 합니다.

## Level 2: 트레이드오프

### 핵심 질문
- CP 시스템과 AP 시스템의 트레이드오프를 설명하고, 각각의 실제 예시를 들어주세요. 어떤 서비스 요구사항에서 CP를 선택하고 어떤 경우에 AP를 선택하나요?

### 꼬리 질문
- 금융 결제 시스템과 소셜 미디어 좋아요 기능은 각각 CP와 AP 중 어느 쪽이 더 적합한가요? 그 이유는 무엇인가요?
- AP 시스템에서 네트워크 파티션이 복구된 후 서로 다른 노드에 쌓인 충돌하는 데이터를 어떻게 해결하나요?

### 예상 이해 수준
CP 시스템(ZooKeeper, HBase)은 파티션 상황에서 일관성을 위해 일부 요청을 거부하거나 대기시키고, AP 시스템(Cassandra, DynamoDB)은 파티션 상황에서도 가용성을 유지하지만 일시적으로 데이터 불일치가 발생할 수 있음을 설명할 수 있어야 합니다. 비즈니스 요구사항(데이터 정확성 vs. 서비스 지속성)에 따라 선택 기준을 제시할 수 있어야 합니다.

## Level 3: 심화

### 핵심 질문
- PACELC theorem이 CAP theorem을 어떻게 보완하나요? Eventual consistency는 실제로 어떻게 동작하며, 어떤 문제를 야기할 수 있나요?

### 꼬리 질문
- Eventual consistency 환경에서 사용자가 자신이 방금 올린 게시물이 보이지 않는 "read-your-own-writes" 문제를 어떻게 해결하나요?
- Cassandra에서 consistency level을 QUORUM으로 설정하면 CAP theorem의 어느 지점에 위치하게 되나요?

### 예상 이해 수준
PACELC는 파티션이 없는 정상 상황에서도 latency와 consistency 사이의 트레이드오프가 존재함을 설명함을 이해할 수 있어야 합니다. Eventual consistency는 모든 업데이트가 결국 모든 노드에 전파되지만 그 시점까지 불일치가 허용됨을 설명하고, read-your-own-writes, monotonic reads 등의 보장 수준(consistency model)을 구분할 수 있어야 합니다.

## Level 4: 시스템 설계

### 핵심 질문
- 글로벌 전자상거래 플랫폼을 설계한다고 할 때, 재고 관리, 장바구니, 상품 조회 기능 각각에 대해 CAP 속성 중 어떤 것을 우선시할지 결정하고 그 이유를 설명해주세요.

### 꼬리 질문
- 재고가 1개 남은 상품을 두 사용자가 동시에 구매하려는 경우, AP 시스템에서 이를 어떻게 처리할 수 있나요?
- 서로 다른 일관성 요구사항을 가진 기능들을 하나의 시스템 안에서 어떻게 공존시킬 수 있나요?

### 예상 이해 수준
상품 조회는 AP(높은 가용성, 오래된 데이터 허용), 장바구니는 AP(사용자 세션 범위의 일관성), 재고 차감은 CP(정확한 수량 보장)로 구분하고, 각 결정의 근거를 비즈니스 관점에서 설명할 수 있어야 합니다. 한 트랜잭션 내에서 여러 일관성 모델이 공존하는 설계와 이를 구현하는 기술적 접근(optimistic locking, saga pattern 등)을 제시할 수 있어야 합니다.

## 현실 연결
- Amazon DynamoDB(AP 시스템)의 eventually consistent read와 strongly consistent read 옵션 선택
- Apache ZooKeeper(CP 시스템)를 분산 락, 리더 선출, 설정 관리에 사용하는 사례
- Apache Cassandra의 tunable consistency와 MongoDB의 replica set read preference를 통한 유연한 일관성 수준 조절

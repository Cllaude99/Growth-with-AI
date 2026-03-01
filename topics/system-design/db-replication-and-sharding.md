# DB Replication and Sharding

## 메타데이터
- **카테고리**: 시스템 설계
- **난이도**: 고급
- **우선순위**: 중간
- **선수과목**: Transaction & ACID
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Replication과 sharding은 각각 무엇이고, 어떤 문제를 해결하기 위해 사용하나요? 두 기법의 근본적인 차이는 무엇인가요?

### 꼬리 질문
- 데이터를 복제하면(replication) 스토리지 비용이 늘어나는데도 사용하는 이유는 무엇인가요?
- Sharding을 하지 않고 단일 DB 서버의 용량만 계속 늘리면(vertical scaling) 어떤 한계에 부딪히나요?

### 예상 이해 수준
Replication은 동일한 데이터를 여러 노드에 복사해 읽기 처리량 향상과 고가용성을 달성하고, Sharding은 데이터를 분할해 여러 노드에 나눠 저장함으로써 쓰기 처리량과 저장 용량을 수평 확장함을 설명할 수 있어야 합니다. 두 기법이 상호 배타적이지 않고 함께 사용할 수 있음도 이해해야 합니다.

## Level 2: 트레이드오프

### 핵심 질문
- Master-slave replication과 multi-master replication의 차이점과 트레이드오프를 설명해주세요. 각각 어떤 상황에서 적합한가요?

### 꼬리 질문
- Master-slave 구성에서 master가 장애를 일으키면 어떤 일이 발생하고, 자동 failover는 어떻게 구현하나요?
- Multi-master 구성에서 두 master에 동시에 같은 레코드를 수정하면 어떻게 충돌을 해결하나요?

### 예상 이해 수준
Master-slave는 쓰기가 master에서만 발생하고 slave는 읽기만 처리해 충돌 문제가 없지만 master가 병목이 될 수 있음을 설명할 수 있어야 합니다. Multi-master는 여러 노드에서 쓰기가 가능해 가용성과 쓰기 처리량이 높지만 write conflict 해결이 필요하고 구현이 복잡함을 비교할 수 있어야 합니다. Replication lag으로 인한 stale read 문제도 설명할 수 있어야 합니다.

## Level 3: 심화

### 핵심 질문
- Sharding key를 어떻게 선택해야 하나요? Hotspot 문제가 발생하는 원인과 해결 방법, 그리고 resharding 시 발생하는 어려움을 설명해주세요.

### 꼬리 질문
- 사용자 ID를 sharding key로 사용할 때와 지역을 sharding key로 사용할 때 각각 어떤 장단점이 있나요?
- 데이터가 늘어나 shard를 추가해야 할 때 기존 데이터를 재분배하는 과정에서 서비스 중단 없이 진행하려면 어떻게 해야 하나요?

### 예상 이해 수준
좋은 sharding key는 데이터를 균등하게 분산하고 쿼리 패턴에 맞게 locality를 제공해야 함을 설명할 수 있어야 합니다. 시간 기반 sharding key나 인기 사용자(celebrity problem)로 인한 hotspot 문제와 consistent hashing을 통한 해결 방법을 설명할 수 있어야 합니다. Resharding 시 데이터 이동 중 발생하는 이중 쓰기, 서비스 지속성 유지 전략도 설명할 수 있어야 합니다.

## Level 4: 시스템 설계

### 핵심 질문
- 수억 명의 사용자를 보유한 글로벌 소셜 미디어 서비스의 데이터 분산 전략을 설계해주세요. 사용자 데이터, 게시물, 팔로우 관계, 피드 데이터를 어떻게 저장하고 분산하나요?

### 꼬리 질문
- 팔로워가 수천만 명인 인플루언서가 게시물을 올릴 때 피드 fanout을 어떻게 처리하나요? (push vs pull 모델)
- 지역별로 데이터를 분리해야 하는 GDPR 등의 규제 요구사항을 분산 DB 설계에서 어떻게 수용하나요?

### 예상 이해 수준
사용자 프로필은 user_id 기반 sharding, 게시물은 post_id 또는 user_id 기반 sharding, 팔로우 관계는 그래프 DB 또는 adjacency list 방식으로 각 데이터 특성에 맞는 분산 전략을 제시할 수 있어야 합니다. 각 리전의 replica를 두어 읽기 latency를 낮추고, 규제 요구사항에 따라 데이터 residency를 보장하는 설계를 고려할 수 있어야 합니다.

## 현실 연결
- MySQL Group Replication과 Vitess를 활용한 YouTube의 대규모 MySQL 수평 확장 사례
- MongoDB Sharded Cluster를 통한 도큐먼트 데이터의 자동 분산 및 zone sharding으로 지역별 데이터 격리
- CockroachDB와 Google Spanner의 분산 트랜잭션 지원과 글로벌 데이터 분산 아키텍처

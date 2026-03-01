# Indexing

## 메타데이터
- **카테고리**: 데이터베이스
- **난이도**: 중급
- **우선순위**: 최고
- **선수과목**: Tree
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Database index란 무엇이고, 왜 필요한가?

### 꼬리 질문
- Index가 없는 경우 데이터베이스는 어떻게 데이터를 조회하는가?
- Index를 많이 만들면 좋은 것 아닌가? 어떤 단점이 있는가?

### 예상 이해 수준
Index가 책의 목차처럼 데이터 위치를 빠르게 찾아주는 자료구조임을 설명할 수 있어야 한다. Index 없이는 Full table scan이 발생하며 O(n) 시간이 걸린다는 것, Index는 별도 공간을 차지하고 INSERT/UPDATE/DELETE 시 overhead가 발생한다는 트레이드오프를 알아야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- B-Tree index와 Hash index의 트레이드오프는 무엇인가?

### 꼬리 질문
- 범위 검색(BETWEEN, >, <)이 많다면 어떤 index를 선택해야 하는가?
- Hash index가 B-Tree보다 빠른 경우는 언제인가?

### 예상 이해 수준
B-Tree는 정렬된 구조로 범위 검색과 ORDER BY에 효율적이며 O(log n) 시간복잡도를 가진다. Hash index는 equality 검색에서 O(1)이지만 범위 검색이 불가능하고 메모리를 많이 사용한다. MySQL InnoDB는 기본적으로 B-Tree를 사용하며 Hash index는 Memory 엔진에서만 지원됨을 설명할 수 있어야 한다.

## Level 3: 심화

### 핵심 질문
- Composite index에서 column 순서가 왜 중요한가? Covering index란 무엇인가?

### 꼬리 질문
- Index selectivity가 낮은 column에 index를 걸면 어떤 문제가 발생하는가?
- (a, b, c) composite index에서 b, c만 조건으로 쓰면 index를 타는가?

### 예상 이해 수준
Composite index는 leftmost prefix rule을 따르므로 첫 번째 column 기준으로 정렬되어야 한다. Covering index는 query에 필요한 모든 column이 index에 포함되어 실제 row lookup이 필요 없는 상태다. Index selectivity는 cardinality/total rows로, selectivity가 낮으면 optimizer가 Full table scan을 선택할 수 있음을 알아야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- 월간 활성 사용자 1000만 명 규모의 서비스에서 인덱싱 전략을 어떻게 설계할 것인가?

### 꼬리 질문
- 쓰기가 많은 서비스(예: 로그 수집)에서 index 전략은 어떻게 달라지는가?
- Index 추가 없이 읽기 성능을 개선할 수 있는 다른 방법에는 무엇이 있는가?

### 예상 이해 수준
실제 query pattern 분석(slow query log)을 기반으로 index를 설계해야 한다. 읽기가 많으면 covering index를 적극 활용하고, 쓰기가 많으면 index 수를 최소화한다. Partial index(조건부 index), Functional index, Index를 활용한 pagination 최적화(keyset pagination)까지 설명할 수 있어야 한다.

## 현실 연결
- MySQL InnoDB에서 Primary key는 Clustered index로, 모든 Secondary index는 Primary key 값을 포함한다
- PostgreSQL은 B-Tree 외에 GIN(전문 검색), GiST(지리 데이터), BRIN(범위 기반) 등 다양한 index type을 지원한다
- EXPLAIN 또는 EXPLAIN ANALYZE로 query plan을 확인하여 index scan vs Full table scan 여부를 판단할 수 있다

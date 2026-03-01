# Query Optimization

## 메타데이터
- **카테고리**: 데이터베이스
- **난이도**: 고급
- **우선순위**: 중간
- **선수과목**: Indexing, SQL Join
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Query execution plan이란 무엇이고, EXPLAIN 결과에서 어떤 정보를 읽어야 하는가?

### 꼬리 질문
- EXPLAIN과 EXPLAIN ANALYZE의 차이는 무엇인가?
- Query execution plan에서 "cost"는 무엇을 의미하는가?

### 예상 이해 수준
Query execution plan은 데이터베이스 optimizer가 query를 실행하는 방식을 단계별로 보여주는 트리 구조다. EXPLAIN은 예상 실행 계획을, EXPLAIN ANALYZE는 실제 실행 통계(실제 row 수, 실행 시간)를 함께 보여준다. seq_scan(Full table scan), index_scan, rows, cost 등의 핵심 항목을 읽고 병목을 식별할 수 있어야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- Optimizer가 Index scan 대신 Full table scan을 선택하는 기준은 무엇인가?

### 꼬리 질문
- Index가 존재하는데도 사용되지 않는 경우는 어떤 상황인가?
- Index scan이 오히려 Full table scan보다 느릴 수 있는 경우는 언제인가?

### 예상 이해 수준
Optimizer는 통계 정보(cardinality, selectivity)를 기반으로 실행 계획을 결정한다. Index selectivity가 낮아 전체 데이터의 20-30% 이상을 읽어야 한다면 Full table scan이 더 효율적일 수 있다. Index scan은 random I/O를 유발하므로 반환 row가 많을수록 sequential scan보다 느려진다. 함수로 감싼 컬럼(WHERE YEAR(created_at) = 2024), 암묵적 형변환, LIKE '%keyword' 패턴은 index를 타지 못한다.

## Level 3: 심화

### 핵심 질문
- Query rewriting, Partition pruning, Materialized view는 각각 어떻게 query 성능을 개선하는가?

### 꼬리 질문
- Partition pruning이 동작하려면 query가 어떤 조건을 만족해야 하는가?
- Materialized view의 갱신 전략(즉시 갱신 vs 주기적 갱신)의 트레이드오프는 무엇인가?

### 예상 이해 수준
Query rewriting은 의미는 같지만 optimizer가 더 효율적으로 처리할 수 있는 형태로 query를 변환하는 기법이다(예: IN 대신 EXISTS, subquery 대신 JOIN). Partition pruning은 테이블을 날짜나 범위 기준으로 파티션 분리 후, 조건에 해당하는 파티션만 스캔하는 최적화다. Materialized view는 집계나 복잡한 JOIN 결과를 미리 계산해 저장하여 조회 속도를 높이지만, 원본 데이터 변경 시 동기화 비용이 발생한다.

## Level 4: 시스템 설계

### 핵심 질문
- 수억 건 이상의 데이터를 다루는 시스템에서 slow query를 근본적으로 해결하기 위한 전략을 설계하라.

### 꼬리 질문
- Slow query log 기반으로 개선 우선순위를 어떻게 결정하는가?
- Query 최적화만으로 해결되지 않는 경우 어떤 아키텍처 수준의 접근을 고려할 수 있는가?

### 예상 이해 수준
Slow query log 분석으로 임계치 초과 query를 식별하고, EXPLAIN ANALYZE로 bottleneck을 진단한 뒤 index 추가, query 재작성, 파티셔닝을 순서대로 시도한다. 그래도 해결이 안 된다면 Read replica로 읽기 부하를 분산하거나, 자주 조회되는 결과를 Redis에 캐싱하거나, OLTP 데이터를 Data warehouse로 복제하여 분석 query를 분리하는 아키텍처를 고려한다. Query optimizer의 통계가 오래된 경우 ANALYZE 또는 VACUUM으로 갱신해야 한다.

## 현실 연결
- EXPLAIN ANALYZE: PostgreSQL에서 실제 실행 계획과 소요 시간을 출력하여 예상 plan과 실제 plan의 차이를 확인할 수 있다
- Slow query log: MySQL에서 long_query_time 이상 걸린 query를 로그로 남겨 주기적으로 분석하고 개선 작업을 수행한다
- MySQL query optimizer: 통계 기반으로 실행 계획을 결정하며, FORCE INDEX 힌트로 강제 지정하거나 optimizer_switch로 특정 최적화를 제어할 수 있다

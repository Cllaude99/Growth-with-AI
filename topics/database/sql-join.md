# SQL Join

## 메타데이터
- **카테고리**: 데이터베이스
- **난이도**: 기초
- **우선순위**: 높음
- **선수과목**: 없음
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- JOIN의 종류(INNER, LEFT, RIGHT, FULL OUTER)와 각각의 동작 방식은 무엇인가?

### 꼬리 질문
- INNER JOIN과 LEFT JOIN의 결과가 달라지는 경우는 언제인가?
- NULL 값이 JOIN 결과에 미치는 영향은 무엇인가?

### 예상 이해 수준
INNER JOIN은 양쪽 테이블에 매칭되는 row만 반환하고, LEFT JOIN은 왼쪽 테이블의 모든 row를 유지하며 매칭되지 않는 경우 NULL을 채운다. FULL OUTER JOIN은 양쪽 모두를 보존한다. Venn diagram으로 시각적으로 설명할 수 있어야 하며, 각 JOIN이 생성하는 row 수와 NULL 처리 방식을 알아야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- JOIN 성능을 결정하는 요소는 무엇이고, 언제 JOIN을 피해야 하는가?

### 꼬리 질문
- JOIN 알고리즘(Nested Loop, Hash Join, Merge Join)은 각각 어떤 상황에서 유리한가?
- 테이블이 매우 크고 JOIN 조건에 index가 없다면 어떤 문제가 발생하는가?

### 예상 이해 수준
JOIN 성능은 테이블 크기, JOIN 조건의 index 유무, 반환 row 수에 따라 크게 달라진다. Nested Loop Join은 작은 테이블에, Hash Join은 큰 테이블 간 equality join에, Merge Join은 이미 정렬된 데이터에 유리하다. 수천만 건 이상의 테이블을 JOIN할 때는 application 레벨에서 분리하거나 denormalization을 고려해야 한다.

## Level 3: 심화

### 핵심 질문
- Self join, Cross join, Anti join 패턴은 각각 어떤 문제를 해결하는가?

### 꼬리 질문
- Anti join을 NOT IN, NOT EXISTS, LEFT JOIN ... WHERE IS NULL로 각각 구현할 때 차이점은 무엇인가?
- 계층 데이터(트리 구조)를 SQL로 조회하는 방법에는 무엇이 있는가?

### 예상 이해 수준
Self join은 같은 테이블을 두 번 참조하여 계층 관계나 비교를 표현한다(예: 직원과 매니저). Cross join은 Cartesian product로 모든 조합을 생성한다. Anti join은 한 테이블에는 있지만 다른 테이블에는 없는 데이터를 찾는 패턴이다. NOT IN은 NULL이 있으면 예상치 못한 결과가 발생하므로 NOT EXISTS나 LEFT JOIN이 더 안전하다.

## Level 4: 시스템 설계

### 핵심 질문
- JOIN 성능 문제를 근본적으로 해결하기 위한 Denormalization 전략을 어떻게 설계할 것인가?

### 꼬리 질문
- Denormalization이 데이터 일관성에 미치는 영향은 무엇이고 어떻게 관리하는가?
- CQRS 패턴에서 읽기 모델을 어떻게 설계하면 JOIN 없이 복잡한 데이터를 조회할 수 있는가?

### 예상 이해 수준
Denormalization은 읽기 성능을 위해 중복 데이터를 허용하는 전략으로, 쓰기 시 일관성 유지 비용이 발생한다. Data warehouse의 Star schema는 Fact 테이블과 Dimension 테이블을 분리해 OLAP query를 최적화한다. Materialized view나 이벤트 기반 read model 업데이트를 통해 JOIN 없는 조회를 설계할 수 있다.

## 현실 연결
- ORM의 N+1 문제: 게시글 목록 조회 후 각 게시글의 작성자를 별도 query로 조회하는 패턴으로, JOIN 또는 eager loading으로 해결한다
- Data warehouse의 Star schema: 주문(Fact) 테이블과 고객/상품/날짜(Dimension) 테이블을 JOIN하여 분석 query를 수행한다
- ETL pipeline: 여러 소스 데이터를 JOIN하여 통합된 데이터 모델을 만드는 과정에서 대용량 JOIN 최적화가 핵심이다

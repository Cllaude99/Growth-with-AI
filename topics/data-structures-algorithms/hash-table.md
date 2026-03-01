# Hash Table

## 메타데이터
- **카테고리**: 자료구조 & 알고리즘
- **난이도**: 중급
- **우선순위**: 최고
- **선수과목**: Array & Linked List
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Hash table이 key-value 쌍을 어떻게 저장하고 조회하는지, hash function의 역할과 함께 내부 동작을 설명해주세요.

### 꼬리 질문
- 좋은 hash function이 갖춰야 할 조건은 무엇인가요?
- Hash table에서 평균 O(1) 조회가 가능한 이유는 무엇이고, 그 전제 조건은 무엇인가요?

### 예상 이해 수준
Hash function이 key를 받아 배열의 인덱스를 반환하고, 그 인덱스에 value를 저장하는 방식을 설명할 수 있어야 한다. 좋은 hash function의 특성으로 균등 분포(uniform distribution), 결정론적(deterministic), 빠른 계산 속도를 언급할 수 있어야 한다. Collision이 없다면 O(1) 평균 성능이 가능하지만 collision이 발생하면 성능이 저하된다는 점을 이해해야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- Hash collision을 해결하는 두 가지 주요 방법인 Separate Chaining과 Open Addressing의 동작 방식과 각각의 장단점을 비교해주세요.

### 꼬리 질문
- Open Addressing에서 Linear probing, Quadratic probing, Double hashing의 차이는 무엇이고, 각각 어떤 clustering 문제를 해결하려 하나요?
- Separate Chaining 방식에서 최악의 경우 모든 key가 같은 bucket에 몰린다면 성능이 어떻게 변하나요? 이를 방지하기 위해 어떤 추가 전략을 쓸 수 있나요?

### 예상 이해 수준
Separate Chaining은 각 bucket을 linked list로 구성해 collision 시 list에 추가하는 방식이고, Open Addressing은 collision 발생 시 다른 빈 slot을 probing하는 방식임을 설명할 수 있어야 한다. Chaining은 load factor가 1을 넘어도 동작하지만 pointer overhead가 있고, Open Addressing은 cache-friendly하지만 load factor가 높아지면 성능이 급격히 저하되는 차이를 이해해야 한다.

## Level 3: 심화

### 핵심 질문
- Load factor가 hash table 성능에 미치는 영향을 설명하고, rehashing이 필요한 시점과 rehashing의 비용을 어떻게 amortize하는지 설명해주세요. 또한 hash table의 worst case가 O(n)이 되는 상황과 이를 방지하는 방법은 무엇인가요?

### 꼬리 질문
- Java의 HashMap은 bucket의 linked list가 일정 크기(8개)를 넘으면 Red-Black Tree로 전환합니다. 이 설계의 의도는 무엇인가요?
- Denial of Service 공격에서 hash flooding 공격이란 무엇이고, 이를 방어하기 위해 어떤 기법을 사용하나요?

### 예상 이해 수준
Load factor = n/m (요소 수/버킷 수)이며, 일반적으로 0.75를 threshold로 사용한다는 점을 알아야 한다. Rehashing 시 O(n) 비용이 발생하지만 드물게 일어나 amortized O(1)이 유지됨을 설명할 수 있어야 한다. 악의적으로 hash collision을 유발하는 입력으로 O(n) worst case를 만들 수 있으며, randomized hashing이나 universal hashing으로 방어함을 이해해야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- 분산 시스템에서 여러 서버에 데이터를 분산 저장할 때 Consistent Hashing을 사용하는 이유는 무엇이고, 일반 modulo hashing 대비 어떤 문제를 해결하나요?

### 꼬리 질문
- Consistent Hashing에서 가상 노드(virtual node)를 도입하는 이유와 그 효과는 무엇인가요?
- 서버가 추가되거나 제거될 때 Consistent Hashing과 일반 modulo hashing에서 각각 몇 개의 key가 재배치되어야 하나요?

### 예상 이해 수준
일반 modulo hashing에서 서버 수가 변경되면 거의 모든 key가 재배치되어야 하는 문제가 있음을 이해해야 한다. Consistent Hashing은 hash ring을 사용해 서버 변경 시 평균 k/n 개의 key만 재배치되도록 하며, 가상 노드로 부하를 더 균등하게 분산할 수 있음을 설명할 수 있어야 한다.

## 현실 연결
- Java의 HashMap과 Python의 dict는 모두 hash table로 구현되어 있으며, Python 3.7+ dict는 삽입 순서를 보장하도록 내부 구조가 개선되었다.
- 데이터베이스의 hash index는 등가 조건(=) 검색에 O(1) 성능을 제공하지만, range query에는 사용할 수 없어 B-Tree index와 상호보완적으로 사용된다.
- AWS, Cassandra, DynamoDB 같은 분산 데이터베이스는 Consistent Hashing을 사용해 노드 추가/제거 시 데이터 재배치를 최소화하며 CDN에서도 요청을 서버에 라우팅할 때 동일한 기법을 활용한다.

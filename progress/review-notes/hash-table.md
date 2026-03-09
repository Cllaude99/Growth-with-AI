# Hash Table - 복습 노트

> 학습일: 2026-03-10

## Level 1: 기본 이해

### 주요 질문
- Hash Table이 뭔지 본인의 말로 설명해볼 수 있나요?
- Key-Value를 저장하는 것만이라면 배열에 순서대로 넣어도 되는데, Hash Table이 O(1) 조회가 가능한 이유는?
- key가 "apple"이라는 문자열일 때, 내부적으로 배열의 어떤 위치에 저장할지 어떻게 결정하나요?
- 서로 다른 두 개의 key가 같은 인덱스를 반환하면 어떤 일이 벌어지나요?
- (검증) 좋은 hash function이 갖춰야 할 조건은?

### 다시 공부할 내용
- **균등 분포(uniform distribution)의 중요성**: hash function이 특정 인덱스에 값을 몰아서 반환하면 충돌이 많아지고 성능이 떨어짐. 값을 골고루 분산시키는 것이 좋은 hash function의 핵심 조건.

## Level 2: 트레이드오프

### 주요 질문
- 오픈주소법(Open Addressing)은 collision을 어떻게 해결하나요?
- Linear Probing에서 clustering 문제를 줄이려면 어떤 방법이 있나요?
- Separate Chaining과 Open Addressing 중 cache 성능이 더 좋은 쪽은?
- Separate Chaining이 Open Addressing보다 유리한 점은?
- (검증) 메모리가 제한적이고 데이터 양이 예측 가능한 환경이라면 어떤 방식을 선택하겠어요?

### 다시 공부할 내용
- **Open Addressing의 cache 이점**: 데이터가 배열 안에 연속 저장되어 cache hit rate가 높음. Chaining은 Linked List 노드가 메모리에 흩어져 cache miss 발생.
- **Chaining의 load factor 유연성**: load factor가 1을 넘어도 동작 가능. Open Addressing은 배열이 차면 성능 급격히 저하.

## Level 3: 심화

### 주요 질문
- Load factor라는 개념을 설명해볼 수 있나요?
- Load factor가 높아지면 Hash Table의 성능에 어떤 영향을 주나요?
- Rehashing의 시간 복잡도는?
- (검증) 악의적인 사용자가 의도적으로 모든 key가 같은 bucket에 들어가도록 입력을 보내면 조회 성능은?
- 이런 공격을 방어하려면 어떤 방법이 있나요?

### 다시 공부할 내용
- **Load factor**: 저장된 데이터 수 / 버킷 수. 보통 0.75를 threshold로 사용.
- **Rehashing**: load factor가 기준을 넘으면 배열을 키우고 모든 데이터를 다시 배치. O(n) 비용이지만 amortized O(1).
- **Hash flooding 공격 방어**: 악의적 입력으로 모든 key가 같은 bucket에 몰리면 O(n). Randomized hashing으로 hash function에 랜덤 요소를 넣어 공격자가 충돌을 예측할 수 없게 방어.

## Level 4: 시스템 설계

### 주요 질문
- 서버 3대에 `hash(key) % 3`으로 분산 저장할 때, 서버가 1대 추가되면 어떤 문제가 발생하나요?
- Consistent Hashing에서 서버 1대가 추가되면 원 전체의 데이터가 이동해야 하나요?
- (검증) 서버가 원 위에 균등하게 배치되지 않으면 데이터가 몰리는 문제를 어떻게 해결하나요?

### 다시 공부할 내용
- **Modulo hashing의 한계**: 서버 수가 바뀌면 `hash(key) % N`의 결과가 달라져서 거의 모든 데이터가 재배치되어야 함. 데이터는 기존 서버에 있는데 다른 서버에서 찾으려 하니 못 찾는 문제 발생.
- **Consistent Hashing**: hash ring 위에 서버와 데이터를 배치. 데이터는 시계 방향으로 처음 만나는 서버에 저장. 서버 추가/제거 시 인접한 일부 데이터만 이동하면 됨.
- **가상 노드(virtual node)**: 실제 서버 수가 적으면 원 위에 편향될 수 있음. 각 서버를 여러 개의 가상 노드로 복제하여 균등 분산 달성.

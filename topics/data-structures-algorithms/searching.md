# Searching

## 메타데이터
- **카테고리**: 자료구조 & 알고리즘
- **난이도**: 기초
- **우선순위**: 중간
- **선수과목**: Array & Linked List
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Linear Search와 Binary Search의 동작 방식과 시간복잡도를 비교하고, Binary Search가 O(log n)인 이유를 탐색 공간의 변화 관점에서 설명해주세요.

### 꼬리 질문
- Binary Search에서 mid = (left + right) / 2 대신 mid = left + (right - left) / 2를 사용하는 이유는 무엇인가요?
- 탐색 공간의 크기가 n일 때 Binary Search가 최대 몇 번의 비교 연산을 수행하나요?

### 예상 이해 수준
Binary Search가 매 단계마다 탐색 공간을 절반으로 줄여 n → n/2 → n/4 → ... → 1의 과정에서 log₂n 단계가 필요함을 수학적으로 설명할 수 있어야 한다. mid = (left + right) / 2는 정수 overflow 가능성이 있음을 이해하고 안전한 계산식을 사용해야 함을 알아야 한다. Binary Search의 전제 조건으로 배열이 정렬되어 있어야 함을 명확히 인식해야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- Binary Search를 기본 형태에서 변형하여 활용하는 패턴들을 설명해주세요. 특히 "정확한 값 찾기"를 넘어서 "특정 조건을 만족하는 최솟값/최댓값 찾기" 같은 문제에 어떻게 적용하나요?

### 꼬리 질문
- Rotated sorted array(예: [4,5,6,7,0,1,2])에서 Binary Search를 어떻게 변형해 적용할 수 있나요?
- 실수(floating point) 범위에서 Binary Search를 사용해 방정식의 근을 찾는 방법을 설명해주세요.

### 예상 이해 수준
Binary Search를 "답이 될 수 있는 공간에서 조건을 만족하는 경계를 찾는다"는 추상적인 패턴으로 이해해야 한다. 단순한 값 탐색뿐만 아니라 "최솟값 최대화", "최댓값 최소화" 같은 결정 문제(decision problem)로 변환 가능한 최적화 문제에 Binary Search를 적용할 수 있음을 이해해야 한다.

## Level 3: 심화

### 핵심 질문
- Binary Search 구현에서 흔히 발생하는 off-by-one 에러를 방지하기 위한 loop invariant 기반 접근법을 설명하고, lower_bound(첫 번째 target 위치)와 upper_bound(target보다 큰 첫 번째 위치)를 각각 어떻게 구현하나요?

### 꼬리 질문
- while(left < right)와 while(left <= right)를 사용하는 경우의 차이와 각각 종료 조건에서 정답이 어떻게 결정되는지 설명해주세요.
- 중복 원소가 있는 배열에서 target의 개수를 Binary Search만으로 O(log n)에 구하는 방법은 무엇인가요?

### 예상 이해 수준
lower_bound와 upper_bound 구현에서 경계 처리의 미묘한 차이를 정확히 설명할 수 있어야 한다. Loop invariant를 명확히 정의하고(예: "정답은 항상 [left, right] 구간 내에 있다"), 루프 종료 시 left와 right의 관계와 정답 위치를 논리적으로 도출할 수 있어야 한다. 이는 단순 암기가 아니라 불변식 기반 추론 능력을 요구한다.

## Level 4: 시스템 설계

### 핵심 질문
- 수십억 개의 문서를 빠르게 검색하는 Search Engine의 핵심 자료구조인 Inverted Index가 무엇인지 설명하고, Inverted Index와 Binary Search가 어떻게 연계되어 검색 성능을 달성하는지 설명해주세요.

### 꼬리 질문
- 검색 엔진에서 "AND" 쿼리(두 단어를 모두 포함하는 문서)를 Inverted Index에서 효율적으로 처리하는 방법은 무엇인가요?
- Git의 `git bisect` 명령이 Binary Search를 사용해 버그를 도입한 커밋을 찾는 원리를 설명해주세요.

### 예상 이해 수준
Inverted Index가 단어 → 해당 단어가 등장하는 문서 목록의 매핑 구조임을 이해하고, 정렬된 posting list에서 Binary Search로 특정 문서 ID를 O(log n)에 탐색할 수 있음을 설명해야 한다. AND 쿼리에서 두 posting list를 two-pointer로 merge하는 기법과, 짧은 posting list를 기준으로 긴 list를 Binary Search하는 최적화를 설명할 수 있어야 한다.

## 현실 연결
- 관계형 데이터베이스의 B-Tree index에서 특정 값을 찾는 것은 Binary Search의 일반화이며, 인덱스 없이 full table scan은 Linear Search에 해당한다. EXPLAIN 명령으로 실행 계획을 확인하면 이 차이를 직접 볼 수 있다.
- `git bisect` 명령은 Binary Search를 사용해 수백 개의 커밋 중 버그를 도입한 커밋을 O(log n) 단계로 자동으로 찾아내며, 각 단계에서 해당 커밋이 good/bad인지만 확인하면 된다.
- Elasticsearch와 Apache Lucene은 Inverted Index를 핵심 자료구조로 사용하며, 텍스트 검색 쿼리를 posting list 탐색과 intersection으로 변환해 수십억 개의 문서에서 관련 결과를 밀리초 내에 반환한다.

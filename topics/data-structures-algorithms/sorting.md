# Sorting

## 메타데이터
- **카테고리**: 자료구조 & 알고리즘
- **난이도**: 중급
- **우선순위**: 높음
- **선수과목**: Array & Linked List
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Bubble Sort, Selection Sort, Insertion Sort의 동작 원리를 각각 설명하고, 세 알고리즘 모두 O(n²)인데도 실제로 성능 차이가 나는 이유를 설명해주세요.

### 꼬리 질문
- 이미 거의 정렬된(nearly sorted) 배열에서 세 알고리즘 중 가장 유리한 것은 무엇이고 그 이유는 무엇인가요?
- Insertion Sort가 소규모 데이터에 적합한 이유는 단순히 구현이 쉬워서만은 아닙니다. 어떤 특성 때문인가요?

### 예상 이해 수준
세 알고리즘의 동작을 구체적인 예시로 설명할 수 있어야 한다. Big-O가 같아도 실제 연산 수(swap 횟수, comparison 횟수)와 cache 효율성에서 차이가 있음을 이해해야 한다. 특히 Insertion Sort는 partially sorted 배열에서 O(n)에 가깝게 동작하고, constant factor가 작아 소규모에서 효율적임을 설명할 수 있어야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- Merge Sort, Quick Sort, Heap Sort는 모두 O(n log n)이지만 실제 현장에서 Quick Sort가 가장 많이 쓰입니다. 세 알고리즘의 특성을 비교하고 Quick Sort가 선호되는 이유를 설명해주세요.

### 꼬리 질문
- Merge Sort는 stable sort이고 Quick Sort는 일반적으로 unstable입니다. Stable sort가 중요한 실제 사례를 설명해주세요.
- Heap Sort가 O(n log n) 보장에도 불구하고 실제로 Quick Sort보다 느린 경향이 있는 이유는 무엇인가요?

### 예상 이해 수준
Quick Sort의 average case O(n log n)이 실제로 매우 작은 constant를 가지고 in-place로 동작하며 cache-friendly하다는 점을 이해해야 한다. Merge Sort는 O(n) 추가 공간이 필요하지만 stable하고 linked list에 적합하다는 점, Heap Sort는 in-place이고 O(n log n) 최악 보장이지만 cache 효율이 낮다는 트레이드오프를 설명할 수 있어야 한다.

## Level 3: 심화

### 핵심 질문
- Quick Sort의 worst case가 O(n²)가 되는 조건은 무엇이고, 이를 방지하기 위한 pivot 선택 전략(median-of-three, random pivot 등)들을 비교해주세요. 또한 sorting algorithm에서 stability와 in-place의 의미를 정확히 설명해주세요.

### 꼬리 질문
- Introsort란 무엇이고, 왜 많은 표준 라이브러리가 순수 Quick Sort 대신 Introsort를 구현하나요?
- O(n log n)이 comparison-based sort의 이론적 하한임을 증명하는 핵심 아이디어는 무엇인가요?

### 예상 이해 수준
Quick Sort의 worst case가 이미 정렬된 배열에서 첫 번째 또는 마지막 원소를 pivot으로 선택할 때 발생함을 이해해야 한다. Stability는 동일한 key의 원소들이 정렬 후에도 원래 순서를 유지하는 성질임을 설명하고, in-place는 O(log n) 이하의 추가 공간만 사용함을 정확히 정의할 수 있어야 한다. Decision tree argument로 comparison sort의 lower bound가 Ω(n log n)임을 개념적으로 설명할 수 있어야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- 메모리에 올릴 수 없는 수십 TB 규모의 데이터를 정렬해야 한다면 어떤 전략을 사용해야 하나요? External Sort의 핵심 아이디어와 Merge Sort와의 관계를 설명해주세요.

### 꼬리 질문
- External Sort에서 k-way merge를 효율적으로 수행하기 위해 어떤 자료구조를 활용할 수 있나요?
- 분산 시스템(예: Hadoop MapReduce)에서 정렬은 어떻게 이루어지며, 이것이 External Sort와 어떤 관계가 있나요?

### 예상 이해 수준
데이터를 메모리에 맞는 chunk로 나눠 각각 정렬 후 disk에 저장하고, 이후 k-way merge로 최종 정렬된 결과를 만드는 External Sort의 흐름을 설명할 수 있어야 한다. k-way merge에서 min-heap을 활용하면 O(n log k)로 효율적으로 수행할 수 있음을 이해해야 한다.

## 현실 연결
- Python과 Java의 기본 정렬 알고리즘인 TimSort는 Merge Sort와 Insertion Sort를 결합한 hybrid algorithm으로, 실제 데이터의 partially sorted 특성을 이용해 run 단위로 분할하고 merge한다.
- 관계형 데이터베이스의 ORDER BY 연산은 데이터가 메모리에 맞으면 in-memory sort, 그렇지 않으면 External Sort를 사용하며 index가 있다면 index scan으로 정렬을 우회한다.
- Linux의 `sort` 커맨드는 기본적으로 External Sort를 구현하고 있으며, `-m` 옵션으로 이미 정렬된 파일들을 merge하거나 `--parallel` 옵션으로 병렬 정렬을 지원한다.

# Stack & Queue

## 메타데이터
- **카테고리**: 자료구조 & 알고리즘
- **난이도**: 기초
- **우선순위**: 기초
- **선수과목**: Array & Linked List
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Stack과 Queue의 핵심 개념(LIFO vs FIFO)과 각 자료구조가 지원하는 기본 연산을 설명하고, 두 자료구조가 각각 어떤 종류의 문제에 자연스럽게 적용되는지 설명해주세요.

### 꼬리 질문
- Stack을 이용해 Queue를 구현하려면 어떻게 해야 하나요? 반대로 Queue 두 개로 Stack을 구현하는 방법은 무엇인가요?
- 재귀 함수 호출이 Stack과 어떤 관계를 가지는지 설명해주세요.

### 예상 이해 수준
Stack의 push/pop/peek와 Queue의 enqueue/dequeue 연산을 정확히 설명할 수 있어야 한다. Stack은 역순 처리, 중첩 구조 파싱, 함수 호출 관리에 적합하고, Queue는 순서 보존이 필요한 작업 처리, 너비 우선 탐색에 적합함을 이해해야 한다. Stack 두 개로 Queue를 구현할 때 amortized O(1) 성능이 나오는 이유를 설명할 수 있어야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- Stack과 Queue를 각각 Array와 Linked List로 구현할 때의 장단점을 비교하고, 어떤 상황에서 각 구현 방식이 더 적합한지 설명해주세요.

### 꼬리 질문
- Array 기반 Queue에서 앞에서 dequeue하면 빈 공간이 생겨 공간 낭비가 발생합니다. Circular array로 이 문제를 어떻게 해결하나요?
- 고정 크기 Stack/Queue와 동적 크기 Stack/Queue의 적합한 사용 사례를 각각 들어주세요.

### 예상 이해 수준
Array 기반 구현은 cache-friendly하고 메모리 overhead가 작지만 크기가 고정되거나 resizing이 필요하고, Linked List 기반은 동적 크기 조절이 용이하지만 pointer overhead와 cache miss가 발생함을 이해해야 한다. Circular array Queue의 front/rear pointer 관리 방식을 구체적으로 설명할 수 있어야 한다.

## Level 3: 심화

### 핵심 질문
- Deque(Double-ended Queue), Priority Queue, Monotonic Stack의 개념을 설명하고, 각각이 해결하는 대표적인 문제를 예시와 함께 설명해주세요.

### 꼬리 질문
- Monotonic Stack을 사용해 배열에서 각 원소의 Next Greater Element를 O(n)에 찾는 방법을 설명해주세요.
- Sliding window maximum 문제를 Monotonic Deque를 활용해 O(n)에 해결하는 아이디어를 설명해주세요.

### 예상 이해 수준
Deque는 양쪽 끝에서 삽입/삭제가 가능한 자료구조임을 이해하고, Priority Queue가 힙으로 구현되어 O(log n) 삽입과 O(log n) 최대/최솟값 추출을 지원함을 알아야 한다. Monotonic Stack이 단조 증가/감소 순서를 유지하는 stack으로, 각 원소의 이전/다음 크거나 작은 원소를 O(n)에 찾는 데 활용됨을 설명할 수 있어야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- 운영체제의 Call Stack 동작 원리를 설명하고, stack overflow가 발생하는 조건과 이를 방지하기 위한 프로그래밍 기법을 설명해주세요. 또한 Message Queue 시스템이 분산 시스템에서 어떤 역할을 하는지 설명해주세요.

### 꼬리 질문
- 재귀를 반복문(iteration)으로 변환할 때 explicit stack을 사용하는 이유와 방법을 설명해주세요.
- Message Queue(Kafka, RabbitMQ 등)가 단순한 API 호출 대신 사용되는 이유는 무엇이고, producer-consumer 패턴에서 어떤 이점을 제공하나요?

### 예상 이해 수준
Call Stack에서 함수 호출 시 stack frame이 push되고 반환 시 pop되는 구조를 이해해야 한다. Tail call optimization이 stack overflow를 방지하는 원리를 설명할 수 있어야 한다. Message Queue가 비동기 통신, decoupling, back-pressure 처리, 내결함성 향상에 기여하는 방식을 시스템 설계 관점에서 설명할 수 있어야 한다.

## 현실 연결
- 브라우저의 뒤로가기 버튼은 방문한 URL을 Stack에 쌓고, 뒤로가기 시 pop하는 방식으로 구현된다. Undo/Redo 기능도 동일한 두 개의 Stack으로 구현된다.
- BFS(너비 우선 탐색)는 Queue를 사용하고, DFS(깊이 우선 탐색)는 Stack(또는 재귀)을 사용하며, 이 차이가 최단 경로 탐색 vs 경로 존재 여부 탐색에 적합한 알고리즘을 결정한다.
- Apache Kafka와 RabbitMQ 같은 Message Queue 시스템은 Queue를 추상화하여 대규모 분산 시스템에서 서비스 간 비동기 통신을 가능하게 하며, OS의 task scheduler도 ready queue로 실행 대기 프로세스를 관리한다.

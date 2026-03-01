# Array & Linked List

## 메타데이터
- **카테고리**: 자료구조 & 알고리즘
- **난이도**: 기초
- **우선순위**: 기초
- **선수과목**: 없음
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Array와 Linked List의 내부 구조가 어떻게 다르고, 각각의 메모리 배치 방식이 어떤 성능 차이를 만드는지 설명해주세요.

### 꼬리 질문
- Array에서 임의 접근이 O(1)인 이유를 메모리 주소 계산 관점에서 설명해주세요.
- Linked List에서 삽입/삭제가 O(1)이라고 할 때, 그게 항상 성립하는 조건은 무엇인가요?

### 예상 이해 수준
Array는 연속된 메모리 공간에 데이터를 저장하므로 인덱스 기반 접근이 O(1)이지만, 중간 삽입/삭제 시 O(n)의 shift 연산이 필요함을 설명할 수 있어야 한다. Linked List는 각 node가 data와 pointer로 구성되어 메모리가 분산되어 있으며, pointer를 알고 있을 때 삽입/삭제가 O(1)이지만 탐색은 O(n)임을 이해해야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- 실제 코드를 작성할 때 Array와 Linked List 중 어떤 것을 선택할지 결정하는 기준이 무엇인지, 구체적인 시나리오를 들어 설명해주세요.

### 꼬리 질문
- Cache locality 관점에서 Array가 Linked List보다 빠를 수 있는 이유는 무엇인가요?
- 삽입/삭제가 빈번한 상황에서도 Array가 Linked List보다 나을 수 있는 경우가 있을까요?

### 예상 이해 수준
단순한 시간복잡도 비교를 넘어서, CPU cache line과 spatial locality 개념을 이해하고 있어야 한다. Array는 메모리가 연속적이라 cache hit rate가 높고, Linked List는 pointer를 따라가며 cache miss가 자주 발생한다는 점을 설명할 수 있어야 한다. 읽기 위주의 workload에서는 Array, 중간 삽입/삭제가 많고 크기 예측이 어려운 경우 Linked List가 유리함을 판단할 수 있어야 한다.

## Level 3: 심화

### 핵심 질문
- Array의 dynamic resizing 전략(일반적으로 2배씩 늘리는 방식)이 amortized O(1) append를 보장하는 이유를 수학적으로 설명해주세요. 그리고 Doubly Linked List와 Circular Linked List는 각각 어떤 문제를 해결하기 위해 설계되었나요?

### 꼬리 질문
- Dynamic array에서 shrinking(축소) 전략은 왜 단순히 절반이 비면 바로 줄이지 않고, 1/4이 남을 때까지 기다리는 전략을 쓰는 경우가 많은가요?
- Circular Linked List에서 무한루프를 방지하면서 순회하는 방법은 무엇인가요?

### 예상 이해 수준
Amortized analysis의 핵심인 "총 비용을 연산 수로 나누면 평균적으로 상수"라는 개념을 설명할 수 있어야 한다. Doubly Linked List가 양방향 순회와 O(1) 이전 노드 삭제를 가능하게 한다는 점, Circular Linked List가 마지막 노드에서 첫 노드로 자연스럽게 이어지는 Ring buffer 구현에 적합하다는 점을 이해해야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- LRU(Least Recently Used) Cache를 O(1) get과 O(1) put 연산으로 구현하려면 어떤 자료구조 조합을 사용해야 하고, 그 이유는 무엇인가요?

### 꼬리 질문
- LRU Cache를 multi-threaded 환경에서 thread-safe하게 만들려면 어떤 추가 고려가 필요한가요?
- LRU 대신 LFU(Least Frequently Used) Cache를 O(1)로 구현하려면 어떤 접근이 필요한가요?

### 예상 이해 수준
HashMap + Doubly Linked List의 조합으로 LRU Cache를 구현하는 설계를 구체적으로 설명할 수 있어야 한다. HashMap은 O(1) key lookup을, Doubly Linked List는 O(1) 노드 이동(head/tail 조작)을 담당함을 이해하고, 실제 코드 레벨에서 get/put 연산 시 노드를 어떻게 이동시키는지 설명할 수 있어야 한다.

## 현실 연결
- Java의 ArrayList는 내부적으로 Object 배열로 구현되어 dynamic resizing을 수행하고, LinkedList는 Doubly Linked List로 구현되어 있어 Deque로도 활용된다.
- Redis의 List 자료형은 내부적으로 요소가 적을 때는 ziplist(연속 메모리), 많아지면 quicklist(linked list of ziplists)로 자동 전환하여 메모리와 성능을 균형있게 관리한다.
- 브라우저의 뒤로가기/앞으로가기 기능은 방문 기록을 Doubly Linked List 구조로 관리하며, 현재 페이지 포인터를 기준으로 양방향 이동을 지원한다.

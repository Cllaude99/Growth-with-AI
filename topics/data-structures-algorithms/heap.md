# Heap

## 메타데이터
- **카테고리**: 자료구조 & 알고리즘
- **난이도**: 중급
- **우선순위**: 중간
- **선수과목**: Tree, Array & Linked List
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Heap의 구조적 특성(heap property, complete binary tree)을 설명하고, insert와 extract-max/min 연산이 어떻게 heap property를 유지하며 동작하는지 설명해주세요.

### 꼬리 질문
- Heap을 배열로 표현할 때 인덱스 i인 노드의 부모, 왼쪽 자식, 오른쪽 자식의 인덱스를 구하는 공식은 무엇인가요?
- sift-up(bubble-up)과 sift-down(heapify-down)은 각각 어떤 연산 후에 사용되나요?

### 예상 이해 수준
Max Heap에서 모든 노드가 자식보다 크거나 같고, Complete Binary Tree 구조를 유지함을 이해해야 한다. Insert 시 마지막 위치에 추가 후 sift-up O(log n), extract-max 시 root를 제거하고 마지막 원소를 root로 옮긴 후 sift-down O(log n)을 수행함을 설명할 수 있어야 한다. Heap을 별도의 자료구조 없이 배열로 표현할 수 있는 이유를 complete binary tree 속성과 연결해 설명해야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- Max Heap과 Min Heap의 차이, 그리고 Heap과 BST(Binary Search Tree)를 비교하여 각각이 더 적합한 사용 사례를 설명해주세요.

### 꼬리 질문
- BST에서는 임의의 원소를 O(log n)에 찾을 수 있지만 Heap에서는 왜 O(n)이 걸리나요?
- 최댓값과 최솟값을 동시에 O(log n)에 접근해야 할 때 어떤 자료구조 설계가 필요할까요?

### 예상 이해 수준
Heap은 최댓값 또는 최솟값에 O(1) 접근, O(log n) 삽입/삭제를 지원하지만 임의 원소 검색은 O(n)임을 이해해야 한다. BST는 O(log n) 탐색/삽입/삭제와 정렬된 순회를 지원하지만 최댓값에 항상 O(1) 접근을 보장하지 않음을 설명할 수 있어야 한다. 우선순위 기반 접근이 필요하면 Heap, 정렬된 탐색이 필요하면 BST가 적합함을 판단할 수 있어야 한다.

## Level 3: 심화

### 핵심 질문
- n개의 원소로 heap을 만들 때 Build Heap 연산의 시간복잡도가 O(n)인 이유를 설명하세요. Heap Sort의 전체 시간복잡도와, Quick Sort 대비 Heap Sort가 실제로 느린 이유도 설명해주세요.

### 꼬리 질문
- Build Heap을 맨 위부터 삽입하는 방식(n번의 insert)으로 구현하면 O(n log n)이 되는데, 아래부터 heapify하는 방식이 O(n)이 되는 수학적 근거는 무엇인가요?
- k번째로 큰 원소를 찾는 문제를 Heap을 사용해 어떤 방식으로 해결하고, 시간복잡도는 얼마인가요?

### 예상 이해 수준
Build Heap에서 leaf node는 heapify가 필요 없고 내부 노드만 처리하며, 레벨이 낮을수록 노드 수가 많고 sift-down 비용이 작다는 점에서 총 비용이 O(n)으로 수렴함을 geometric series로 설명할 수 있어야 한다. Heap Sort가 O(n log n) 최악 보장이지만 실제로 cache-unfriendly한 접근 패턴 때문에 Quick Sort보다 느림을 이해해야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- 동시에 수천 개의 작업이 들어오는 Task Scheduler 시스템을 Priority Queue 기반으로 설계하되, 우선순위가 동적으로 변경되거나 작업이 취소될 수 있는 요구사항을 어떻게 처리할지 설명해주세요.

### 꼬리 질문
- Heap에서 특정 원소의 우선순위를 변경하는 decrease-key/increase-key 연산을 효율적으로 지원하려면 어떤 추가 자료구조가 필요한가요?
- 중간값(median)을 스트리밍 데이터에서 실시간으로 유지하는 알고리즘을 두 개의 Heap을 사용해 설계해주세요.

### 예상 이해 수준
기본 Heap은 임의 원소의 위치를 모르므로 decrease-key를 O(n)에 수행해야 하는데, HashMap으로 원소 위치를 추적하면 O(log n)으로 개선할 수 있음을 이해해야 한다. 스트리밍 중앙값 문제에서 Max Heap(하위 절반)과 Min Heap(상위 절반)을 균형있게 유지하는 알고리즘을 설명할 수 있어야 한다.

## 현실 연결
- 운영체제의 CPU 프로세스 스케줄러는 Priority Queue(Heap)를 사용해 우선순위가 가장 높은 프로세스를 O(log n)에 선택하며, Linux의 Completely Fair Scheduler(CFS)는 Red-Black Tree를 사용하지만 개념적으로 동일하다.
- Dijkstra 최단 경로 알고리즘은 Min Heap(Priority Queue)을 활용해 미방문 노드 중 가장 짧은 거리의 노드를 O(log V)에 선택하며, 이것이 Dijkstra의 시간복잡도를 O((V+E) log V)로 만드는 핵심이다.
- 스트리밍 데이터에서 실시간 중앙값을 계산하는 알고리즘(예: 실시간 주식 가격 중앙값, 사용자 응답 시간 중앙값)은 두 개의 Heap을 사용하는 기법이 실무에서 널리 활용된다.

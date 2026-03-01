# Graph

## 메타데이터
- **카테고리**: 자료구조 & 알고리즘
- **난이도**: 고급
- **우선순위**: 중간
- **선수과목**: Tree, Stack & Queue
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Graph를 표현하는 두 가지 주요 방법인 Adjacency Matrix와 Adjacency List의 구조를 설명하고, 각각의 시간복잡도와 공간복잡도를 비교해주세요.

### 꼬리 질문
- Directed Graph와 Undirected Graph, Weighted Graph와 Unweighted Graph의 차이가 실제 문제 모델링에서 어떻게 반영되나요?
- 노드 수 V, 간선 수 E인 Sparse Graph와 Dense Graph에서 각각 어떤 표현 방식이 더 효율적인가요?

### 예상 이해 수준
Adjacency Matrix는 O(V²) 공간을 사용하고 간선 존재 여부를 O(1)에 확인 가능하지만 sparse graph에서 공간 낭비가 크다는 점을 이해해야 한다. Adjacency List는 O(V+E) 공간을 사용하며 대부분의 실용적 그래프 알고리즘에 더 적합함을 알아야 한다. 각 표현 방식에서 인접 노드 탐색, 간선 존재 확인의 시간복잡도 차이를 설명할 수 있어야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- BFS와 DFS는 그래프 탐색의 두 가지 기본 전략입니다. 두 알고리즘의 동작 방식, 사용하는 자료구조, 시간/공간복잡도를 비교하고 각각이 적합한 문제 유형을 설명해주세요.

### 꼬리 질문
- BFS로 최단 경로를 구할 수 있는 것은 어떤 조건에서만 성립하나요? Weighted graph에서 BFS로 최단 경로를 구하면 왜 틀리나요?
- DFS의 space complexity가 O(V)인데, 이것이 실제로 stack overflow 문제를 일으킬 수 있는 상황은 어떤 경우인가요?

### 예상 이해 수준
BFS는 Queue 기반으로 레벨 단위 탐색을 수행해 unweighted graph의 최단 경로를 보장하고, DFS는 Stack(또는 재귀) 기반으로 깊이 우선 탐색해 경로 존재 여부, cycle 감지, topological sort에 활용됨을 이해해야 한다. BFS는 O(V) 공간(queue), DFS는 O(V) 공간(call stack)을 사용하지만 실제 메모리 패턴이 다름을 설명할 수 있어야 한다.

## Level 3: 심화

### 핵심 질문
- Dijkstra 알고리즘과 Bellman-Ford 알고리즘이 최단 경로를 구하는 방식을 비교하고, Dijkstra가 negative weight edge에서 실패하는 이유와 Bellman-Ford가 이를 처리할 수 있는 이유를 설명해주세요. 또한 cycle detection 방법을 directed/undirected graph별로 설명해주세요.

### 꼬리 질문
- Dijkstra 알고리즘에서 Priority Queue(min-heap)를 사용하는 이유와, 이를 사용하지 않으면 시간복잡도가 어떻게 변하나요?
- DAG(Directed Acyclic Graph)에서 최단 경로를 Topological Sort를 이용해 O(V+E)에 구하는 방법을 설명해주세요.

### 예상 이해 수준
Dijkstra의 greedy 방식이 negative weight에서 실패하는 이유(한번 확정된 노드를 다시 방문하지 않기 때문)를 구체적인 반례와 함께 설명할 수 있어야 한다. Bellman-Ford가 V-1번 relaxation을 반복하는 이유와 negative cycle 감지 방법을 이해해야 한다. DFS를 이용한 cycle detection에서 visited/recursion stack 상태 추적 방식을 설명할 수 있어야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- 수억 명의 사용자를 가진 소셜 네트워크에서 "친구 추천" 기능과 "6단계 분리" 이론을 검증하는 시스템을 설계한다면, 어떤 그래프 알고리즘과 데이터 저장 전략을 사용하겠나요?

### 꼬리 질문
- 전체 그래프를 메모리에 올릴 수 없을 때 BFS 기반 친구 추천을 어떻게 구현할 수 있나요?
- 그래프 데이터베이스(Neo4j)와 관계형 데이터베이스 중 소셜 네트워크 관계 쿼리에 어떤 것이 더 적합하며, 그 이유는 무엇인가요?

### 예상 이해 수준
대규모 그래프를 단일 서버에 올릴 수 없으므로 파티셔닝 전략이 필요하고, BFS depth를 제한(예: 2-3 hop)하여 친구 추천을 현실적으로 구현해야 함을 이해해야 한다. 그래프 데이터베이스가 관계 탐색(traversal) 쿼리에서 관계형 데이터베이스의 JOIN 연산보다 훨씬 빠른 이유를 index-free adjacency 개념으로 설명할 수 있어야 한다.

## 현실 연결
- Google Maps와 Naver 지도의 최단 경로 탐색은 실제 도로망을 그래프로 모델링하고 Dijkstra 또는 A* 알고리즘을 사용하며, 실시간 교통 정보를 반영하기 위해 edge weight를 동적으로 업데이트한다.
- Facebook의 소셜 그래프, LinkedIn의 직업 연결망, Twitter의 팔로우 관계는 모두 대규모 그래프로 표현되며, 친구 추천과 연결 강도 분석에 그래프 알고리즘이 활용된다.
- npm, pip 같은 패키지 매니저는 패키지 의존성을 DAG로 표현하고 Topological Sort로 설치 순서를 결정하며, circular dependency를 cycle detection으로 탐지한다.

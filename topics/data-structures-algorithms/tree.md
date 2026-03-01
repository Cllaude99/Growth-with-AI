# Tree

## 메타데이터
- **카테고리**: 자료구조 & 알고리즘
- **난이도**: 중급
- **우선순위**: 높음
- **선수과목**: Array & Linked List
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Binary Search Tree(BST)의 핵심 속성을 설명하고, inorder/preorder/postorder traversal이 각각 어떤 순서로 노드를 방문하며 어떤 상황에 유용한지 설명해주세요.

### 꼬리 질문
- BST에서 탐색, 삽입, 삭제의 시간복잡도가 왜 O(h)이고, h가 최선/최악의 경우 각각 얼마인가요?
- BST에서 노드를 삭제할 때 자식이 두 개인 경우 어떻게 처리하나요?

### 예상 이해 수준
BST의 핵심 속성(왼쪽 자식 < 부모 < 오른쪽 자식)을 이해하고, 세 가지 순회 방식을 구체적으로 설명할 수 있어야 한다. Inorder traversal이 정렬된 순서를 출력한다는 점, 그리고 트리의 높이 h에 따라 성능이 결정되며 skewed tree에서는 O(n)으로 악화됨을 알아야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- 일반 BST의 worst case 문제를 해결하기 위해 AVL Tree와 Red-Black Tree 같은 self-balancing BST가 등장했습니다. 두 자료구조의 균형 유지 방식과 각각의 적합한 사용 사례를 비교해주세요.

### 꼬리 질문
- AVL Tree는 height difference를 최대 1로 유지하고, Red-Black Tree는 더 느슨한 균형을 허용합니다. 이 차이가 실제 삽입/삭제 성능에 어떤 영향을 미치나요?
- Java의 TreeMap이 Red-Black Tree를 사용하는 이유는 무엇일까요?

### 예상 이해 수준
AVL Tree가 더 엄격한 균형을 유지해 탐색이 빠르지만 삽입/삭제 시 rotation이 더 자주 발생하고, Red-Black Tree는 덜 엄격하지만 삽입/삭제가 더 효율적이라는 트레이드오프를 이해해야 한다. 읽기 위주 workload에는 AVL, 삽입/삭제가 빈번한 경우 Red-Black Tree가 유리함을 설명할 수 있어야 한다.

## Level 3: 심화

### 핵심 질문
- Tree rotation(left rotation, right rotation)이 어떻게 동작하는지 설명하고, AVL Tree에서 삽입 후 불균형이 발생할 수 있는 4가지 경우(LL, RR, LR, RL)와 각각의 해결 방법을 설명해주세요.

### 꼬리 질문
- Rotation 연산이 BST의 ordering property를 보존하는 이유를 설명해주세요.
- Red-Black Tree에서 삽입 후 색상 재배정(recoloring)과 rotation 중 어떤 것을 먼저 시도하고, 그 이유는 무엇인가요?

### 예상 이해 수준
Left rotation과 right rotation이 노드의 부모-자식 관계를 어떻게 재구성하는지 시각적으로 이해하고, 각 경우에 BST 속성이 유지됨을 논리적으로 설명할 수 있어야 한다. LR, RL 경우는 double rotation이 필요하다는 점을 알고, rotation의 시간복잡도가 O(1)임을 이해해야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- 데이터베이스가 B-Tree(또는 B+Tree)를 index 자료구조로 사용하는 이유를 디스크 I/O 특성과 연관지어 설명하고, B-Tree와 Binary Tree의 구조적 차이가 성능에 어떤 영향을 미치는지 설명해주세요.

### 꼬리 질문
- B-Tree와 B+Tree의 가장 큰 차이점은 무엇이고, 대부분의 RDBMS가 B+Tree를 선호하는 이유는 무엇인가요?
- Database index를 B-Tree가 아닌 hash index로 구현하면 어떤 한계가 생기나요?

### 예상 이해 수준
디스크는 블록 단위로 I/O가 발생하므로, 하나의 노드에 많은 key를 저장하는 B-Tree는 tree height가 낮아져 디스크 접근 횟수를 최소화할 수 있음을 이해해야 한다. B+Tree에서 모든 data가 leaf node에 있고 leaf node들이 linked list로 연결되어 range scan에 최적화된 구조임을 설명할 수 있어야 한다.

## 현실 연결
- Linux와 macOS의 파일 시스템(ext4, APFS 등)은 디렉토리 구조와 파일 메타데이터를 B-Tree 기반으로 관리하여 수백만 개의 파일을 효율적으로 탐색한다.
- 브라우저의 DOM(Document Object Model)은 HTML 문서를 Tree로 표현하며, JavaScript의 querySelector 같은 CSS selector 탐색은 이 tree를 순회하는 연산이다.
- MySQL InnoDB와 PostgreSQL은 모두 B+Tree를 기본 index 구조로 사용하며, leaf node에 실제 row data(또는 primary key)를 저장하여 range query 시 연속된 leaf node를 순차 스캔한다.

# Greedy

## 메타데이터
- **카테고리**: 자료구조 & 알고리즘
- **난이도**: 중급
- **우선순위**: 중간
- **선수과목**: Sorting
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Greedy Algorithm의 핵심 아이디어를 설명하고, Activity Selection Problem(회의실 배정 문제)을 예시로 어떻게 greedy 접근으로 최적해를 구하는지 설명해주세요.

### 꼬리 질문
- Greedy 알고리즘이 항상 최적해를 보장하지는 않습니다. 거스름돈 문제에서 greedy가 동작하는 경우와 동작하지 않는 경우를 각각 예시로 들어주세요.
- Greedy 알고리즘의 시간복잡도가 일반적으로 DP보다 낮은 이유는 무엇인가요?

### 예상 이해 수준
Greedy의 핵심은 매 단계에서 현재 시점에 최선인 선택을 하고 이전 선택을 번복하지 않는다는 점을 이해해야 한다. Activity Selection에서 종료 시간이 가장 빠른 활동을 선택하는 것이 왜 최적인지 직관적으로 설명할 수 있어야 한다. Greedy는 모든 경우의 수를 탐색하지 않아 효율적이지만, 반례가 존재하면 최적해를 보장하지 못함을 알아야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- Greedy와 Dynamic Programming을 비교하고, 같은 문제를 두 방식으로 모두 풀 수 있을 때(예: 0/1 Knapsack은 DP, Fractional Knapsack은 Greedy) 왜 적용 가능 여부가 달라지는지 설명해주세요.

### 꼬리 질문
- Fractional Knapsack에서 greedy(가치/무게 비율 내림차순)가 최적해를 보장하는 이유를 증명해주세요.
- 0/1 Knapsack에서 greedy가 최적해를 보장하지 못하는 반례를 만들어주세요.

### 예상 이해 수준
Greedy가 최적해를 보장하려면 Greedy Choice Property(현재의 최선 선택이 전체 최적해에 포함된다)와 Optimal Substructure 두 가지 모두 성립해야 함을 이해해야 한다. 0/1 Knapsack은 아이템을 쪼갤 수 없어 greedy choice가 나중 선택에 영향을 미치므로 DP가 필요하지만, Fractional Knapsack은 쪼갤 수 있어 greedy가 최적임을 논리적으로 설명할 수 있어야 한다.

## Level 3: 심화

### 핵심 질문
- Greedy Choice Property를 exchange argument(교환 논법)를 사용해 증명하는 방법을 설명하고, 특정 greedy 알고리즘이 올바른지 확인하기 위해 반례를 체계적으로 찾는 전략을 설명해주세요.

### 꼬리 질문
- Prim's Algorithm과 Kruskal's Algorithm이 모두 Minimum Spanning Tree를 greedy 방식으로 구하는데, 두 알고리즘의 greedy choice가 각각 무엇인지 설명하고 이것이 최적임을 직관적으로 설명해주세요.
- 인터뷰에서 greedy로 문제를 풀었는데 틀렸다는 것을 알게 되었을 때, 어떤 방식으로 접근을 수정하는지 설명해주세요.

### 예상 이해 수준
Exchange argument의 핵심은 "최적해에서 greedy의 선택과 다른 부분을 greedy의 선택으로 교환해도 최적성이 유지된다"는 것을 보이는 것임을 이해해야 한다. 반례를 찾을 때 극단적인 경우(모든 값이 같은 경우, 정렬된 경우, 역정렬된 경우 등)를 체계적으로 시도하는 전략을 갖추어야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- Huffman Coding이 데이터 압축에서 최적 prefix-free code를 greedy 방식으로 생성하는 원리를 설명하고, 이를 파일 압축 시스템에서 실제로 어떻게 활용하는지 설명해주세요.

### 꼬리 질문
- Huffman Coding이 최적 prefix-free code임을 증명하는 핵심 아이디어는 무엇인가요? (두 빈도가 가장 낮은 문자를 먼저 합치는 것이 왜 최적인가)
- Job Scheduling 문제에서 마감 시간 내 최대한 많은 작업을 완료하거나 총 지연 시간을 최소화하는 greedy 전략을 각각 설명해주세요.

### 예상 이해 수준
Huffman Coding에서 빈도가 낮은 문자에 긴 코드, 높은 문자에 짧은 코드를 배정하는 원리와, 이를 greedy하게 달성하기 위해 min-heap으로 빈도가 가장 낮은 두 노드를 반복적으로 합치는 알고리즘을 설명할 수 있어야 한다. Prefix-free 코드가 무엇인지(어떤 코드도 다른 코드의 prefix가 되지 않음)와 이것이 왜 ambiguity-free decoding을 보장하는지 이해해야 한다.

## 현실 연결
- Huffman Coding은 JPEG, PNG, ZIP, HTTP 압축(deflate)의 핵심 구성요소로, 실제로 인터넷에서 전송되는 대부분의 압축 데이터에 활용되고 있다.
- CPU와 운영체제의 Job Scheduling 알고리즘(Shortest Job First, Earliest Deadline First)은 greedy 방식으로 최적 스케줄을 구하며, 실시간 시스템에서 deadline miss를 최소화하는 데 활용된다.
- Kruskal's Algorithm 기반의 Minimum Spanning Tree는 네트워크 인프라 설계(최소 비용으로 모든 노드를 연결하는 케이블 배선), 클러스터 분석, 이미지 분할 등에 실용적으로 활용된다.

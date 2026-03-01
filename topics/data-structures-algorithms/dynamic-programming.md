# Dynamic Programming

## 메타데이터
- **카테고리**: 자료구조 & 알고리즘
- **난이도**: 고급
- **우선순위**: 높음
- **선수과목**: Searching
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Dynamic Programming의 두 가지 핵심 속성인 Overlapping Subproblems와 Optimal Substructure가 무엇인지 설명하고, 피보나치 수열을 예시로 DP가 왜 단순 재귀보다 효율적인지 설명해주세요.

### 꼬리 질문
- Overlapping Subproblems 속성이 없는 문제에서 DP를 적용하면 어떤 일이 발생하나요? (예: Merge Sort)
- Optimal Substructure가 성립하지 않는 문제의 예시를 들어주세요. 왜 그 경우에는 DP가 적용되지 않나요?

### 예상 이해 수준
재귀로 피보나치를 계산하면 동일한 subproblem을 지수 횟수로 반복 계산해 O(2^n)이 되지만, DP로 결과를 저장하면 O(n)으로 줄어드는 이유를 명확히 설명할 수 있어야 한다. Optimal Substructure는 문제의 최적해가 subproblem의 최적해로 구성될 수 있음을 의미하며, 최단 경로 문제는 성립하지만 최장 단순 경로 문제는 성립하지 않음을 이해해야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- DP를 구현하는 두 방식인 Top-down(Memoization)과 Bottom-up(Tabulation)의 차이를 설명하고, 어떤 상황에서 각각이 더 적합한지 비교해주세요.

### 꼬리 질문
- Top-down 방식에서 call stack이 깊어지면 발생할 수 있는 문제는 무엇이고, Bottom-up은 이 문제를 어떻게 피하나요?
- Bottom-up에서는 모든 subproblem을 계산하지만, Top-down은 필요한 subproblem만 계산합니다. 이 차이가 실제로 성능에 영향을 주는 경우는 어떤 경우인가요?

### 예상 이해 수준
Top-down은 재귀 + caching으로 자연스러운 구현이 가능하고 불필요한 subproblem 계산을 피할 수 있지만 call stack overhead와 recursion limit이 단점임을 이해해야 한다. Bottom-up은 반복문으로 구현되어 space와 time 최적화가 쉽고 stack overflow가 없지만 subproblem 간의 의존성을 명확히 파악해야 한다는 점을 설명할 수 있어야 한다.

## Level 3: 심화

### 핵심 질문
- DP 문제를 풀 때 상태(state)를 정의하고 전이식(transition function)을 설계하는 방법론을 설명하고, 2D DP(예: Longest Common Subsequence)에서 공간 최적화를 통해 O(m*n)을 O(min(m,n))으로 줄이는 기법을 설명해주세요.

### 꼬리 질문
- Knapsack 문제(0/1 Knapsack vs Unbounded Knapsack)에서 상태 정의와 전이식이 어떻게 다른지 비교해주세요.
- 상태 공간이 너무 커서 일반적인 DP table로는 메모리 초과가 발생할 때, 어떤 대안적 접근 방법들이 있나요?

### 예상 이해 수준
LCS 문제에서 dp[i][j] = "s1의 처음 i글자와 s2의 처음 j글자의 LCS 길이"로 상태를 정의하고, dp[i][j] = dp[i-1][j-1]+1 (s1[i]==s2[j]) 또는 max(dp[i-1][j], dp[i][j-1])로 전이식을 도출할 수 있어야 한다. Bottom-up에서 현재 row가 이전 row에만 의존함을 파악해 두 행만 유지하는 공간 최적화를 설명할 수 있어야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- 새로운 문제를 마주쳤을 때 DP를 적용할 수 있는지 판단하고, 최적의 상태와 전이식을 설계하는 체계적인 사고 프로세스를 설명해주세요. 실제 인터뷰에서 DP 문제에 접근할 때의 단계별 전략은 무엇인가요?

### 꼬리 질문
- DP로 풀 수 있는 문제와 Greedy로 풀 수 있는 문제를 어떻게 구분하나요?
- Matrix Chain Multiplication이나 Optimal BST처럼 구간 DP(interval DP) 패턴이 적용되는 문제들의 공통적인 특징은 무엇인가요?

### 예상 이해 수준
DP 적용 판단 시 "더 작은 subproblem으로 나눌 수 있나?", "subproblem이 중복되나?", "부분 최적해가 전체 최적해를 구성하나?"를 체계적으로 확인하는 프로세스를 설명할 수 있어야 한다. 인터뷰에서는 brute force에서 시작해 중복을 발견하고 memoization을 추가하는 방식으로 자연스럽게 DP를 도출하는 능력이 중요함을 이해해야 한다.

## 현실 연결
- Unix의 `diff` 명령어와 Git의 파일 차이 비교는 Longest Common Subsequence(LCS) 알고리즘의 변형을 사용하며, 두 버전의 파일에서 변경된 줄을 최소 편집 거리(Edit Distance)로 계산한다.
- 맞춤법 교정 시스템(스마트폰 키보드, Google의 "Did you mean?")은 Levenshtein Distance(편집 거리) DP 알고리즘을 사용해 입력 단어와 사전 단어 간의 최소 편집 횟수를 계산한다.
- 물류, 택배, 네비게이션의 최적 경로 계획에서 여러 제약 조건이 있는 최적화 문제는 DP 기반 알고리즘으로 해결되며, Traveling Salesman Problem의 근사 해법에도 DP가 활용된다.

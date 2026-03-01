# Load Balancing

## 메타데이터
- **카테고리**: 시스템 설계
- **난이도**: 중급
- **우선순위**: 높음
- **선수과목**: 없음
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Load balancer가 무엇이고, 왜 필요한지 설명해주세요. Load balancer가 없다면 어떤 문제가 발생하나요?

### 꼬리 질문
- Load balancer 자체가 single point of failure가 될 수 있지 않나요? 이를 어떻게 해결하나요?
- Load balancing과 단순히 서버를 여러 대 운영하는 것은 어떻게 다른가요?

### 예상 이해 수준
Load balancer가 들어오는 트래픽을 여러 서버에 분산해 단일 서버의 과부하를 방지하고 고가용성을 달성하는 역할을 함을 설명할 수 있어야 합니다. 수평 확장(horizontal scaling)을 가능하게 하는 핵심 컴포넌트임을 이해하고, active-passive 또는 active-active 구성으로 load balancer 자체의 이중화를 설명할 수 있어야 합니다.

## Level 2: 트레이드오프

### 핵심 질문
- Round Robin, Least Connection, IP Hash, Weighted Round Robin 알고리즘의 차이점은 무엇인가요? 각각 어떤 상황에서 적합한가요?

### 꼬리 질문
- 서버들의 처리 능력이 서로 다를 때 Round Robin을 그대로 사용하면 어떤 문제가 생기나요?
- 사용자 세션 상태가 특정 서버에 저장된 경우, 어떤 알고리즘을 선택해야 하고 그 한계는 무엇인가요?

### 예상 이해 수준
Round Robin은 구현이 단순하지만 서버 부하 차이를 고려하지 못함을 설명할 수 있어야 합니다. Least Connection은 현재 연결 수가 가장 적은 서버로 요청을 보내 불균형을 줄이고, IP Hash는 동일 클라이언트의 요청을 같은 서버로 보내 세션 일관성을 유지함을 비교할 수 있어야 합니다. 각 알고리즘의 적합한 use case를 연결지어 설명할 수 있어야 합니다.

## Level 3: 심화

### 핵심 질문
- L4 load balancing과 L7 load balancing의 차이점은 무엇인가요? Health check는 어떻게 구현하고, sticky session의 문제점은 무엇인가요?

### 꼬리 질문
- L7 load balancer는 HTTP 헤더나 URL 경로를 기반으로 라우팅할 수 있는데, 이를 어떤 상황에서 활용하나요?
- Health check가 false positive를 반환하는 경우(서버는 살아있지만 실제로는 요청을 처리 못하는 상황)를 어떻게 탐지하나요?

### 예상 이해 수준
L4는 TCP/UDP 레벨에서 동작해 빠르고 범용적이지만 요청 내용을 볼 수 없고, L7은 HTTP 레벨에서 동작해 URL, 헤더, 쿠키 기반의 정교한 라우팅이 가능하지만 오버헤드가 더 큼을 설명할 수 있어야 합니다. Sticky session이 서버 간 트래픽 불균형을 야기하고 장애 복구 시 세션이 유실되는 문제를 야기하므로, 세션을 외부 저장소(Redis 등)에 저장하는 stateless 설계가 더 바람직함을 설명할 수 있어야 합니다.

## Level 4: 시스템 설계

### 핵심 질문
- 글로벌 서비스를 위한 multi-region load balancing과 failover 전략을 설계해주세요. DNS-based load balancing, Anycast, regional load balancer를 어떻게 조합하나요?

### 꼬리 질문
- 특정 리전 전체가 장애 상황일 때 자동 failover는 어떻게 구현하고, 이 과정에서 사용자 경험에 미치는 영향을 최소화하는 방법은 무엇인가요?
- Active-active와 active-passive 구성의 트레이드오프는 무엇이고, 어떤 서비스에 각각 적합한가요?

### 예상 이해 수준
DNS round robin 또는 GeoDNS를 통해 사용자를 가장 가까운 리전으로 라우팅하는 global load balancing 구조를 설계할 수 있어야 합니다. 리전 내 L7 load balancer가 서비스별 라우팅을 담당하고, health check를 통해 비정상 서버를 자동으로 제외하는 흐름을 설명할 수 있어야 합니다. Failover 시 DNS TTL의 영향과 이를 줄이기 위한 전략도 고려할 수 있어야 합니다.

## 현실 연결
- Nginx와 HAProxy를 사용한 서버 트래픽 분산 및 SSL termination 처리
- AWS ALB(L7)와 NLB(L4)의 사용 사례 구분 및 Auto Scaling Group과의 연동
- Kubernetes Ingress Controller를 통한 클러스터 내 서비스 라우팅 및 path-based routing

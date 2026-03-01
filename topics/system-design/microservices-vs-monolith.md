# Microservices vs Monolith

## 메타데이터
- **카테고리**: 시스템 설계
- **난이도**: 중급
- **우선순위**: 중간
- **선수과목**: REST API
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Monolith 아키텍처와 Microservices 아키텍처의 구조적 차이는 무엇인가요? 각각의 특징을 설명해주세요.

### 꼬리 질문
- Monolith 아키텍처가 반드시 나쁜 선택인가요? 어떤 상황에서 Monolith가 더 적합할 수 있나요?
- Microservices에서 서비스 간 통신은 어떻게 이루어지고, Monolith 내 함수 호출과 무엇이 다른가요?

### 예상 이해 수준
Monolith는 모든 기능이 하나의 배포 단위로 묶여있어 개발이 단순하지만 규모가 커질수록 복잡도가 증가하고, Microservices는 기능별로 독립적인 서비스로 분리해 각자 독립적으로 배포하고 확장할 수 있음을 설명할 수 있어야 합니다. 초기 스타트업에서 Monolith가 더 적합한 이유(빠른 개발, 낮은 운영 복잡도)를 설명할 수 있어야 합니다.

## Level 2: 트레이드오프

### 핵심 질문
- Monolith와 Microservices 아키텍처의 장단점을 개발 속도, 배포, 확장성, 장애 격리, 운영 복잡도 측면에서 비교해주세요. 어떤 기준으로 아키텍처를 선택해야 하나요?

### 꼬리 질문
- 팀 규모와 아키텍처 선택 사이에는 어떤 관계가 있나요? (Conway's Law)
- Microservices에서 서비스 간 데이터 일관성을 유지하는 것이 왜 Monolith에서보다 어렵나요?

### 예상 이해 수준
Microservices는 독립 배포, 기술 스택 다양성, 세밀한 확장이 가능하지만 네트워크 레이턴시, 분산 트랜잭션, 운영 복잡도가 크게 증가함을 설명할 수 있어야 합니다. Conway's Law에 따라 조직 구조가 시스템 아키텍처에 영향을 주므로 팀 규모와 구조에 맞는 아키텍처 선택이 필요함을 설명할 수 있어야 합니다. "마이크로서비스는 성숙한 팀과 조직에서 빛을 발한다"는 맥락을 이해해야 합니다.

## Level 3: 심화

### 핵심 질문
- Microservices 환경에서 Service discovery, Circuit breaker, Distributed tracing이 각각 왜 필요하고 어떻게 동작하나요?

### 꼬리 질문
- Circuit breaker 패턴의 세 가지 상태(Closed, Open, Half-open)를 설명하고, 언제 각 상태로 전이되나요?
- 수십 개의 마이크로서비스로 구성된 시스템에서 특정 API 요청이 느릴 때 원인을 어떻게 찾나요?

### 예상 이해 수준
Service discovery는 동적으로 변하는 서비스 인스턴스 위치를 자동으로 탐지하는 메커니즘(client-side vs server-side discovery)임을 설명할 수 있어야 합니다. Circuit breaker는 특정 서비스 장애가 연쇄적으로 전파되는 것을 방지하는 패턴이고, Distributed tracing(Jaeger, Zipkin)은 여러 서비스에 걸친 요청 흐름을 추적해 병목과 오류를 파악하는 도구임을 설명할 수 있어야 합니다.

## Level 4: 시스템 설계

### 핵심 질문
- 기존 Monolith 서비스를 Microservices로 마이그레이션하는 전략을 설계해주세요. Strangler fig pattern을 포함해 서비스를 중단하지 않고 점진적으로 전환하는 방법은 무엇인가요?

### 꼬리 질문
- 마이그레이션 도중 Monolith와 신규 마이크로서비스가 동일한 데이터베이스를 공유하는 문제를 어떻게 해결하나요?
- 어떤 기능부터 먼저 분리하는 것이 좋고, 그 판단 기준은 무엇인가요?

### 예상 이해 수준
Strangler fig pattern을 통해 새로운 기능은 마이크로서비스로 개발하고 기존 Monolith 기능을 점진적으로 이전하는 전략을 설명할 수 있어야 합니다. API gateway 또는 proxy를 통해 트래픽을 점진적으로 전환하고, 데이터베이스는 strangler 단계에서는 공유하다가 최종적으로 서비스별로 분리하는(database per service pattern) 로드맵을 제시할 수 있어야 합니다. 트래픽이 적거나 독립성이 높은 기능부터 분리하는 우선순위 전략도 설명할 수 있어야 합니다.

## 현실 연결
- Netflix가 DVD 대여 Monolith에서 스트리밍 Microservices로 전환하며 수백 개의 서비스로 분리한 아키텍처 진화 과정
- Amazon이 단일 Monolith를 분리해 팀별 독립 서비스를 운영하게 된 과정과 "two-pizza team" 원칙
- Strangler fig pattern을 적용해 Monolith를 점진적으로 분리한 Uber의 아키텍처 마이그레이션 사례

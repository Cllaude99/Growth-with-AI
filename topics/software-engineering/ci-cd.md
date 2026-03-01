# CI/CD

## 메타데이터
- **카테고리**: 소프트웨어 공학
- **난이도**: 중급
- **우선순위**: 중간
- **선수과목**: Testing Strategies
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- CI(Continuous Integration), CD(Continuous Delivery), CD(Continuous Deployment)의 개념과 차이를 설명해주세요. 세 가지가 연속된 개념인데, 각 단계에서 무엇이 자동화되고 무엇이 수동으로 남나요?

### 꼬리 질문
- Continuous Delivery와 Continuous Deployment의 차이가 사소해 보이는데, 실제 비즈니스 관점에서 이 차이가 왜 중요한가요?
- CI 없이 팀이 개발하면 어떤 문제가 생기나요? "Integration hell"이란 무엇인지 설명해주세요.

### 예상 이해 수준
CI는 팀원들이 자주(하루에 여러 번) 코드를 공유 브랜치에 통합하고, 통합 시 자동 빌드와 테스트로 문제를 조기에 발견하는 실천임을 이해해야 합니다. Continuous Delivery는 언제든지 프로덕션 배포가 가능한 상태를 유지하되 배포는 수동 승인이 필요하고, Continuous Deployment는 승인 없이 모든 변경이 자동으로 프로덕션에 배포됨을 설명할 수 있어야 합니다.

## Level 2: 트레이드오프

### 핵심 질문
- GitFlow와 Trunk-based development(트렁크 기반 개발)는 어떤 차이가 있고, 각 전략에서 CI/CD 파이프라인은 어떻게 달라지나요? 어떤 상황에서 각 전략이 더 적합한가요?

### 꼬리 질문
- GitFlow에서 long-lived feature branch가 CI의 목적과 충돌하는 이유는 무엇인가요?
- Trunk-based development를 채택하면 미완성 기능이 프로덕션에 배포될 수 있는데, 이 문제를 어떻게 해결하나요?

### 예상 이해 수준
GitFlow는 release 브랜치, develop 브랜치, feature 브랜치 등 여러 long-lived 브랜치를 유지하는 반면, Trunk-based development는 모든 개발자가 main 브랜치에 소규모 커밋을 자주 통합함을 이해해야 합니다. GitFlow는 릴리즈 주기가 긴 제품에 적합하고, Trunk-based는 지속적 배포와 빠른 피드백이 필요한 웹 서비스에 적합함을 설명할 수 있어야 합니다. Feature flag가 Trunk-based에서 미완성 기능을 숨기는 핵심 도구임을 알아야 합니다.

## Level 3: 심화

### 핵심 질문
- Blue-green deployment, Canary release, Feature flag는 각각 어떤 배포 전략이고, 어떤 문제를 해결하나요? 세 가지의 차이점과 각각의 장단점을 설명해주세요.

### 꼬리 질문
- Canary release에서 "canary" 트래픽 비율을 어떻게 결정하고, 어떤 메트릭을 기준으로 전체 배포를 진행하거나 롤백하나요?
- Feature flag를 장기간 관리하지 않으면 어떤 문제가 생기나요? Feature flag의 lifecycle을 어떻게 관리해야 하나요?

### 예상 이해 수준
Blue-green은 두 개의 동일한 프로덕션 환경을 유지하고 트래픽을 전환하여 즉각적인 롤백이 가능하지만 인프라 비용이 두 배가 됨을 이해해야 합니다. Canary는 일부 트래픽으로 새 버전을 점진적으로 검증하며 실제 사용자로 리스크를 최소화하지만, 두 버전이 동시에 운영되어 복잡성이 증가함을 알아야 합니다. Feature flag는 코드 배포와 기능 출시를 분리하여 A/B 테스트와 점진적 출시를 가능하게 하지만, flag가 누적되면 "flag debt"가 생김을 설명할 수 있어야 합니다.

## Level 4: 시스템 설계

### 핵심 질문
- 수백만 사용자를 가진 대규모 서비스의 배포 파이프라인을 설계해야 합니다. 하루에 수십 번 배포가 이루어지고, 배포 중 downtime이 0이어야 하며, 문제 발생 시 5분 안에 롤백이 가능해야 합니다. 어떻게 설계하겠나요?

### 꼬리 질문
- 배포 파이프라인에서 어떤 단계를 병렬로 실행하고, 어떤 단계는 반드시 순차적으로 실행해야 하나요? 그 이유는 무엇인가요?
- DB 스키마 변경을 zero-downtime으로 배포하는 것이 왜 어렵고, 어떻게 접근해야 하나요?

### 예상 이해 수준
파이프라인을 Build, Test(Unit/Integration/E2E 단계별), Security scan, Staging 배포, 프로덕션 배포(Canary → 전체)로 구성하고, 각 단계의 게이트 조건을 정의할 수 있어야 합니다. Zero-downtime을 위해 Blue-green 또는 Rolling update를 선택하고, DB 스키마 변경은 코드 변경과 별도로 여러 배포에 걸쳐 하위 호환성을 유지하며 진행하는 전략(expand-contract 패턴)을 설명할 수 있어야 합니다.

## 현실 연결
- GitHub Actions는 코드 push나 PR 시 자동으로 빌드, 테스트, 배포 워크플로우를 실행하는 CI/CD 플랫폼으로, YAML 기반 파이프라인 정의와 Marketplace의 다양한 Action을 조합해 사용함
- ArgoCD는 Kubernetes 환경에서 GitOps 방식으로 CD를 구현하는 도구로, Git 저장소의 선언적 Kubernetes 매니페스트를 클러스터 상태와 지속적으로 동기화하며, 드리프트 감지와 자동 롤백을 지원함
- Kubernetes rolling update는 기존 Pod를 점진적으로 새 버전으로 교체하여 zero-downtime 배포를 가능하게 하며, readiness probe로 새 Pod가 준비된 후에만 트래픽을 전환해 배포 중 서비스 가용성을 유지함

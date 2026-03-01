# Virtual DOM & Reconciliation

## 메타데이터
- **카테고리**: 웹 & 프론트엔드
- **난이도**: 중급
- **우선순위**: 높음
- **선수과목**: Browser Rendering Pipeline
- **예상 시간**: 30-40분

## Level 1: 기본 이해

### 핵심 질문
- Virtual DOM이란 무엇이고, 왜 직접 DOM을 조작하는 대신 Virtual DOM을 사용하는지 설명해주세요.

### 꼬리 질문
- 실제 DOM 조작이 비용이 큰 이유는 무엇인가요?
- Virtual DOM은 메모리에 어떤 형태로 존재하나요?

### 예상 이해 수준
Virtual DOM은 실제 DOM의 가벼운 JavaScript 객체 복사본이며, 상태 변경 시 새로운 Virtual DOM을 생성해 이전 것과 비교(diffing)한 후, 변경된 부분만 실제 DOM에 반영(patching)하는 방식임을 이해해야 한다. 실제 DOM 조작은 reflow/repaint를 유발하므로 비용이 크다는 점을 연결할 수 있어야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- React의 reconciliation 알고리즘(diffing)이 O(n³)이 아닌 O(n)으로 동작할 수 있는 이유는 무엇이고, 이를 위한 두 가지 핵심 가정(heuristic)은 무엇인가요?

### 꼬리 질문
- 리스트 렌더링에서 `key` prop이 reconciliation에 어떤 역할을 하나요?
- `key`로 index를 사용하면 안 되는 상황은 어떤 경우인가요?

### 예상 이해 수준
React의 diffing은 (1) 다른 타입의 element는 다른 트리를 생성한다, (2) key를 통해 같은 element를 식별한다는 두 가지 가정으로 O(n) 복잡도를 달성한다는 점을 알아야 한다. key가 element의 정체성(identity)을 나타내며, 리스트 재정렬 시 불필요한 DOM 재생성을 방지한다는 역할을 이해해야 한다.

## Level 3: 심화

### 핵심 질문
- React Fiber 아키텍처가 기존 Stack Reconciler의 어떤 한계를 해결하기 위해 도입되었는지 설명하고, 작업을 중단하고 재개할 수 있는(interruptible) 렌더링이 왜 중요한지 설명해주세요.

### 꼬리 질문
- Fiber에서 작업 단위를 나누는 기준은 무엇이고, 우선순위는 어떻게 결정되나요?
- Concurrent Mode에서 `startTransition`이 하는 역할은 무엇인가요?

### 예상 이해 수준
Stack Reconciler는 동기적으로 전체 트리를 비교하므로 대규모 업데이트 시 메인 스레드를 블로킹하는 문제가 있었음을 이해해야 한다. Fiber는 작업을 작은 단위(fiber node)로 나누고, 우선순위에 따라 작업을 중단/재개/폐기할 수 있어 사용자 인터랙션 응답성을 유지한다는 점을 설명할 수 있어야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- Virtual DOM 없이 UI를 효율적으로 업데이트하는 대안적 접근법(Svelte의 컴파일 타임 접근, SolidJS의 fine-grained reactivity)과 Virtual DOM 방식을 비교하고, 각각이 적합한 시나리오를 설명해주세요.

### 꼬리 질문
- Svelte가 "Virtual DOM is pure overhead"라고 주장하는 근거는 무엇인가요?
- Server Components가 Virtual DOM과 hydration에 미치는 영향은 무엇인가요?

### 예상 이해 수준
Virtual DOM은 범용적이지만 diffing 비용이 존재하고, Svelte는 컴파일 타임에 변경 추적 코드를 생성해 런타임 비교를 제거하며, SolidJS는 signal 기반으로 변경된 부분만 정확히 업데이트한다는 차이를 이해해야 한다. 각 접근법이 어떤 규모와 유형의 애플리케이션에 적합한지 논의할 수 있어야 한다.

## 현실 연결
- React 18의 Concurrent Features(useTransition, useDeferredValue)는 Fiber 아키텍처 위에서 동작하며, 사용자 경험을 크게 개선한다.
- Vue 3는 Proxy 기반 reactivity + Virtual DOM으로 세밀한 변경 추적과 효율적 패칭을 결합했다.
- 대규모 데이터 테이블(AG Grid 등)은 Virtual DOM 대신 직접 DOM 조작과 가상화(virtualization)를 사용해 성능을 최적화한다.

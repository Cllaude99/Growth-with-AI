# JavaScript Event Loop & 비동기

## 메타데이터
- **카테고리**: 웹 & 프론트엔드
- **난이도**: 중급
- **우선순위**: 최고
- **선수과목**: 없음
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- JavaScript가 single-threaded임에도 불구하고 비동기 작업(setTimeout, fetch 등)을 처리할 수 있는 이유를 event loop의 동작 원리와 함께 설명해주세요.

### 꼬리 질문
- Call stack, Web APIs, callback queue(task queue)가 각각 어떤 역할을 하나요?
- `setTimeout(fn, 0)`이 즉시 실행되지 않는 이유는 무엇인가요?

### 예상 이해 수준
JavaScript 엔진(V8 등)은 single-threaded로 하나의 call stack만 가지지만, 브라우저/Node.js 런타임이 Web APIs를 제공해 비동기 작업을 위임한다는 점을 이해해야 한다. 비동기 작업 완료 시 callback이 task queue에 들어가고, call stack이 비면 event loop가 queue에서 callback을 꺼내 실행하는 흐름을 설명할 수 있어야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- Microtask queue(Promise)와 macrotask queue(setTimeout)의 실행 우선순위 차이를 설명하고, 이 차이가 실제 코드 실행 순서에 어떤 영향을 미치는지 예시를 들어 설명해주세요.

### 꼬리 질문
- `Promise.resolve().then()`과 `setTimeout(fn, 0)` 중 어떤 것이 먼저 실행되나요? 왜 그런 설계를 했을까요?
- `queueMicrotask()`는 어떤 상황에서 유용하고, 무한 microtask loop가 위험한 이유는 무엇인가요?

### 예상 이해 수준
Microtask(Promise, MutationObserver)는 현재 task가 끝난 직후, 다음 macrotask 전에 모두 실행된다는 점을 알아야 한다. 이로 인해 Promise chain이 setTimeout보다 항상 먼저 실행되며, microtask가 계속 추가되면 렌더링이 블로킹될 수 있다는 위험을 이해해야 한다.

## Level 3: 심화

### 핵심 질문
- `async/await`가 event loop 위에서 어떻게 동작하는지 설명하고, `await` 키워드가 실행 흐름을 어떻게 변화시키는지 내부 메커니즘을 설명해주세요.

### 꼬리 질문
- `async` 함수 내에서 `await` 이후의 코드는 어느 queue에 스케줄링되나요?
- 여러 개의 독립적인 비동기 작업을 순차적으로 `await`하는 것과 `Promise.all()`로 병렬 실행하는 것의 차이는 무엇인가요?

### 예상 이해 수준
`async/await`는 Promise의 syntactic sugar이며, `await`는 함수 실행을 일시 중단하고 나머지를 microtask로 스케줄링한다는 점을 설명할 수 있어야 한다. 순차 await vs `Promise.all()` 병렬 실행의 성능 차이를 이해하고, 적절한 상황에서 선택할 수 있어야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- 대규모 데이터 처리(예: 10만 행 테이블 렌더링)나 무거운 연산이 UI를 블로킹하지 않도록 하려면 어떤 전략들을 사용할 수 있나요? 각 전략의 장단점을 비교해주세요.

### 꼬리 질문
- Web Worker와 메인 스레드 간 통신 방식과 그 비용은 어떠한가요?
- `requestAnimationFrame`, `requestIdleCallback`, chunk 분할 처리를 각각 어떤 상황에 사용하나요?

### 예상 이해 수준
장시간 실행 작업을 chunk로 분할하여 event loop에 양보하는 패턴, Web Worker로 별도 스레드에서 처리하는 패턴, `requestAnimationFrame`으로 렌더링 사이클에 맞추는 패턴의 차이를 이해해야 한다. 각 방식의 trade-off(복잡도, 데이터 전송 비용, 사용 가능 API 제한 등)를 설명할 수 있어야 한다.

## 현실 연결
- React의 Concurrent Mode(Fiber)는 렌더링 작업을 작은 단위로 쪼개 event loop에 양보하는 방식으로, 사용자 인터랙션이 블로킹되지 않도록 한다.
- Node.js의 `process.nextTick()`은 microtask보다도 높은 우선순위를 가지며, I/O 작업과의 상호작용에서 중요한 역할을 한다.
- 실제 프로덕션에서 무한 스크롤, 가상 스크롤 등은 대량 데이터를 렌더링할 때 event loop 블로킹을 방지하기 위한 패턴이다.

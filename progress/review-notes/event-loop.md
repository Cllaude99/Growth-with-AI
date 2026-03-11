# Event Loop & 비동기 - 복습 노트

## 학습일: 2026-03-12
## 판정: 통과 (Level 4)

## Level 1: 기본 이해

### 주요 질문
- JavaScript가 single-threaded인데 비동기 작업을 어떻게 처리하나요?
- call stack, Web APIs, callback queue의 역할은?
- `setTimeout(fn, 0)`이 즉시 실행되지 않는 이유는?
- (검증) `console.log('A'); setTimeout(() => console.log('B'), 0); console.log('C');`의 출력 순서와 이유는?

### 다시 공부할 내용
- 특별히 없음. 흐름을 정확히 이해함.

## Level 2: 트레이드오프

### 주요 질문
- microtask queue와 macrotask queue의 차이는?
- microtask 안에서 microtask를 계속 추가하면 어떻게 되나요?
- (검증) `setTimeout → Promise → console.log` 코드의 출력 순서와 이유는?

### 다시 공부할 내용
- `queueMicrotask()`의 구체적 활용 사례
- MutationObserver가 microtask로 동작하는 이유

## Level 3: 심화

### 주요 질문
- `await` 키워드를 만나면 함수 실행 흐름이 어떻게 변하나요?
- 순차 `await` vs `Promise.all()`의 차이는?
- (검증) 항상 `Promise.all()`이 좋은가? 순차 `await`이 적절한 상황은?

### 다시 공부할 내용
- `async/await`가 Promise의 syntactic sugar라는 점을 더 깊이 이해 (내부 generator 기반 동작)

## Level 4: 시스템 설계

### 주요 질문
- 10만 행 데이터 렌더링 시 UI 블로킹 방지 전략은?
- Web Worker와 메인 스레드 간 통신 비용은?
- Transferable Objects란?
- (검증) 10만 건 검색 필터링 기능 구현 시 어떤 전략을 조합하겠나요?

### 다시 공부할 내용
- `requestAnimationFrame`과 `requestIdleCallback`의 차이와 활용 시점
- Transferable Objects 실제 사용법
- Web Worker에서 필터링 처리하는 패턴 (서버 처리로만 생각했으나 클라이언트에서도 Web Worker로 가능)

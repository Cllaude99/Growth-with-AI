# Closure & Scope

## 메타데이터
- **카테고리**: 웹 & 프론트엔드
- **난이도**: 중급
- **우선순위**: 최고
- **선수과목**: 없음
- **예상 시간**: 30-40분

## Level 1: 기본 이해

### 핵심 질문
- JavaScript의 scope(유효 범위)란 무엇이고, closure가 형성되는 조건과 동작 원리를 설명해주세요.

### 꼬리 질문
- 함수가 선언된 위치와 호출된 위치 중 어떤 것이 scope를 결정하나요? 이를 무엇이라 부르나요?
- closure가 외부 변수에 접근할 수 있는 이유는 무엇인가요?

### 예상 이해 수준
JavaScript는 lexical scoping(정적 스코핑)을 사용하여 함수가 선언된 위치의 scope chain을 기억한다는 점을 이해해야 한다. Closure는 함수가 자신이 선언된 환경(lexical environment)에 대한 참조를 유지하는 것이며, 이를 통해 외부 함수가 종료된 후에도 외부 변수에 접근할 수 있다는 점을 설명할 수 있어야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- `var`, `let`, `const`가 scope 측면에서 어떻게 다르고, 이 차이가 closure와 결합했을 때 어떤 문제를 일으킬 수 있는지 설명해주세요.

### 꼬리 질문
- 클래식한 for 루프에서 `var`와 `let`을 사용할 때 setTimeout 결과가 달라지는 이유는 무엇인가요?
- hoisting 관점에서 `var`와 `let/const`의 차이는 무엇이고, TDZ(Temporal Dead Zone)란 무엇인가요?

### 예상 이해 수준
`var`는 function scope, `let/const`는 block scope를 가진다는 차이를 알아야 한다. for 루프에서 `var`를 사용하면 closure가 같은 변수를 공유해 의도치 않은 결과가 발생하고, `let`은 매 iteration마다 새로운 binding을 생성해 이 문제를 해결한다는 점을 설명할 수 있어야 한다. TDZ로 인해 선언 전 접근이 에러를 발생시킴을 이해해야 한다.

## Level 3: 심화

### 핵심 질문
- Closure를 활용한 data privacy(정보 은닉) 패턴과 module 패턴을 설명하고, closure가 메모리에 미치는 영향과 메모리 누수를 방지하는 방법을 설명해주세요.

### 꼬리 질문
- closure가 참조하는 변수는 가비지 컬렉션(GC)에서 어떻게 처리되나요?
- event listener에서 closure로 인한 메모리 누수가 발생하는 시나리오와 해결책은 무엇인가요?

### 예상 이해 수준
Closure를 이용해 private 변수를 만드는 패턴(모듈 패턴, factory 함수)을 이해하고, closure가 외부 변수에 대한 참조를 유지하므로 GC가 해당 변수를 해제하지 못한다는 메모리 영향을 알아야 한다. 불필요한 closure 참조를 해제하거나(null 할당, removeEventListener), WeakRef를 사용하는 방법을 설명할 수 있어야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- 대규모 프론트엔드 애플리케이션에서 closure, module 패턴, ES Modules를 활용한 상태 관리 전략을 설계한다면 어떻게 접근하시겠어요? 각 방식의 장단점과 적합한 사용 시나리오를 설명해주세요.

### 꼬리 질문
- React의 `useState` hook은 내부적으로 closure를 어떻게 활용하나요?
- stale closure 문제란 무엇이고, React hooks에서 이를 어떻게 방지하나요?

### 예상 이해 수준
Closure 기반 상태 관리(Redux store, custom hooks), module 패턴 기반 singleton, ES Modules의 정적 분석 장점을 비교할 수 있어야 한다. React hooks가 closure를 통해 컴포넌트 상태를 캡처하며, dependency array가 stale closure를 방지하는 역할을 한다는 점을 이해해야 한다.

## 현실 연결
- React hooks(useState, useEffect)는 closure를 기반으로 동작하며, dependency array를 통해 stale closure 문제를 관리한다.
- JavaScript의 module 패턴(IIFE + closure)은 ES Modules 이전에 private 상태를 관리하는 표준 방법이었다.
- debounce, throttle 같은 유틸리티 함수는 closure를 활용해 내부 타이머 상태를 은닉한다.

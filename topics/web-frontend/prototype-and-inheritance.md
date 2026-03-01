# Prototype & Inheritance

## 메타데이터
- **카테고리**: 웹 & 프론트엔드
- **난이도**: 중급
- **우선순위**: 높음
- **선수과목**: Closure & Scope
- **예상 시간**: 30-40분

## Level 1: 기본 이해

### 핵심 질문
- JavaScript의 prototype이란 무엇이고, prototype chain을 통해 상속이 어떻게 이루어지는지 설명해주세요.

### 꼬리 질문
- 모든 JavaScript 객체가 `__proto__`를 가지는 이유는 무엇인가요?
- `Object.prototype`은 prototype chain에서 어떤 위치에 있나요?

### 예상 이해 수준
JavaScript의 모든 객체는 내부적으로 `[[Prototype]]` 링크를 가지고, 프로퍼티 접근 시 해당 객체에 없으면 prototype chain을 따라 상위 객체에서 검색한다는 점을 이해해야 한다. `Object.prototype`이 chain의 최상위이며, 그 위는 `null`이라는 점을 알아야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- `class` 문법과 prototype 기반 상속의 관계를 설명하고, `class`가 도입된 이유와 내부적으로 어떻게 동작하는지 비교해주세요.

### 꼬리 질문
- `new` 키워드가 내부적으로 수행하는 4단계는 무엇인가요?
- `Object.create()`와 `new` 키워드의 차이는 무엇인가요?

### 예상 이해 수준
ES6 `class`는 prototype 기반 상속의 syntactic sugar이며, 내부적으로 함수와 prototype 객체를 사용한다는 점을 알아야 한다. `new`가 빈 객체 생성 → `__proto__` 연결 → 생성자 호출 → 결과 반환의 과정을 거친다는 것을 이해해야 한다. classical inheritance와 prototypal inheritance의 개념적 차이를 설명할 수 있어야 한다.

## Level 3: 심화

### 핵심 질문
- Prototype 오염(prototype pollution) 공격이란 무엇이고, 어떻게 발생하며 방어하는 방법은 무엇인가요? 또한 prototype chain의 성능 특성은 어떠한가요?

### 꼬리 질문
- `hasOwnProperty()`와 `in` 연산자의 차이는 무엇이고, 왜 구분해서 사용해야 하나요?
- `Object.freeze()`와 `Object.seal()`은 prototype 오염 방어에 어떻게 활용되나요?

### 예상 이해 수준
Prototype pollution은 `__proto__`나 `constructor.prototype`을 통해 Object.prototype을 오염시키는 공격이며, JSON 파싱이나 deep merge에서 주로 발생한다는 점을 알아야 한다. 깊은 prototype chain은 프로퍼티 조회 성능에 영향을 미치며, `hasOwnProperty()`로 자신의 프로퍼티만 확인하는 것이 중요하다는 점을 이해해야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- 현대 프론트엔드에서 상속보다 composition(합성)이 선호되는 이유를 설명하고, React에서 이 원칙이 어떻게 적용되는지 설계 관점에서 논의해주세요.

### 꼬리 질문
- Mixin 패턴과 composition 패턴은 각각 어떤 장단점이 있나요?
- TypeScript의 interface와 JavaScript의 prototype 상속은 어떤 관계가 있나요?

### 예상 이해 수준
깊은 상속 계층의 문제점(취약한 기반 클래스, 강한 결합)을 이해하고, composition이 유연성과 재사용성에서 유리한 이유를 설명할 수 있어야 한다. React가 HOC → Hooks로 전환하면서 composition 패턴을 더 강화한 배경을 이해해야 한다.

## 현실 연결
- React는 "Composition over Inheritance" 원칙을 공식 문서에서 강조하며, class component에서 functional component + hooks로 전환했다.
- lodash의 `_.merge()` 같은 deep merge 함수에서 prototype pollution이 발생할 수 있어, 많은 라이브러리가 이에 대한 방어 코드를 추가했다.
- TypeScript의 class 문법은 JavaScript prototype을 기반으로 하면서도 정적 타입 검사를 추가하여 안전성을 높인다.

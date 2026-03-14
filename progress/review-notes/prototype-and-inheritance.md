# Prototype & Inheritance - 복습 노트

## 학습일: 2026-03-14
## 판정: 통과 (Level 4)

## Level 1: 기본 이해

### 주요 질문
- Prototype이란 무엇이고 prototype chain은 어떻게 동작하나?
- Prototype chain의 최상위는 무엇인가?
- (검증) 프로퍼티 조회 시 검색 순서는?

### 다시 공부할 내용
- Prototype chain 최상위를 window/document로 답함 → **Object.prototype이 최상위**, 그 위는 null
- `__proto__`와 `[[Prototype]]`의 관계 심화 학습

## Level 2: 트레이드오프

### 주요 질문
- `class`와 prototype의 관계는?
- `new` 키워드의 내부 동작 4단계는?
- (검증) `class Dog extends Animal`에서 prototype chain 구성은?

### 다시 공부할 내용
- `new`의 4단계: ① 빈 객체 생성 → ② `__proto__`를 생성자의 prototype에 연결 → ③ 생성자 함수 호출(this 바인딩) → ④ 결과 반환 — 이 흐름을 확실히 암기
- `Object.create()`와 `new`의 차이 학습 필요

## Level 3: 심화

### 주요 질문
- Prototype pollution이란 무엇이고 어떻게 방어하나?
- (검증) `for...in`에서 자기 자신의 프로퍼티만 확인하는 방법은?

### 다시 공부할 내용
- **hasOwnProperty()** — 처음에 몰랐음. `obj.hasOwnProperty('key')`로 자체 프로퍼티 확인
- `Object.freeze()`와 `Object.seal()`의 차이
- Prototype pollution의 구체적 공격 벡터 (JSON 파싱, deep merge)

## Level 4: 시스템 설계

### 주요 질문
- "상속보다 합성(composition)을 선호하라"는 원칙은 무엇인가?
- React의 class component → functional component + hooks 전환 배경은?
- (검증) 여러 기능을 조합할 때 상속 vs composition 방식 비교

### 다시 공부할 내용
- Mixin 패턴의 장단점
- HOC(Higher-Order Component) → Hooks 전환의 구체적 사례
- TypeScript의 interface와 prototype 상속의 관계

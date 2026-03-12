# Closure & Scope - 복습 노트

## 학습일: 2026-03-13
## 판정: 통과 (Level 4)

## Level 1: 기본 이해

### 주요 질문
- Scope란 무엇인가?
- Closure란 무엇이고 어떻게 동작하나?
- 외부 함수가 종료된 후에도 내부 함수가 외부 변수에 접근할 수 있는 이유는?
- (검증) Lexical scoping vs dynamic scoping — JavaScript는 어느 쪽인가?

### 다시 공부할 내용
- 처음에 dynamic scoping으로 답함 → **JavaScript는 lexical scoping** (함수 선언 위치 기준)

## Level 2: 트레이드오프

### 주요 질문
- `var`와 `let`의 scope 차이는?
- for 루프에서 `var` + `setTimeout` 시 출력 결과는?
- `let`으로 바꾸면 왜 달라지는가?
- (검증) hoisting 관점에서 `var`와 `let`의 차이, TDZ란?

### 다시 공부할 내용
- for 루프 + `var` + closure 문제에서 처음에 `0,1,2,3`으로 답함 → **`3,3,3`이 정답** (var는 함수 스코프라 하나의 i를 공유)
- 루프 종료 후 `i`의 값을 undefined로 답함 → **3이 정답** (i < 3 조건 실패 시점)

## Level 3: 심화

### 주요 질문
- Closure를 활용한 private 변수(정보 은닉) 패턴은?
- Closure로 인한 메모리 누수 시나리오는?
- 메모리 누수를 어떻게 방지하나?
- (검증) 디바운스 함수가 내부적으로 closure를 어떻게 활용하는가?

### 다시 공부할 내용
- 메모리 누수 구체적 시나리오 (addEventListener + DOM 제거) — 힌트 필요했음
- 디바운스 함수의 closure 활용 — timer 변수를 closure로 은닉하는 패턴 재학습 필요
- WeakRef를 사용한 메모리 관리 방법

## Level 4: 시스템 설계

### 주요 질문
- React `useState`가 내부적으로 closure를 어떻게 활용하는가?
- Stale closure 문제란 무엇인가?
- (검증) Stale closure를 dependency array 외에 해결하는 방법은?

### 다시 공부할 내용
- Stale closure 개념 자체를 처음 접함 → 구체적인 코드 예시로 다시 학습 필요
- React hooks의 closure 캡처 메커니즘 심화 학습

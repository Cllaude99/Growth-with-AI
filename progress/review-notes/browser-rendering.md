# Browser Rendering Pipeline - 복습 노트

## 학습일: 2026-03-14
## 판정: 통과 (Level 4)

## Level 1: 기본 이해

### 주요 질문
- Critical Rendering Path의 단계별 과정은?
- display: none vs visibility: hidden이 Render Tree에 포함되는지?
- (검증) DOM과 CSSOM이 왜 별도로 생성되는가?

### 다시 공부할 내용
- 특별히 없음. 전체 흐름을 잘 이해함.

## Level 2: 트레이드오프

### 주요 질문
- Reflow와 Repaint의 성능 비용 차이는?
- transform/opacity가 성능이 좋은 이유는?
- (검증) offsetWidth를 읽기만 해도 성능 문제가 발생하는 이유는?

### 다시 공부할 내용
- **Forced synchronous layout** 개념 심화 — layout thrashing 패턴과 방지법
- GPU 가속 레이어(will-change, translateZ(0)) 남용의 부작용

## Level 3: 심화

### 주요 질문
- Script 태그가 HTML 파싱을 블로킹하는 이유는?
- async와 defer의 차이는?
- (검증) CSS가 JavaScript 실행을 블로킹하는 상황은?

### 다시 공부할 내용
- **CSS → JS 블로킹**: CSSOM이 완성되어야 JS가 실행 가능 — 처음에 정확히 답하지 못함
- `<link rel="preload">`와 `<link rel="prefetch">`의 차이
- `DOMContentLoaded`와 `window.load` 이벤트의 발생 시점 차이

## Level 4: 시스템 설계

### 주요 질문
- 초기 로딩 성능 최적화 전략은?
- SSR과 CSR이 Critical Rendering Path에 미치는 차이는?
- Hydration이란?

### 다시 공부할 내용
- Streaming SSR과 Selective Hydration 개념
- Core Web Vitals (LCP, FID, CLS)와 렌더링 파이프라인의 관계
- Next.js Server Components가 hydration 비용을 줄이는 방식

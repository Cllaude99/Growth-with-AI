# Browser Rendering Pipeline

## 메타데이터
- **카테고리**: 웹 & 프론트엔드
- **난이도**: 중급
- **우선순위**: 최고
- **선수과목**: 없음
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- 브라우저가 HTML, CSS, JavaScript를 받아서 화면에 픽셀을 그리기까지의 렌더링 과정(Critical Rendering Path)을 단계별로 설명해주세요.

### 꼬리 질문
- DOM과 CSSOM은 각각 어떻게 생성되고, 왜 별도로 만들어지나요?
- Render Tree는 DOM Tree와 어떻게 다른가요? `display: none`인 요소는 어디에 포함되나요?

### 예상 이해 수준
HTML 파싱 → DOM 생성, CSS 파싱 → CSSOM 생성, DOM + CSSOM → Render Tree 생성 → Layout(Reflow) → Paint → Composite의 흐름을 설명할 수 있어야 한다. Render Tree에는 시각적으로 보이는 노드만 포함되며, `display: none`은 제외되지만 `visibility: hidden`은 포함된다는 차이를 이해해야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- Reflow(Layout)와 Repaint의 차이를 설명하고, 각각이 발생하는 상황과 성능 비용의 차이를 비교해주세요.

### 꼬리 질문
- `offsetWidth`를 읽는 것만으로도 reflow가 발생할 수 있는 이유는 무엇인가요? (forced synchronous layout)
- `transform`과 `opacity` 변경이 `width`나 `top` 변경보다 성능이 좋은 이유는 무엇인가요?

### 예상 이해 수준
Reflow는 요소의 크기/위치가 변경될 때 layout을 재계산하는 과정이고, Repaint는 시각적 속성(색상 등)만 변경될 때 발생한다는 점을 알아야 한다. Reflow는 Repaint를 항상 동반하지만 역은 성립하지 않으며, reflow가 훨씬 비용이 크다는 것을 이해해야 한다. GPU 가속이 가능한 속성(transform, opacity)은 compositor 레이어에서 처리되어 reflow/repaint를 건너뛸 수 있다는 점을 알아야 한다.

## Level 3: 심화

### 핵심 질문
- 브라우저의 렌더링 파이프라인에서 JavaScript가 파싱과 렌더링을 블로킹하는 원리를 설명하고, `async`, `defer`, `<script>` 위치에 따른 차이와 CSS가 JavaScript 실행을 블로킹하는 상황을 설명해주세요.

### 꼬리 질문
- `<link rel="preload">`와 `<link rel="prefetch">`는 각각 어떤 리소스 로딩 전략인가요?
- `document.DOMContentLoaded`와 `window.load` 이벤트는 각각 렌더링 파이프라인의 어느 시점에서 발생하나요?

### 예상 이해 수준
JavaScript가 DOM을 수정할 수 있으므로 기본적으로 HTML 파싱을 블로킹하고, CSSOM이 완성되어야 JS가 실행될 수 있다는 CSS-JS 의존관계를 이해해야 한다. `async`는 다운로드 완료 즉시 실행, `defer`는 파싱 완료 후 순서대로 실행이라는 차이를 설명할 수 있어야 한다. Resource hints(preload, prefetch, preconnect)의 역할을 알아야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- 초기 로딩 성능을 최적화하기 위한 전략을 렌더링 파이프라인 관점에서 설계해주세요. Critical CSS, code splitting, lazy loading 등을 어떻게 조합하시겠어요?

### 꼬리 질문
- Server-Side Rendering(SSR)과 Client-Side Rendering(CSR)이 Critical Rendering Path에 어떤 차이를 만드나요?
- Streaming SSR과 Selective Hydration은 어떤 문제를 해결하나요?

### 예상 이해 수준
Critical CSS를 inline으로 삽입해 첫 렌더링을 가속하고, 나머지 CSS는 비동기 로드하는 전략을 이해해야 한다. Code splitting으로 초기 JavaScript 번들을 줄이고, route-based lazy loading으로 필요한 시점에 로드하는 패턴을 설명할 수 있어야 한다. SSR이 TTFB 후 즉시 콘텐츠를 보여줄 수 있는 이유와 hydration 비용의 trade-off를 이해해야 한다.

## 현실 연결
- Chrome DevTools의 Performance 탭은 실제 렌더링 파이프라인의 각 단계(Parse, Layout, Paint, Composite)를 시각화하여 병목을 식별할 수 있다.
- Next.js의 App Router는 Server Components + Streaming SSR을 통해 Critical Rendering Path를 최적화하며, 자동 code splitting을 지원한다.
- Google의 Core Web Vitals 중 LCP(Largest Contentful Paint)는 Critical Rendering Path 최적화와 직접적으로 연관된다.

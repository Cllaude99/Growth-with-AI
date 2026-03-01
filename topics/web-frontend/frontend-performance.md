# Frontend Performance & Core Web Vitals

## 메타데이터
- **카테고리**: 웹 & 프론트엔드
- **난이도**: 중급
- **우선순위**: 높음
- **선수과목**: Browser Rendering Pipeline
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Core Web Vitals(LCP, INP, CLS)가 각각 무엇을 측정하는지 설명하고, 왜 이 세 가지 지표가 중요한지 사용자 경험 관점에서 설명해주세요.

### 꼬리 질문
- LCP(Largest Contentful Paint)가 측정하는 "가장 큰 콘텐츠 요소"란 구체적으로 어떤 것들인가요?
- CLS(Cumulative Layout Shift)가 높아지면 사용자 경험에 어떤 문제가 생기나요?

### 예상 이해 수준
LCP는 뷰포트 내 가장 큰 콘텐츠가 렌더링되는 시점(로딩 성능), INP(Interaction to Next Paint)는 사용자 인터랙션에 대한 응답 시간(응답성), CLS는 예기치 않은 레이아웃 이동의 누적(시각적 안정성)을 측정한다는 것을 이해해야 한다. 각 지표의 좋음/개선 필요/나쁨 기준(LCP 2.5s, INP 200ms, CLS 0.1)을 알아야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- 이미지 최적화 전략(format 선택, lazy loading, responsive images, CDN)들의 장단점을 비교하고, 각각이 Core Web Vitals의 어떤 지표에 영향을 주는지 설명해주세요.

### 꼬리 질문
- WebP, AVIF, PNG, JPEG는 각각 어떤 상황에서 최적의 선택인가요?
- `loading="lazy"`와 Intersection Observer 기반 lazy loading의 차이는 무엇인가요?

### 예상 이해 수준
모던 이미지 포맷(WebP, AVIF)이 압축 효율에서 우수하지만 브라우저 호환성 trade-off가 있다는 점을 알아야 한다. Lazy loading은 초기 로드를 줄여 LCP를 개선하지만, above-the-fold 이미지에 적용하면 오히려 LCP를 악화시킨다는 점을 이해해야 한다. `<img>`에 width/height를 지정해 CLS를 방지하는 방법을 알아야 한다.

## Level 3: 심화

### 핵심 질문
- JavaScript 번들 최적화(code splitting, tree shaking, dynamic import)가 성능에 미치는 영향을 설명하고, 번들 크기와 로딩 성능의 관계를 분석해주세요.

### 꼬리 질문
- Tree shaking이 동작하기 위한 전제조건은 무엇인가요? CommonJS 모듈에서는 왜 tree shaking이 어려운가요?
- `React.lazy()`와 `Suspense`를 이용한 code splitting은 어떻게 동작하나요?

### 예상 이해 수준
Code splitting으로 route별로 번들을 분리하면 초기 로드 시간을 줄일 수 있고, tree shaking은 ES Modules의 정적 구조를 분석해 사용하지 않는 코드를 제거한다는 점을 이해해야 한다. Dynamic import로 필요한 시점에 모듈을 로드하는 패턴과, 이것이 네트워크 요청 수 증가와의 trade-off 관계에 있다는 점을 설명할 수 있어야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- 전체 프론트엔드 성능 최적화 전략을 설계한다면, 네트워크(caching, CDN, compression), 렌더링(SSR, streaming, lazy hydration), 런타임(메모이제이션, 가상화) 관점에서 어떻게 조합하시겠어요?

### 꼬리 질문
- Cache-Control 헤더의 `max-age`, `stale-while-revalidate`, `immutable` 전략은 각각 어떤 상황에 적합한가요?
- React의 `useMemo`, `useCallback`, `React.memo`를 과도하게 사용하면 오히려 성능이 나빠질 수 있는 이유는 무엇인가요?

### 예상 이해 수준
성능 최적화를 네트워크 레이어(HTTP caching, CDN, Brotli/Gzip 압축), 렌더링 레이어(SSR/SSG, streaming, partial hydration), 런타임 레이어(메모이제이션, 가상화, Web Worker)로 구분해 전략을 수립할 수 있어야 한다. 과도한 최적화가 복잡도를 높이고 오히려 성능을 저하시킬 수 있다는 점도 이해해야 한다.

## 현실 연결
- Google은 Core Web Vitals를 검색 순위 요소로 사용하며, Lighthouse와 PageSpeed Insights에서 측정할 수 있다.
- Next.js는 자동 code splitting, Image 컴포넌트(자동 최적화, lazy loading, CLS 방지), font 최적화 등 Core Web Vitals 개선을 프레임워크 차원에서 지원한다.
- Netflix, Airbnb 등은 성능 예산(performance budget)을 설정하고 CI/CD에서 자동으로 성능 회귀를 감지한다.

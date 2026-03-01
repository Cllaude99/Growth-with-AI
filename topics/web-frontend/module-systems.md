# Module Systems & Bundling

## 메타데이터
- **카테고리**: 웹 & 프론트엔드
- **난이도**: 중급
- **우선순위**: 중간
- **선수과목**: Closure & Scope
- **예상 시간**: 25-35분

## Level 1: 기본 이해

### 핵심 질문
- JavaScript의 모듈 시스템이 왜 필요한지 설명하고, CommonJS(`require/module.exports`)와 ES Modules(`import/export`)의 차이를 설명해주세요.

### 꼬리 질문
- 모듈 시스템이 없던 시절에는 전역 스코프 오염을 어떻게 방지했나요?
- `module.exports`와 `export default`는 어떤 차이가 있나요?

### 예상 이해 수준
모듈 시스템은 코드를 독립적인 단위로 분리해 네임스페이스 충돌을 방지하고 의존성을 명시적으로 관리한다는 목적을 이해해야 한다. CommonJS는 동기적으로 로드하며 Node.js에서 사용, ES Modules는 비동기 로드가 가능하고 정적 분석이 가능하다는 핵심 차이를 설명할 수 있어야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- ES Modules의 정적 구조(static structure)가 CommonJS의 동적 구조(dynamic structure)와 비교해 어떤 장점을 제공하고, 이것이 tree shaking에 어떻게 연결되는지 설명해주세요.

### 꼬리 질문
- CommonJS에서 조건부 `require()`가 가능한 것이 왜 tree shaking을 어렵게 만드나요?
- Named export와 default export는 tree shaking 관점에서 어떤 차이가 있나요?

### 예상 이해 수준
ES Modules는 import/export가 최상위 레벨에서만 가능하므로 빌드 타임에 의존 관계를 정적으로 분석할 수 있고, 사용하지 않는 export를 제거(tree shaking)할 수 있다는 점을 이해해야 한다. CommonJS의 `require()`는 런타임에 동적으로 실행되므로 어떤 모듈이 실제로 사용되는지 정적 분석이 불가능하다는 차이를 알아야 한다.

## Level 3: 심화

### 핵심 질문
- 번들러(Webpack, Vite, esbuild)의 동작 원리를 설명하고, 번들링이 필요한 이유와 각 번들러의 핵심 차이점을 비교해주세요.

### 꼬리 질문
- Webpack의 module graph 생성 과정은 어떻게 진행되나요?
- Vite가 개발 서버에서 Native ESM을 사용하는 것이 Webpack 대비 어떤 이점을 제공하나요?

### 예상 이해 수준
번들러가 entry point에서 시작해 import를 따라가며 dependency graph를 구성하고, 이를 브라우저가 이해할 수 있는 하나 이상의 번들로 변환한다는 과정을 이해해야 한다. Webpack은 강력한 플러그인 생태계, Vite는 ESM 기반 빠른 개발 서버 + Rollup 기반 프로덕션 빌드, esbuild는 Go로 작성된 극한의 빌드 속도라는 특성 차이를 설명할 수 있어야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- 대규모 모노레포(monorepo) 프론트엔드 프로젝트에서 모듈 구조, 패키지 분리, 번들 전략을 어떻게 설계하시겠어요? shared 라이브러리의 버전 관리와 빌드 최적화도 고려해주세요.

### 꼬리 질문
- Module Federation(Webpack 5)이나 Import Maps가 마이크로 프론트엔드에서 어떤 역할을 하나요?
- barrel file(index.ts에서 re-export)이 tree shaking에 미치는 영향은 무엇인가요?

### 예상 이해 수준
모노레포에서 공유 패키지를 분리하고, 각 앱이 독립적으로 빌드/배포될 수 있는 구조를 설계할 수 있어야 한다. Module Federation으로 런타임에 다른 앱의 모듈을 공유하는 패턴과 그 trade-off(런타임 의존성, 버전 충돌)를 이해해야 한다. Barrel file이 side effect가 있는 모듈을 모두 포함시켜 tree shaking을 방해할 수 있다는 점을 알아야 한다.

## 현실 연결
- Node.js는 v12부터 ES Modules를 지원하며, package.json의 `"type": "module"`로 기본 모듈 시스템을 설정할 수 있다.
- Next.js, Nuxt, SvelteKit 등 모던 프레임워크는 Vite나 Turbopack을 번들러로 채택하는 추세이다.
- Turborepo, Nx 같은 모노레포 도구는 빌드 캐싱과 태스크 파이프라이닝으로 대규모 프로젝트의 빌드 시간을 크게 단축한다.

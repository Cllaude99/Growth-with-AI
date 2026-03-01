# Web Security (XSS, CSRF, CORS)

## 메타데이터
- **카테고리**: 웹 & 프론트엔드
- **난이도**: 중급
- **우선순위**: 최고
- **선수과목**: 없음
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- XSS(Cross-Site Scripting) 공격이란 무엇이고, 어떤 방식으로 사용자에게 피해를 주며, 프론트엔드에서 어떻게 방어할 수 있는지 설명해주세요.

### 꼬리 질문
- Stored XSS, Reflected XSS, DOM-based XSS의 차이는 무엇인가요?
- `innerHTML`을 사용하면 왜 위험하고, 대안은 무엇인가요?

### 예상 이해 수준
XSS는 공격자가 악성 스크립트를 웹 페이지에 삽입해 다른 사용자의 브라우저에서 실행시키는 공격임을 이해해야 한다. 세 가지 유형(Stored, Reflected, DOM-based)의 차이를 설명할 수 있고, 입력 검증(validation), 출력 이스케이핑(escaping), CSP(Content Security Policy) 등의 방어 전략을 알아야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- CSRF(Cross-Site Request Forgery) 공격의 원리를 설명하고, SameSite Cookie, CSRF 토큰, Origin 검증 등 방어 방법들의 장단점을 비교해주세요.

### 꼬리 질문
- 왜 쿠키가 자동으로 전송되는 특성이 CSRF의 핵심 원인인가요?
- SameSite=Lax와 SameSite=Strict의 차이는 무엇이고, 각각 어떤 상황에 적합한가요?

### 예상 이해 수준
CSRF는 인증된 사용자의 브라우저가 공격자가 의도한 요청을 자동으로 보내게 만드는 공격이며, 쿠키의 자동 전송 특성을 악용한다는 점을 이해해야 한다. CSRF 토큰(서버에서 생성한 일회용 값), SameSite 쿠키 속성, Origin/Referer 헤더 검증 등 다층 방어 전략의 trade-off를 설명할 수 있어야 한다.

## Level 3: 심화

### 핵심 질문
- CORS(Cross-Origin Resource Sharing)가 왜 필요하고, preflight 요청(OPTIONS)이 발생하는 조건과 동작 원리를 설명해주세요. Same-Origin Policy와의 관계도 함께 설명해주세요.

### 꼬리 질문
- Simple request와 preflighted request의 조건은 각각 무엇인가요?
- `Access-Control-Allow-Origin: *`을 사용하면 어떤 보안 위험이 있나요?

### 예상 이해 수준
Same-Origin Policy는 다른 origin의 리소스 접근을 브라우저가 기본적으로 차단하는 보안 정책이고, CORS는 서버가 특정 origin에 접근을 허용하는 메커니즘임을 이해해야 한다. Simple request 조건(GET/POST/HEAD + 특정 헤더/Content-Type만)에 해당하지 않으면 preflight가 발생하며, 서버의 CORS 헤더가 허용 범위를 결정한다는 점을 설명할 수 있어야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- 대규모 웹 애플리케이션의 보안 아키텍처를 설계한다면, CSP(Content Security Policy), 인증/인가 전략(JWT vs Session), secure headers를 어떻게 조합하시겠어요?

### 꼬리 질문
- CSP의 `script-src`, `style-src` 디렉티브는 어떤 역할을 하고, `nonce`와 `hash` 기반 허용의 차이는 무엇인가요?
- Subresource Integrity(SRI)는 CDN에서 로드하는 스크립트에 어떤 보안을 제공하나요?

### 예상 이해 수준
다층 방어(defense in depth) 원칙에 따라 CSP로 스크립트 실행 제한, Secure/HttpOnly/SameSite 쿠키 설정, HTTPS 강제(HSTS), X-Frame-Options/X-Content-Type-Options 등을 조합하는 전략을 설명할 수 있어야 한다. JWT와 Session의 보안 특성 차이(XSS vs CSRF 노출 차이, 토큰 크기, 무효화 방법)를 이해해야 한다.

## 현실 연결
- React는 JSX에서 자동으로 HTML escaping을 수행해 XSS를 기본적으로 방어하지만, `dangerouslySetInnerHTML`을 사용하면 이 보호가 무효화된다.
- Chrome의 SameSite=Lax 기본값 정책은 2020년부터 적용되어 많은 CSRF 공격을 사전에 차단한다.
- GitHub, Slack 등 대규모 서비스들은 CSP를 엄격하게 설정하고, Bug Bounty 프로그램으로 XSS 취약점을 관리한다.

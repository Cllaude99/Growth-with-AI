# Cookie, Session, JWT

## 메타데이터
- **카테고리**: 네트워크
- **난이도**: 중급
- **우선순위**: 높음
- **선수과목**: HTTP & HTTPS
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Cookie, Session, JWT 각각이 무엇이며, HTTP의 stateless 특성을 보완하는 데 어떻게 사용되는지 설명해주세요.

### 꼬리 질문
- Cookie와 Session은 어떻게 함께 작동하나요? 브라우저에 저장된 쿠키와 서버의 세션이 어떻게 연결되는지 설명해주세요.
- JWT는 어디에 저장하는 것이 권장되며(localStorage vs HttpOnly cookie), 각각의 보안 위험은 무엇인가요?

### 예상 이해 수준
Cookie는 브라우저에 저장되는 작은 데이터 조각으로 서버가 Set-Cookie 헤더로 설정하고 이후 요청마다 자동으로 전송된다는 것을 설명할 수 있어야 한다. Session은 서버 측에 상태를 저장하고 클라이언트에는 session ID만 쿠키로 전달하는 방식이며, JWT는 클라이언트 측에 서명된 토큰 형태로 상태를 저장하는 방식이라는 근본적 차이를 이해해야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- Session 기반 인증과 JWT 기반 Token 인증의 트레이드오프를 서버 확장성, 토큰 무효화, 보안 측면에서 비교해주세요.

### 꼬리 질문
- JWT의 가장 큰 단점 중 하나가 "즉시 무효화(revocation)가 어렵다"는 것인데, 이를 해결하기 위한 접근 방법에는 어떤 것들이 있나요?
- 서버를 수평 확장할 때 Session 기반 인증에서 발생하는 문제(sticky session, session replication)와 JWT가 이 문제를 어떻게 해결하는지 설명해주세요.

### 예상 이해 수준
Session 기반 인증은 서버에서 상태를 관리하므로 즉시 로그아웃/만료가 가능하지만 서버 확장 시 세션 공유 문제가 발생한다는 것을 설명할 수 있어야 한다. JWT는 서버가 상태를 저장하지 않아 확장에 유리하지만, 토큰 발급 후 만료 전까지 강제 무효화가 어렵다는 근본적인 트레이드오프를 이해해야 한다. Redis 같은 중앙 저장소로 세션을 공유하는 방법, JWT blacklist 또는 짧은 expiry + refresh token 방식의 타협점도 설명할 수 있어야 한다.

## Level 3: 심화

### 핵심 질문
- JWT의 구조(header, payload, signature)와 서명 검증 과정을 설명하고, JWT 관련 주요 보안 취약점과 대응 방법을 이야기해주세요.

### 꼬리 질문
- JWT의 "alg: none" 취약점이 무엇이며, 이를 방지하려면 어떻게 해야 하나요? 또한 HS256(대칭키)과 RS256(비대칭키) 알고리즘의 차이와 각각이 적합한 상황은 무엇인가요?
- Refresh token rotation 전략이 무엇이며, 이 방식이 refresh token 탈취 공격을 어떻게 탐지하고 대응하나요?

### 예상 이해 수준
JWT가 header(알고리즘 타입), payload(클레임 데이터), signature(헤더+페이로드를 비밀키로 서명)의 세 부분을 Base64URL로 인코딩하여 점(.)으로 연결한 구조임을 설명할 수 있어야 한다. "alg: none" 공격(알고리즘을 none으로 변조하여 서명 검증을 우회), JWT payload는 암호화가 아닌 인코딩이라는 점(민감 정보 저장 금지), XSS를 통한 localStorage 탈취 위험을 이해해야 한다. Refresh token rotation에서 이미 사용된 refresh token이 재사용되면 토큰 탈취를 감지하고 모든 세션을 무효화하는 메커니즘을 설명할 수 있어야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- 여러 서비스(웹, 모바일, 파트너 API)를 가진 분산 환경에서 SSO(Single Sign-On)를 지원하는 중앙화된 인증 시스템을 설계해주세요.

### 꼬리 질문
- OAuth 2.0의 Authorization Code Flow에서 access token을 직접 클라이언트에 전달하지 않고 authorization code를 먼저 발급하는 이유는 무엇인가요?
- 마이크로서비스 환경에서 각 서비스가 JWT를 독립적으로 검증할 수 있게 하려면 어떤 키 관리 방식(공개키 배포, JWKS endpoint)을 사용해야 하나요?

### 예상 이해 수준
OAuth 2.0와 OpenID Connect의 역할 차이(OAuth는 권한 위임, OIDC는 인증)를 설명하고, Authorization Code Flow의 각 단계(authorization request, code 발급, token 교환)와 PKCE 확장의 목적을 이해해야 한다. 중앙 인증 서버(Authorization Server)가 JWT를 발급하고, 각 마이크로서비스가 JWKS endpoint에서 공개키를 가져와 서명을 검증하는 아키텍처를 설계할 수 있어야 한다. Access token은 짧게(15분), Refresh token은 길게(7-30일) 설정하고 HttpOnly Secure 쿠키에 저장하는 전략을 포함하면 좋다.

## 현실 연결
- OAuth 2.0는 "Google로 로그인", "GitHub로 로그인" 같은 소셜 로그인의 기반 프로토콜이며, 제3자 앱이 사용자의 비밀번호를 직접 다루지 않고 권한을 위임받는 표준 방식이다.
- SSO(Single Sign-On)는 기업 환경에서 Okta, Azure AD, Google Workspace 같은 Identity Provider(IdP)를 통해 한 번의 로그인으로 여러 서비스에 접근할 수 있게 하며, SAML 또는 OpenID Connect 프로토콜을 사용한다.
- Refresh token rotation은 Auth0, Supabase 등 주요 인증 플랫폼에서 채택하고 있으며, refresh token을 사용할 때마다 새 토큰을 발급하고 이전 토큰을 무효화하여 탈취된 토큰의 사용을 탐지한다.
- HttpOnly 쿠키에 JWT를 저장하면 JavaScript에서 접근할 수 없어 XSS 공격으로부터 토큰을 보호할 수 있으며, SameSite=Strict 또는 SameSite=Lax 설정을 통해 CSRF 공격도 방어할 수 있다.

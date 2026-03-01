# HTTP와 HTTPS

## 메타데이터
- **카테고리**: 네트워크
- **난이도**: 중급
- **우선순위**: 최고
- **선수과목**: TCP & UDP
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- HTTP의 동작 원리와 request/response의 기본 구조를 설명해주세요.

### 꼬리 질문
- HTTP가 "stateless"하다는 것이 무엇을 의미하며, 이로 인해 어떤 문제가 발생하나요?
- HTTP request에서 Header와 Body의 역할 차이는 무엇인가요?

### 예상 이해 수준
HTTP는 클라이언트-서버 모델에서 request와 response로 통신하는 텍스트 기반 프로토콜임을 설명할 수 있어야 한다. Request는 method(GET, POST 등), URL, 헤더, 바디로 구성되고, response는 상태 코드, 헤더, 바디로 구성된다는 것을 이해해야 한다. Stateless의 의미와 이로 인해 쿠키/세션이 필요하게 된 배경을 설명할 수 있으면 좋다.

## Level 2: 트레이드오프

### 핵심 질문
- HTTP/1.1, HTTP/2, HTTP/3의 핵심 차이점과 각 버전이 해결하려 했던 문제를 비교해서 설명해주세요.

### 꼬리 질문
- HTTP/1.1의 head-of-line blocking 문제가 무엇이며, HTTP/2는 이를 어떻게 해결했고 HTTP/3는 왜 또 다른 접근 방식을 택했나요?
- HTTP/2의 server push 기능이 실제로는 잘 사용되지 않는 이유가 무엇인가요?

### 예상 이해 수준
HTTP/1.1의 문제점(연결당 하나의 요청, 파이프라이닝의 한계, 헤더 중복)을 설명하고, HTTP/2가 멀티플렉싱과 헤더 압축(HPACK)으로 이를 개선했음을 이해해야 한다. HTTP/2는 TCP 레벨의 head-of-line blocking이 여전히 남아있었고, HTTP/3는 QUIC(UDP 기반)을 사용하여 이 문제를 해결했다는 점을 설명할 수 있어야 한다.

## Level 3: 심화

### 핵심 질문
- HTTPS에서 TLS handshake가 어떻게 이루어지며, 인증서(certificate)와 비대칭/대칭 암호화가 각각 어떤 역할을 하는지 설명해주세요.

### 꼬리 질문
- TLS 1.2와 TLS 1.3의 handshake 과정 차이는 무엇이며, TLS 1.3이 어떻게 더 빠르게 연결을 수립하나요?
- Man-in-the-middle 공격을 방지하기 위해 인증서 체계(Certificate Chain, CA)가 어떻게 신뢰를 보장하는지 설명해주세요.

### 예상 이해 수준
TLS handshake의 주요 단계(ClientHello, ServerHello, 인증서 검증, 키 교환, session key 생성)를 순서대로 설명할 수 있어야 한다. 비대칭 암호화는 키 교환과 인증에만 사용하고, 실제 데이터 전송에는 성능상 이유로 대칭 암호화를 사용한다는 것을 이해해야 한다. 루트 CA, 중간 CA, 서버 인증서로 이어지는 신뢰 체인 개념을 설명할 수 있으면 좋다.

## Level 4: 시스템 설계

### 핵심 질문
- 전 세계 사용자를 대상으로 하는 대규모 웹 서비스를 설계할 때, CDN과 HTTPS를 활용하여 성능과 보안을 동시에 확보하는 아키텍처를 설계해주세요.

### 꼬리 질문
- CDN에서 HTTPS를 종료(TLS termination)할 때 CDN과 원본 서버(origin) 사이의 통신은 어떻게 보안을 유지해야 하나요?
- HSTS(HTTP Strict Transport Security)가 무엇이며, 최초 연결 시의 보안 취약점인 SSL stripping 공격을 어떻게 방어하나요?

### 예상 이해 수준
CDN을 통해 정적 자산 캐싱, TLS termination, DDoS 방어를 달성하는 구조를 설명할 수 있어야 한다. CDN과 오리진 서버 간 통신도 암호화해야 하며(origin shield, private connection), HSTS preload를 통해 브라우저가 처음부터 HTTPS로 접속하도록 강제하는 방법을 이해해야 한다. HTTP/2 또는 HTTP/3를 CDN 엣지에서 활성화하여 클라이언트 성능을 최적화하는 전략도 제시할 수 있으면 좋다.

## 현실 연결
- 모든 브라우저와 웹 서버 간 통신은 HTTP/HTTPS를 기반으로 하며, Chrome은 HTTP 사이트에 "안전하지 않음" 경고를 표시하여 HTTPS 전환을 사실상 강제한다.
- Let's Encrypt는 무료 SSL/TLS 인증서를 자동 발급해주어 HTTPS 보급을 크게 확산시켰으며, 현재 수억 개의 도메인이 이를 사용한다.
- HSTS(HTTP Strict Transport Security)는 브라우저가 특정 도메인에 항상 HTTPS로만 접속하도록 기억하게 하여, SSL stripping 공격을 방지한다.
- Cloudflare, Akamai 같은 CDN 사업자들은 TLS termination을 엣지에서 수행하여 오리진 서버의 부하를 줄이고 전 세계 사용자에게 빠른 TLS 연결 수립을 제공한다.

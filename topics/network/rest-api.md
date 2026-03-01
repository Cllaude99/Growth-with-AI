# REST API

## 메타데이터
- **카테고리**: 네트워크
- **난이도**: 중급
- **우선순위**: 높음
- **선수과목**: HTTP & HTTPS
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- REST의 핵심 원칙(제약 조건)을 설명하고, 각 HTTP method(GET, POST, PUT, PATCH, DELETE)가 어떤 CRUD 연산에 매핑되는지 설명해주세요.

### 꼬리 질문
- REST에서 resource를 URI로 표현할 때 좋은 설계와 나쁜 설계의 차이를 예시와 함께 설명해주세요. (예: /getUser vs /users/{id})
- GET 요청이 "안전(safe)"하고 "멱등(idempotent)"하다는 것이 무슨 의미이며, 왜 중요한가요?

### 예상 이해 수준
REST의 6가지 제약 조건(Stateless, Client-Server, Cacheable, Layered System, Uniform Interface, Code on Demand)을 설명할 수 있거나, 최소한 Stateless와 Uniform Interface의 의미를 정확히 이해해야 한다. HTTP method의 의미론적 사용(GET은 조회, POST는 생성, PUT은 전체 교체, PATCH는 부분 수정, DELETE는 삭제)과 HTTP 상태 코드(200, 201, 400, 401, 403, 404, 500)의 의미를 설명할 수 있어야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- REST API, GraphQL, gRPC의 핵심 차이점과 각각이 가장 적합한 사용 시나리오를 트레이드오프 관점에서 비교해주세요.

### 꼬리 질문
- GraphQL의 over-fetching과 under-fetching 문제가 무엇이며, REST API에서 이 문제가 어떻게 나타나고 GraphQL은 어떻게 해결하나요?
- gRPC가 REST보다 마이크로서비스 간 내부 통신에 적합한 이유는 무엇이며, 브라우저 클라이언트에서 gRPC 사용이 어려운 이유는 무엇인가요?

### 예상 이해 수준
REST는 단순하고 범용적이지만 over/under-fetching 문제가 있고, GraphQL은 클라이언트가 필요한 데이터만 요청할 수 있지만 캐싱이 복잡하다는 트레이드오프를 설명할 수 있어야 한다. gRPC는 Protocol Buffers를 통해 효율적인 직렬화와 강타입 계약을 제공하여 내부 서비스 간 통신에 적합하지만, 브라우저 지원이 제한적이라는 점을 이해해야 한다.

## Level 3: 심화

### 핵심 질문
- HATEOAS(Hypermedia as the Engine of Application State)가 무엇인지, API 버전 관리 전략의 종류와 트레이드오프, 그리고 idempotency가 API 설계에서 왜 중요한지 설명해주세요.

### 꼬리 질문
- API 버전 관리를 URI path(/v1/users), query parameter(?version=1), 또는 request header(Accept: application/vnd.api+json;version=1)로 하는 방법의 장단점을 비교해주세요.
- 결제 API를 설계할 때 네트워크 오류로 인해 클라이언트가 동일한 요청을 재시도할 경우 중복 결제가 발생하지 않도록 idempotency key를 어떻게 구현하나요?

### 예상 이해 수준
HATEOAS가 API 응답에 다음 가능한 액션의 링크를 포함하여 클라이언트가 API 구조를 미리 알 필요 없게 한다는 개념을 설명할 수 있어야 한다. URI 기반 버전 관리가 가장 명확하지만 URL이 지저분해지고, 헤더 기반이 더 RESTful하지만 사용성이 낮다는 트레이드오프를 이해해야 한다. Idempotency key를 클라이언트가 생성하여 서버가 중복 요청을 감지하고 동일한 결과를 반환하는 패턴을 설명할 수 있어야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- 마이크로서비스 아키텍처에서 외부 클라이언트와 내부 서비스 간의 통신을 중재하는 API gateway를 설계할 때 고려해야 할 핵심 요소들을 설명해주세요.

### 꼬리 질문
- API gateway에서 rate limiting을 구현할 때 token bucket, leaky bucket, fixed window, sliding window 알고리즘의 차이와 각각의 적합한 사용 사례는 무엇인가요?
- BFF(Backend for Frontend) 패턴이 무엇이며, 하나의 API gateway 대신 BFF를 사용하는 것이 적합한 시나리오는 어떤 경우인가요?

### 예상 이해 수준
API gateway가 인증/인가, rate limiting, request routing, protocol translation, logging/monitoring을 중앙화하는 역할을 담당한다는 것을 설명할 수 있어야 한다. 외부는 REST로 노출하고 내부는 gRPC로 통신하는 하이브리드 아키텍처를 설계할 수 있어야 한다. Rate limiting 알고리즘의 동작 방식을 이해하고, 모바일/웹/파트너 API에 따라 다른 인터페이스가 필요할 때 BFF 패턴이 어떻게 복잡도를 줄이는지 설명할 수 있으면 좋다.

## 현실 연결
- Twitter/X API, GitHub API, Stripe API는 RESTful API 설계의 모범 사례로 자주 인용되며, 명확한 resource 구조, 적절한 HTTP method 사용, 상세한 오류 메시지를 제공한다.
- Stripe API는 idempotency key를 공식적으로 지원하여 결제 재시도 시 중복 결제를 방지하며, 이는 금융 API 설계의 표준적인 패턴이 되었다.
- OpenAPI(Swagger) 명세는 REST API를 기계가 읽을 수 있는 형태로 문서화하여 자동 코드 생성, 테스트, 문서화를 가능하게 하며, 대부분의 대형 API 제공자들이 채택하고 있다.
- GraphQL은 Facebook이 2015년에 오픈소스로 공개한 이후 GitHub, Shopify, Twitter 등 대형 플랫폼에서 채택하였으며, 특히 모바일 앱처럼 네트워크 비용이 중요한 환경에서 강점을 발휘한다.

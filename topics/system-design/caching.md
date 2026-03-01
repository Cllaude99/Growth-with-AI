# Caching

## 메타데이터
- **카테고리**: 시스템 설계
- **난이도**: 중급
- **우선순위**: 높음
- **선수과목**: Hash Table
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Cache가 무엇이고, 왜 필요한지 설명해주세요. Cache hit과 cache miss는 각각 어떤 상황인가요?

### 꼬리 질문
- Cache hit ratio가 낮을 때 시스템 성능에 어떤 영향을 주나요?
- Cache를 사용하면 항상 성능이 좋아지나요? 오히려 해가 될 수 있는 경우는 없나요?

### 예상 이해 수준
Cache가 데이터를 빠른 저장소에 임시 저장해 반복 요청의 응답 속도를 높이는 메커니즘임을 설명할 수 있어야 합니다. Cache hit은 요청한 데이터가 cache에 존재하는 경우, cache miss는 존재하지 않아 원본 저장소에서 데이터를 가져와야 하는 경우임을 구분할 수 있어야 합니다.

## Level 2: 트레이드오프

### 핵심 질문
- Cache-aside, Write-through, Write-back 전략의 차이점은 무엇인가요? 어떤 상황에서 각각의 전략을 선택하나요?

### 꼬리 질문
- Write-back 전략에서 cache 서버가 갑자기 다운되면 어떤 문제가 발생하나요?
- Cache-aside 패턴에서 cache miss가 발생한 직후 여러 요청이 동시에 들어오면 어떤 문제가 생기나요?

### 예상 이해 수준
Cache-aside(Lazy Loading)는 읽기 중심 workload에 적합하고 cache에 없는 데이터만 DB에서 조회함을 설명할 수 있어야 합니다. Write-through는 쓰기 시 cache와 DB를 동시에 갱신해 일관성이 높지만 쓰기 latency가 증가하고, Write-back은 cache에만 먼저 쓰고 나중에 DB에 반영해 쓰기 성능은 높지만 데이터 유실 위험이 있음을 비교할 수 있어야 합니다.

## Level 3: 심화

### 핵심 질문
- Cache invalidation 문제란 무엇인가요? Cache stampede와 thundering herd 문제를 설명하고, 각각의 해결 방법을 제시해주세요.

### 꼬리 질문
- Cache invalidation을 어떻게 구현하나요? TTL 기반 방식과 event 기반 방식의 차이는 무엇인가요?
- Cache stampede를 방지하기 위한 mutex lock 방식과 probabilistic early expiration 방식의 트레이드오프는 무엇인가요?

### 예상 이해 수준
Cache invalidation이 분산 환경에서 왜 어려운 문제인지 설명할 수 있어야 합니다. Cache stampede는 인기 있는 cache 항목이 만료될 때 다수의 요청이 동시에 DB로 몰리는 현상이고, thundering herd는 다수의 cache가 동시에 만료될 때 발생하는 유사 현상임을 구분할 수 있어야 합니다. Mutex lock, request coalescing, jitter 추가 등의 해결책을 제시할 수 있어야 합니다.

## Level 4: 시스템 설계

### 핵심 질문
- 대규모 글로벌 서비스를 위한 multi-layer caching 시스템을 설계해주세요. Client-side cache, CDN, Application-level cache (Redis), DB query cache를 어떻게 조합하고 각 layer의 역할을 어떻게 분담하나요?

### 꼬리 질문
- 각 cache layer의 TTL을 어떻게 설정하고, layer 간 데이터 일관성을 어떻게 유지하나요?
- 특정 사용자에게만 보여야 하는 개인화 데이터는 어떤 cache layer에서 처리해야 하나요?

### 예상 이해 수준
Browser cache, CDN, Application cache, DB cache의 계층적 구조를 설계하고 각 layer의 적합한 데이터 유형과 TTL 정책을 제시할 수 있어야 합니다. Cache miss 시 fallback 경로를 명확히 설명하고, 공개 콘텐츠와 사용자별 개인화 데이터의 cache 전략을 다르게 접근할 수 있어야 합니다. Cache warming 전략과 모니터링 방법도 고려할 수 있어야 합니다.

## 현실 연결
- Redis와 Memcached를 Application-level cache로 사용해 DB 부하를 줄이는 사례 (소셜 미디어 피드, 세션 저장소)
- Cloudflare, AWS CloudFront 등 CDN을 통한 정적 자산 및 동적 콘텐츠의 edge caching
- CPU L1/L2/L3 cache 계층 구조와 cache locality가 소프트웨어 성능에 미치는 영향

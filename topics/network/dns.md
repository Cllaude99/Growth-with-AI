# DNS (Domain Name System)

## 메타데이터
- **카테고리**: 네트워크
- **난이도**: 중급
- **우선순위**: 중간
- **선수과목**: OSI & TCP/IP
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- DNS의 역할과 도메인 이름이 IP 주소로 변환되는 과정을 설명해주세요.

### 꼬리 질문
- DNS가 없다면 인터넷 사용이 어떻게 달라질까요?
- 브라우저에 "google.com"을 입력했을 때 DNS 조회가 시작되기 전에 먼저 확인하는 것들은 무엇인가요? (브라우저 캐시, OS 캐시, hosts 파일)

### 예상 이해 수준
DNS가 사람이 읽기 쉬운 도메인 이름을 기계가 사용하는 IP 주소로 변환하는 분산 계층형 데이터베이스 시스템임을 설명할 수 있어야 한다. Root nameserver -> TLD nameserver -> Authoritative nameserver로 이어지는 계층 구조와, 로컬 DNS resolver(ISP 또는 8.8.8.8 같은 공공 resolver)가 이 과정을 대신 수행한다는 것을 이해해야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- DNS의 Recursive query와 Iterative query의 차이점을 설명하고, 각각의 장단점과 실제로 어떤 방식이 주로 사용되는지 이야기해주세요.

### 꼬리 질문
- DNS 캐싱이 성능 향상에 어떻게 기여하는지, 그리고 캐시로 인해 발생할 수 있는 문제는 무엇인가요?
- TTL(Time To Live) 값을 너무 짧게 설정하거나 너무 길게 설정할 때 각각 어떤 트레이드오프가 있나요?

### 예상 이해 수준
Recursive query에서는 resolver가 클라이언트를 대신하여 전체 조회 과정을 완수하고, Iterative query에서는 각 nameserver가 다음 참조할 서버를 알려주는 방식임을 설명할 수 있어야 한다. 실제로 클라이언트-resolver 간은 recursive, resolver-nameserver 간은 iterative 방식이 사용된다는 것을 이해해야 한다. TTL이 길면 캐시 효과가 크지만 DNS 변경이 즉시 반영되지 않는 문제가 있음을 설명할 수 있어야 한다.

## Level 3: 심화

### 핵심 질문
- 주요 DNS record type(A, AAAA, CNAME, MX, NS, TXT)의 역할과 용도를 설명하고, TTL 설정 전략에 대해 이야기해주세요.

### 꼬리 질문
- CNAME record와 A record의 차이점은 무엇이며, CNAME을 apex 도메인(루트 도메인)에 사용할 수 없는 이유와 이를 해결하는 방법(ALIAS/ANAME record)을 설명해주세요.
- DNS 보안 취약점인 DNS cache poisoning 공격이 무엇이며, DNSSEC가 어떻게 이를 방어하나요?

### 예상 이해 수준
각 레코드 타입의 명확한 사용 사례를 설명할 수 있어야 한다(A: IPv4 주소 매핑, AAAA: IPv6, CNAME: 도메인 별칭, MX: 이메일 서버, NS: nameserver 위임, TXT: 도메인 소유권 검증/SPF). 서비스 마이그레이션 전 TTL을 낮추고 이후 복구하는 전략을 이해해야 한다. DNS cache poisoning 공격(위조된 응답을 캐시에 주입)과 DNSSEC의 디지털 서명 기반 방어 메커니즘을 설명할 수 있으면 좋다.

## Level 4: 시스템 설계

### 핵심 질문
- 전 세계에 서비스하는 대규모 플랫폼에서 DNS 기반 load balancing과 GeoDNS를 활용하여 사용자를 가장 가까운 서버로 라우팅하는 시스템을 설계해주세요.

### 꼬리 질문
- DNS 기반 load balancing의 한계점(클라이언트 캐싱, 헬스체크 어려움)은 무엇이며, 이를 보완하기 위해 Anycast 라우팅이나 L4/L7 load balancer와 어떻게 조합할 수 있나요?
- 서비스 장애 발생 시 빠른 failover를 위해 DNS TTL을 0에 가깝게 낮추는 것이 현실적으로 어떤 문제를 일으킬 수 있나요?

### 예상 이해 수준
GeoDNS가 클라이언트의 IP 위치를 기반으로 가장 가까운 리전의 IP를 응답하는 원리를 설명하고, 이를 통해 레이턴시를 줄이는 아키텍처를 설계할 수 있어야 한다. Round-robin DNS, weighted DNS, health-check 기반 failover 등의 개념을 이해하고, DNS TTL과 실제 장애 대응 속도 간의 트레이드오프를 논의할 수 있어야 한다. Anycast를 통해 동일 IP로 여러 데이터센터로 라우팅하는 방식도 설명할 수 있으면 좋다.

## 현실 연결
- Cloudflare의 1.1.1.1과 Google의 8.8.8.8은 전 세계에서 사용되는 대표적인 public DNS resolver로, 빠른 응답과 개인정보 보호를 강조하며 수천억 건의 쿼리를 처리한다.
- AWS Route 53는 DNS 서비스와 헬스체크, 트래픽 라우팅 정책(Weighted, Latency-based, Geolocation, Failover)을 통합하여 제공하는 대표적인 클라우드 DNS 서비스이다.
- nslookup과 dig 명령어는 DNS 레코드를 직접 조회하고 DNS 전파 상태를 확인하는 데 사용되며, "DNS propagation"은 DNS 변경 사항이 전 세계 resolver에 반영되는 데 걸리는 시간을 의미한다.
- 이메일 스팸 방지를 위한 SPF, DKIM, DMARC 정책은 모두 DNS TXT 레코드를 통해 게시되어, 수신 서버가 발송 서버의 정당성을 검증하는 데 사용된다.

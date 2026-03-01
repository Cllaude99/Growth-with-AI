# OSI 7계층과 TCP/IP 4계층

## 메타데이터
- **카테고리**: 네트워크
- **난이도**: 기초
- **우선순위**: 높음
- **선수과목**: 없음
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- OSI 7계층과 TCP/IP 4계층 모델의 각 계층 이름과 역할을 설명해주세요.

### 꼬리 질문
- OSI 모델이 이론 모델이고 TCP/IP 모델이 실제 구현 모델이라는 말이 무슨 의미인가요?
- 웹 브라우저에서 google.com에 접속할 때 각 계층이 어떤 역할을 하는지 간단히 설명해주세요.

### 예상 이해 수준
OSI 7계층(Physical, Data Link, Network, Transport, Session, Presentation, Application)의 이름과 각 계층의 핵심 역할을 설명할 수 있어야 한다. TCP/IP 4계층(Network Access, Internet, Transport, Application)이 OSI 모델을 실용적으로 통합한 것임을 이해하고, 각 계층에서 사용되는 대표적인 프로토콜(HTTP, TCP, IP, Ethernet 등)을 대응시킬 수 있어야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- 네트워크를 계층으로 나누는 이유는 무엇이며, 이러한 추상화가 가져오는 장점과 단점은 무엇인가요?

### 꼬리 질문
- 계층 구조를 엄격하게 따르다 보면 성능 손실이 발생할 수 있는데, 실제 시스템에서 이를 어떻게 최적화하나요? (예: kernel bypass, DPDK)
- OSI 7계층 중 실제 인터넷에서 Session 계층과 Presentation 계층이 뚜렷하게 구분되지 않는 이유는 무엇인가요?

### 예상 이해 수준
계층화(layering)의 장점(모듈성, 표준화, 독립적 개선 가능)과 단점(각 계층을 오가는 오버헤드, 계층 간 정보 공유 제한)을 균형 있게 설명할 수 있어야 한다. 예를 들어, HTTP/3가 TCP 대신 UDP 위에 QUIC을 구현한 것이 전통적인 계층 경계를 일부 허무는 실용적 선택임을 이해하면 좋다.

## Level 3: 심화

### 핵심 질문
- 데이터가 송신자에서 수신자로 전달될 때, 각 계층에서 encapsulation과 decapsulation이 어떻게 이루어지는지 설명해주세요.

### 꼬리 질문
- Ethernet frame, IP packet, TCP segment, HTTP message의 계층별 헤더 구조와 각 헤더가 담고 있는 핵심 정보는 무엇인가요?
- 라우터와 스위치는 각각 몇 번째 계층까지 동작하며, 이로 인해 처리할 수 있는 기능의 차이가 어떻게 생기나요?

### 예상 이해 수준
송신 측에서 애플리케이션 데이터에 각 계층 헤더가 추가되는 encapsulation 과정과, 수신 측에서 반대로 헤더를 제거하는 decapsulation 과정을 명확하게 설명할 수 있어야 한다. L2 스위치는 MAC 주소를 보고 스위칭하고, L3 라우터는 IP 주소를 보고 라우팅한다는 점을 이해해야 한다. L7 로드밸런서가 HTTP 헤더까지 읽어서 라우팅 결정을 내릴 수 있다는 개념도 포함하면 좋다.

## Level 4: 시스템 설계

### 핵심 질문
- 프로덕션 환경에서 네트워크 장애가 발생했을 때, OSI 계층 모델을 활용하여 어떤 순서로 문제를 진단하고 디버깅하시겠습니까?

### 꼬리 질문
- 웹 서비스에 접속이 안 될 때 ping은 되고 curl은 안 된다면 어느 계층에 문제가 있는 것이며, 어떻게 추가 진단하겠습니까?
- Wireshark로 패킷을 캡처했을 때 TCP retransmission이 많이 보인다면 어떤 원인을 의심하고 어떻게 대응하시겠습니까?

### 예상 이해 수준
계층별 top-down 또는 bottom-up 디버깅 접근법(Physical -> 케이블/신호 확인, Data Link -> ARP/MAC 확인, Network -> IP 라우팅/ping, Transport -> 포트/방화벽/TCP 연결, Application -> HTTP 응답/DNS)을 체계적으로 설명할 수 있어야 한다. 각 계층에서 사용하는 진단 도구(ping, traceroute, netstat, tcpdump, Wireshark, curl 등)와 그 도구가 어느 계층의 정보를 보여주는지 매핑할 수 있으면 좋다.

## 현실 연결
- Wireshark는 패킷을 캡처하여 각 계층의 헤더 정보를 시각적으로 보여주며, 네트워크 트러블슈팅과 보안 분석에 필수적인 도구이다.
- ping은 ICMP 프로토콜(L3)을 사용하여 목적지까지의 연결성을 확인하고, traceroute는 패킷이 경유하는 모든 라우터(L3 홉)를 추적한다.
- 클라우드 환경(AWS VPC, GCP VPC)의 보안 그룹과 네트워크 ACL은 L3/L4 계층의 패킷 필터링을 담당하며, L7 계층의 필터링은 WAF(Web Application Firewall)가 담당한다.
- SDN(Software Defined Networking)은 전통적인 네트워크 장비의 제어 계층(Control Plane)과 데이터 계층(Data Plane)을 분리하여 계층 모델을 소프트웨어로 구현하는 현대적 접근 방식이다.

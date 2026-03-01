# Virtual Memory

## 메타데이터
- **카테고리**: 운영체제
- **난이도**: 고급
- **우선순위**: 중간
- **선수과목**: Memory Management
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Virtual memory란 무엇이며, 왜 물리 메모리보다 큰 주소 공간을 제공할 수 있는가?

### 꼬리 질문
- Virtual memory가 없다면 여러 Process를 동시에 실행할 때 어떤 문제가 발생하는가?
- Demand paging이란 무엇이며 왜 프로그램 실행 속도를 향상시킬 수 있는가?

### 예상 이해 수준
Virtual memory는 각 Process에게 연속된 가상 주소 공간의 환상을 제공하며, 실제로 사용 중인 페이지만 물리 메모리에 올려 메모리를 효율적으로 활용한다. Physical memory보다 큰 주소 공간을 제공할 수 있는 이유는 당장 필요하지 않은 페이지를 디스크(swap 공간)에 보관하기 때문이다. Demand paging은 페이지가 실제로 접근될 때만 물리 메모리에 로드하여 초기 로딩 시간을 줄이고 필요한 페이지만 메모리를 사용하게 한다. Process 간 메모리 보호와 격리도 virtual memory의 핵심 이점이라는 것을 설명할 수 있어야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- Paging과 Segmentation의 차이는 무엇이며, 각각의 장단점과 현대 OS에서의 활용 방식은 어떠한가?

### 꼬리 질문
- Paging이 External fragmentation을 해결하지만 Internal fragmentation을 일으키는 이유는 무엇인가?
- Segmentation이 프로그래머에게 더 자연스러운 메모리 모델을 제공하는 이유는 무엇인가?

### 예상 이해 수준
Paging은 메모리를 동일한 크기의 Page로 나누어 관리하므로 External fragmentation이 없지만, 마지막 페이지에서 Internal fragmentation이 발생한다. 주소 변환이 단순하고 하드웨어 지원(MMU)으로 빠르게 처리된다. Segmentation은 코드, 데이터, 스택 등 논리적 단위로 메모리를 나누어 프로그래머의 관점을 반영하고 Segment별 접근 권한 설정이 용이하지만 External fragmentation이 발생한다. 현대 OS(Linux, Windows)는 Segmentation을 거의 사용하지 않고 Paging 위주로 동작하며, x86에서는 하위 호환을 위한 최소한의 Segmentation만 유지한다는 것을 설명할 수 있어야 한다.

## Level 3: 심화

### 핵심 질문
- Page replacement 알고리즘(FIFO, LRU, Optimal)의 동작 방식과 트레이드오프를 비교하고, thrashing이란 무엇인지 설명하라.

### 꼬리 질문
- Belady's anomaly란 무엇이며, 어떤 알고리즘에서 발생하는가?
- Thrashing을 방지하기 위한 Working set model은 어떻게 동작하는가?

### 예상 이해 수준
Optimal 알고리즘은 앞으로 가장 오랫동안 사용되지 않을 페이지를 교체해 이론적 최적이지만 미래 참조 패턴을 알 수 없어 현실에서 불가능하다. LRU는 가장 오랫동안 사용되지 않은 페이지를 교체하며 Optimal에 근사하지만 정확한 구현 비용이 크다(Clock 알고리즘으로 근사). FIFO는 단순하지만 Belady's anomaly(프레임 수를 늘려도 page fault가 증가하는 현상)가 발생할 수 있다. Thrashing은 Process가 page fault를 처리하느라 실제 작업보다 page swapping에 더 많은 시간을 소비하는 상태로, Working set model로 각 Process의 활성 페이지 집합을 추적해 물리 메모리가 충분할 때만 새 Process를 허용하는 방식으로 방지한다는 것을 설명할 수 있어야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- TLB(Translation Lookaside Buffer)와 Multi-level page table은 각각 어떤 문제를 해결하며, Huge page는 언제 사용하는가?

### 꼬리 질문
- TLB miss가 발생했을 때의 처리 과정과 TLB flush가 Context switching 비용에 미치는 영향은 무엇인가?
- 64비트 시스템에서 단일 page table이 아닌 Multi-level page table을 사용하는 이유는 무엇인가?

### 예상 이해 수준
TLB는 가상-물리 주소 변환 결과를 캐싱하는 CPU 내 하드웨어로, 매번 page table을 참조하는 비용을 제거한다. TLB miss 시 OS가 page table walk를 수행(software-managed TLB의 경우)하거나 하드웨어가 자동으로 처리(x86)한다. Context switching 시 TLB flush가 필요해 초기 miss rate가 높아지며, ASID(Address Space Identifier)로 flush 없이 Process를 구분하는 최적화가 가능하다. 64비트 주소 공간에서 단일 page table은 수백 GB를 차지하므로 Multi-level page table(x86-64는 4단계)로 실제 사용 범위만 메모리를 할당한다. Huge page(2MB, 1GB)는 TLB 커버리지를 늘려 데이터베이스나 In-memory 시스템에서 TLB miss를 줄이는 데 사용된다는 것을 설명할 수 있으면 우수하다.

## 현실 연결
- Linux의 swap 파티션/파일은 Physical memory 부족 시 사용되며, SSD swap은 HDD보다 빠르지만 여전히 RAM보다 수십 배 느려 swap이 활발히 발생하면 시스템 성능이 급격히 저하된다.
- mmap() 시스템 콜은 파일을 가상 주소 공간에 직접 매핑해 read/write 대신 포인터로 접근할 수 있게 하며, 대용량 파일 처리나 Process 간 메모리 공유에 활용된다.
- Memory-mapped file I/O는 데이터베이스 엔진(SQLite, LMDB)과 JVM이 JAR 파일 로딩에 활용하며, OS page cache를 재사용하여 추가적인 copy 없이 파일을 처리할 수 있다.

# Memory Management

## 메타데이터
- **카테고리**: 운영체제
- **난이도**: 중급
- **우선순위**: 높음
- **선수과목**: Process & Thread
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Process의 메모리 공간은 어떤 영역으로 구성되며, 각 영역은 어떤 데이터를 저장하는가?

### 꼬리 질문
- Stack과 Heap의 메모리 증가 방향이 서로 반대인 이유는 무엇인가?
- 전역 변수와 지역 변수, 동적으로 할당한 객체는 각각 어느 영역에 저장되는가?

### 예상 이해 수준
Code(Text) 영역에는 실행 가능한 기계어 코드가, Data 영역에는 초기화된 전역/정적 변수가, BSS 영역에는 초기화되지 않은 전역 변수가 저장된다. Stack은 함수 호출 시 생성되는 지역 변수와 반환 주소를 저장하며 LIFO 방식으로 자동 관리된다. Heap은 런타임에 동적으로 할당되는 메모리 영역으로 프로그래머 또는 GC가 관리한다. 각 영역의 역할과 생명주기를 명확히 설명할 수 있어야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- Static memory allocation과 Dynamic memory allocation의 트레이드오프는 무엇이며, 언제 각각을 사용하는가?

### 꼬리 질문
- Stack overflow가 발생하는 원인은 무엇이며 어떻게 방지할 수 있는가?
- Heap 메모리를 사용할 때 발생할 수 있는 문제들은 무엇인가?

### 예상 이해 수준
Static allocation은 컴파일 타임에 크기가 결정되어 빠르고 예측 가능하지만 유연성이 없다. Dynamic allocation은 런타임에 필요한 만큼 메모리를 사용할 수 있어 유연하지만 할당/해제 비용이 발생하고 memory leak, dangling pointer, fragmentation 위험이 있다. Stack은 빠르고 자동 관리되지만 크기가 제한적이며, 재귀 함수 남용 시 stack overflow가 발생할 수 있다는 것을 설명할 수 있어야 한다.

## Level 3: 심화

### 핵심 질문
- Memory fragmentation의 두 가지 종류(Internal, External)를 설명하고, 각각 어떤 상황에서 발생하며 어떻게 해결하는가?

### 꼬리 질문
- Compaction이란 무엇이며 언제 수행하고, 그 비용은 얼마나 되는가?
- Buddy system과 Slab allocator는 fragmentation을 어떻게 줄이는가?

### 예상 이해 수준
Internal fragmentation은 할당된 메모리 블록이 요청보다 크게 할당될 때 내부에 사용되지 않는 공간이 생기는 현상이다. External fragmentation은 충분한 총 여유 메모리가 있지만 연속된 공간이 없어 큰 요청을 충족하지 못하는 현상이다. Compaction은 흩어진 메모리를 한쪽으로 모아 연속 공간을 확보하지만, 실행 중인 Process의 메모리를 이동시켜야 하므로 비용이 크다. 각 fragmentation 유형에 맞는 해결 전략을 제시할 수 있어야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- Garbage collector를 설계한다면 Mark-sweep 방식과 Reference counting 방식 중 어떤 것을 선택하고, 각각의 한계를 어떻게 보완하겠는가?

### 꼬리 질문
- Mark-sweep GC에서 Stop-the-world 문제를 줄이기 위한 기법들은 무엇인가?
- Reference counting이 순환 참조(Circular reference)를 처리하지 못하는 이유와 해결 방법은 무엇인가?

### 예상 이해 수준
Mark-sweep은 GC root에서 도달 가능한 객체를 표시(mark)하고 나머지를 수거(sweep)한다. Circular reference를 처리할 수 있지만 Stop-the-world 시간이 발생한다. 이를 줄이기 위해 Incremental GC, Concurrent GC, Generational GC(대부분의 객체는 금방 사라진다는 가설 기반)를 사용한다. Reference counting은 즉시 수거하여 pause가 없지만 순환 참조 처리를 위해 Weak reference나 별도의 cycle detector가 필요하다. 실제 언어/런타임의 GC 전략(Java: G1, Python: Reference counting + cycle collector)을 연결해 설명할 수 있으면 우수하다.

## 현실 연결
- Java의 G1 GC는 힙을 Region으로 나누어 Generational collection과 Concurrent marking을 결합해 Stop-the-world 시간을 최소화한다.
- C에서 malloc/free를 사용할 때 해제를 잊으면 memory leak이 발생하며, Valgrind 같은 도구로 탐지한다.
- Android 앱에서 Activity에 대한 static reference를 유지하면 화면 회전 시 이전 Activity가 GC되지 않아 memory leak이 발생한다.

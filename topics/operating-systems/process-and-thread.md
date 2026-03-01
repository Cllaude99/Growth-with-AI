# Process와 Thread

## 메타데이터
- **카테고리**: 운영체제
- **난이도**: 중급
- **우선순위**: 최고
- **선수과목**: 없음
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Process와 Thread의 정의는 무엇이며, 둘의 근본적인 차이는 무엇인가?

### 꼬리 질문
- Process가 가지는 독립적인 메모리 공간에는 어떤 영역들이 포함되는가?
- 같은 Process 내의 Thread들이 공유하는 자원과 각자 독립적으로 갖는 자원을 각각 설명하라.

### 예상 이해 수준
Process는 실행 중인 프로그램의 독립적인 인스턴스이며, 각자의 메모리 공간(Code, Data, Heap, Stack)을 가진다. Thread는 Process 내의 실행 단위로, 같은 Process의 Thread들은 Code, Data, Heap 영역을 공유하지만 각자의 Stack과 Register를 가진다는 것을 명확히 설명할 수 있어야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- Multi-process 방식과 Multi-thread 방식의 트레이드오프를 설명하고, 어떤 상황에서 각각을 선택하는가?

### 꼬리 질문
- Multi-process에서 Process 간 통신(IPC)이 필요한 이유와 방법은 무엇인가?
- Multi-thread 환경에서 하나의 Thread가 crash 났을 때 어떤 일이 발생하는가?

### 예상 이해 수준
Multi-process는 격리성이 높아 하나의 Process 장애가 다른 Process에 영향을 미치지 않지만, 메모리 오버헤드가 크고 IPC 비용이 발생한다. Multi-thread는 메모리 공유로 통신이 효율적이고 생성 비용이 낮지만, 하나의 Thread 오류가 전체 Process를 종료시킬 수 있고 동기화 문제가 발생한다. 상황에 따른 선택 근거를 논리적으로 제시할 수 있어야 한다.

## Level 3: 심화

### 핵심 질문
- Context switching이 발생할 때 OS는 어떤 작업을 수행하며, Thread switching과 Process switching의 비용 차이는 왜 발생하는가?

### 꼬리 질문
- PCB(Process Control Block)와 TCB(Thread Control Block)에는 어떤 정보가 저장되며 각각의 역할은 무엇인가?
- Context switching 비용을 줄이기 위한 기법에는 어떤 것이 있는가?

### 예상 이해 수준
Context switching 시 OS는 현재 실행 중인 프로세스/스레드의 상태(Register, Program Counter 등)를 PCB/TCB에 저장하고 다음 실행 대상의 상태를 복원한다. Process switching은 메모리 주소 공간이 바뀌므로 TLB flush가 필요해 Thread switching보다 비용이 크다. PCB에는 PID, 메모리 정보, 파일 디스크립터 등이, TCB에는 Thread ID, Stack pointer, Register 값이 저장된다는 것을 설명할 수 있어야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- 대규모 Web server를 설계할 때 Process per request, Thread pool, Event-driven 모델 중 어떤 것을 선택하고 어떻게 구성하겠는가?

### 꼬리 질문
- Thread pool의 크기를 결정하는 기준은 무엇인가? CPU-bound와 I/O-bound 작업에서 각각 어떻게 다르게 설정하는가?
- Event-driven 모델이 C10K 문제를 어떻게 해결하는지 설명하라.

### 예상 이해 수준
각 모델의 특성을 이해하고 트레이드오프를 바탕으로 설계 결정을 내릴 수 있어야 한다. Process per request는 격리성이 높지만 확장성이 낮다. Thread pool은 생성 비용을 절감하지만 Thread 수 튜닝이 필요하며, CPU core 수와 I/O 대기 비율을 고려해 크기를 결정한다. Event-driven은 단일 Thread로 수만 개의 연결을 처리할 수 있어 I/O 집약적 서비스에 유리하지만 CPU-bound 작업에는 부적합하다. 실제 요구사항을 분석해 혼합 방식(예: Worker thread pool + event loop)을 제안할 수 있으면 우수하다.

## 현실 연결
- Chrome 브라우저는 탭마다 별도의 Process를 사용해 하나의 탭 충돌이 다른 탭에 영향을 미치지 않도록 한다 (Multi-process 아키텍처).
- Java의 ExecutorService와 Thread pool은 Thread 생성/소멸 비용을 줄이고 동시 요청을 효율적으로 처리하기 위해 사용된다.
- Node.js는 Single-thread Event loop 모델을 사용해 비동기 I/O를 처리하며, CPU-intensive 작업은 Worker thread로 분리한다.

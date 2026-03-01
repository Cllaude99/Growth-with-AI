# Synchronization

## 메타데이터
- **카테고리**: 운영체제
- **난이도**: 고급
- **우선순위**: 높음
- **선수과목**: Process & Thread
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- Race condition이란 무엇이며, 왜 Multi-thread 환경에서 심각한 문제가 되는가?

### 꼬리 질문
- Critical section이란 무엇이며, 이를 보호하기 위한 조건 세 가지(Mutual exclusion, Progress, Bounded waiting)는 각각 무엇을 의미하는가?
- 단순한 flag 변수를 사용한 동기화가 왜 올바르게 동작하지 않는가?

### 예상 이해 수준
Race condition은 여러 Thread가 공유 자원에 동시에 접근하고 그 순서에 따라 결과가 달라지는 현상이다. 예를 들어 두 Thread가 동시에 카운터를 증가시킬 때 read-modify-write 연산이 atomic하지 않으면 증가 횟수가 유실된다. Critical section은 공유 자원에 접근하는 코드 구간으로, 한 번에 하나의 Thread만 진입해야 한다(Mutual exclusion). 단순 flag는 컴파일러나 CPU의 명령어 재배열(reordering)로 인해 의도대로 동작하지 않을 수 있다는 것을 설명할 수 있어야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- Mutex, Semaphore, Monitor의 차이는 무엇이며, 각각 어떤 상황에서 적합한가?

### 꼬리 질문
- Binary semaphore와 Mutex는 기능적으로 유사하지만 어떤 중요한 차이가 있는가?
- Spinlock과 Blocking lock의 트레이드오프는 무엇이며, 각각 언제 사용하는가?

### 예상 이해 수준
Mutex는 소유권(ownership)이 있어 잠근 Thread만이 해제할 수 있으며 상호 배제에 사용한다. Semaphore는 카운터 기반으로 소유권 개념이 없어 다른 Thread가 signal을 보낼 수 있으며, 특정 개수의 자원 풀을 관리하거나 Thread 간 순서 제어에 사용한다. Monitor는 Java의 synchronized처럼 언어 수준에서 Mutex와 Condition variable을 통합한 고수준 동기화 구조다. Spinlock은 짧은 대기에서 context switching 없이 CPU를 점유해 효율적이고, Blocking lock은 대기 시간이 길 때 CPU를 양보하는 것이 적합하다는 것을 설명할 수 있어야 한다.

## Level 3: 심화

### 핵심 질문
- Producer-consumer 문제와 Reader-writer 문제를 Semaphore나 Monitor로 어떻게 해결하며, 각 솔루션의 한계는 무엇인가?

### 꼬리 질문
- Reader-writer 문제에서 Reader 우선 방식과 Writer 우선 방식의 트레이드오프는 무엇인가?
- Producer-consumer에서 bounded buffer를 사용할 때 세 개의 Semaphore(mutex, empty, full)가 각각 왜 필요한가?

### 예상 이해 수준
Producer-consumer 문제에서 bounded buffer를 사용할 때 empty(빈 슬롯 수)와 full(채워진 슬롯 수) semaphore로 동기화하고 mutex로 buffer 접근을 보호한다. Reader-writer 문제에서 Reader 우선 방식은 읽기 성능이 높지만 Writer starvation이 발생할 수 있고, Writer 우선 방식은 반대의 트레이드오프를 가진다. 실제 시스템에서는 Fair read-write lock으로 도착 순서를 보장하는 방식을 사용한다. 각 솔루션의 코드 흐름을 논리적으로 설명할 수 있어야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- Lock-free data structure는 무엇이며, CAS(Compare-And-Swap) 연산을 사용해 어떻게 Thread 안전성을 보장하는가?

### 꼬리 질문
- ABA 문제란 무엇이며, Lock-free 알고리즘에서 어떻게 해결하는가?
- Lock-free와 Lock-based 방식 중 어떤 상황에서 각각을 선택하는가?

### 예상 이해 수준
CAS는 "현재 값이 예상 값과 같으면 새 값으로 교체한다"는 atomic 연산으로, 성공/실패 결과를 반환한다. Lock-free 자료구조(예: Lock-free queue, ConcurrentHashMap)는 Mutex 없이 CAS 루프로 공유 상태를 갱신하여 lock 경쟁에 의한 병목을 제거한다. ABA 문제는 값이 A→B→A로 변했을 때 CAS가 변화를 감지하지 못하는 현상으로, versioned pointer나 Hazard pointer로 해결한다. Lock-free는 고경쟁 환경에서 성능이 좋지만 구현이 복잡하고, lock-based는 정확성 검증이 용이하다는 트레이드오프를 설명할 수 있으면 우수하다.

## 현실 연결
- Database의 트랜잭션 lock(행 잠금, 테이블 잠금)은 동시 쓰기 시 데이터 무결성을 보장하며, Deadlock 감지 시 트랜잭션을 rollback한다.
- Java의 synchronized 키워드와 ReentrantLock은 JVM Monitor를 사용하며, java.util.concurrent 패키지의 AtomicInteger는 CAS 기반 lock-free 연산을 제공한다.
- Go의 channel은 CSP(Communicating Sequential Processes) 모델로, 공유 메모리 대신 메시지 패싱으로 동기화 문제를 회피한다("Don't communicate by sharing memory; share memory by communicating").

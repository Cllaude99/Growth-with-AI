# CPU Scheduling

## 메타데이터
- **카테고리**: 운영체제
- **난이도**: 중급
- **우선순위**: 중간
- **선수과목**: Process & Thread
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- CPU scheduling의 목적은 무엇이며, FCFS와 SJF 알고리즘은 각각 어떻게 동작하는가?

### 꼬리 질문
- CPU scheduling의 성능 지표(CPU utilization, Throughput, Turnaround time, Waiting time, Response time)는 각각 무엇을 측정하는가?
- SJF가 평균 대기 시간을 최소화한다는 것을 간단한 예시로 설명하라.

### 예상 이해 수준
CPU scheduling은 한정된 CPU 자원을 여러 Process에 효율적으로 배분하여 CPU utilization을 높이고 Response time을 낮추는 것이 목적이다. FCFS는 먼저 도착한 Process를 먼저 처리하는 단순한 방식이지만 Convoy effect(긴 작업이 짧은 작업을 지연시키는 현상)가 발생한다. SJF는 남은 실행 시간이 가장 짧은 Process를 선택해 평균 대기 시간을 최소화하지만, 실행 시간을 미리 알기 어렵다는 한계가 있다는 것을 설명할 수 있어야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- Preemptive scheduling과 Non-preemptive scheduling의 트레이드오프는 무엇이며, Round Robin의 time quantum 크기는 성능에 어떤 영향을 미치는가?

### 꼬리 질문
- Round Robin에서 time quantum이 너무 크거나 너무 작을 때 각각 어떤 문제가 발생하는가?
- Preemptive scheduling이 반드시 필요한 상황은 어떤 경우인가?

### 예상 이해 수준
Non-preemptive는 실행 중인 Process가 자발적으로 CPU를 반납할 때까지 기다리므로 오버헤드가 적지만 긴 작업이 전체 응답성을 저해한다. Preemptive는 OS가 강제로 CPU를 회수할 수 있어 응답성이 좋지만 context switching 비용이 발생한다. Round Robin의 time quantum이 너무 크면 FCFS에 가까워지고, 너무 작으면 context switching 오버헤드가 처리 시간을 지배한다. 일반적으로 context switching 시간의 10-100배 수준으로 설정한다는 것을 설명할 수 있어야 한다.

## Level 3: 심화

### 핵심 질문
- Priority scheduling에서 starvation 문제가 발생하는 원인은 무엇이며, aging 기법은 이를 어떻게 해결하는가?

### 꼬리 질문
- Priority inversion이란 무엇이며 실시간 시스템에서 왜 치명적인가? Priority inheritance로 어떻게 해결하는가?
- Interactive process와 Batch process를 동시에 효율적으로 처리하려면 어떤 전략이 필요한가?

### 예상 이해 수준
Priority scheduling에서 낮은 우선순위의 Process는 높은 우선순위 Process가 계속 도착하면 영원히 실행되지 못하는 starvation이 발생한다. Aging은 대기 시간이 길어질수록 우선순위를 점진적으로 높여 starvation을 방지한다. Priority inversion은 낮은 우선순위 Process가 자원을 점유한 채 높은 우선순위 Process를 간접적으로 블로킹하는 현상으로, Mars Pathfinder 사례와 같이 실시간 시스템에서 심각한 문제를 일으킨다. Priority inheritance로 자원 보유 Process의 우선순위를 일시적으로 높여 해결한다는 것을 설명할 수 있어야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- Multi-level feedback queue scheduling은 어떻게 동작하며, 실시간(Real-time) 시스템에서의 scheduling 요구사항은 일반 시스템과 어떻게 다른가?

### 꼬리 질문
- Linux의 CFS(Completely Fair Scheduler)는 어떤 문제를 해결하기 위해 설계되었으며 어떻게 공정성을 보장하는가?
- Cloud 환경에서 VM들 간 CPU 자원을 배분할 때 고려해야 하는 요소들은 무엇인가?

### 예상 이해 수준
Multi-level feedback queue는 여러 개의 큐를 두고 Process의 CPU 사용 패턴에 따라 동적으로 큐를 이동시킨다. I/O-bound Process는 높은 우선순위 큐에, CPU-bound Process는 낮은 우선순위 큐에 배치되어 응답성과 처리량을 동시에 최적화한다. Real-time scheduling은 Deadline을 반드시 지켜야 하므로 EDF(Earliest Deadline First)나 Rate Monotonic 같은 알고리즘을 사용한다. Linux CFS는 각 Process에 vruntime을 부여하고 red-black tree로 관리해 O(log n) 복잡도로 공정한 CPU 시간 분배를 구현한다는 것을 설명할 수 있으면 우수하다.

## 현실 연결
- Linux의 CFS(Completely Fair Scheduler)는 cgroup을 통해 컨테이너별 CPU 할당량을 제한하며, Kubernetes의 CPU request/limit이 이를 기반으로 동작한다.
- Windows의 Task Manager에서 Process 우선순위를 변경하면 scheduling 큐 내 위치가 달라져 CPU 할당 비율에 영향을 미친다.
- Cloud provider(AWS, GCP)의 vCPU는 물리 CPU를 시분할로 공유하며, CPU credit(AWS T-type 인스턴스) 방식으로 burst를 허용하는 scheduling 전략을 사용한다.

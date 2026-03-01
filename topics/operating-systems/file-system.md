# File System

## 메타데이터
- **카테고리**: 운영체제
- **난이도**: 중급
- **우선순위**: 중간
- **선수과목**: Memory Management
- **예상 시간**: 30-45분

## Level 1: 기본 이해

### 핵심 질문
- File system이란 무엇이며, 어떤 계층 구조로 이루어져 있는가?

### 꼬리 질문
- File system이 없다면 데이터를 디스크에 저장하고 읽는 과정이 어떻게 달라지는가?
- File descriptor란 무엇이며, OS는 이를 어떻게 관리하는가?

### 예상 이해 수준
File system은 OS가 데이터를 디스크에 구조적으로 저장하고 효율적으로 읽고 쓸 수 있도록 하는 체계로, 파일의 이름, 위치, 권한, 메타데이터를 관리한다. 계층 구조는 사용자 인터페이스(파일 경로) → VFS(Virtual File System) 추상화 계층 → 구체적인 파일 시스템 구현(ext4, NTFS) → Block layer → 실제 디바이스로 이루어진다. File descriptor는 열린 파일에 대한 OS 수준의 정수형 핸들로, Process마다 파일 디스크립터 테이블이 있으며 0(stdin), 1(stdout), 2(stderr)는 기본으로 할당된다는 것을 설명할 수 있어야 한다.

## Level 2: 트레이드오프

### 핵심 질문
- FAT(File Allocation Table) 기반 파일 시스템과 inode 기반 파일 시스템의 구조적 차이와 트레이드오프는 무엇인가?

### 꼬리 질문
- FAT 방식에서 파일의 마지막 블록을 찾으려면 어떤 과정을 거쳐야 하는가?
- inode 기반 시스템에서 파일 이름과 파일 데이터가 분리되어 저장되는 것이 어떤 이점을 가져다주는가?

### 예상 이해 수준
FAT는 파일 블록의 연결 정보를 중앙 테이블(FAT)에 저장하는 방식으로 구조가 단순해 임베디드 기기나 USB에 적합하지만, 파일 크기가 크거나 디렉터리가 많으면 성능이 저하되고 파일 권한 관리가 어렵다. inode 기반(ext4, APFS)은 각 파일마다 inode 구조체에 메타데이터(권한, 크기, 블록 포인터)를 저장하고, 디렉터리는 파일 이름과 inode 번호의 매핑만 보유한다. 이로 인해 hard link(같은 inode를 가리키는 여러 이름) 구현이 자연스럽고, 권한 관리 및 대규모 파일 시스템 관리에 유리하다는 것을 설명할 수 있어야 한다.

## Level 3: 심화

### 핵심 질문
- Journaling은 무엇이며 파일 시스템의 일관성을 어떻게 보장하는가? Soft link와 Hard link는 어떻게 다른가?

### 꼬리 질문
- Journaling의 세 가지 모드(writeback, ordered, data)의 차이는 무엇이며 각각 어떤 트레이드오프를 가지는가?
- Hard link는 디렉터리에 생성할 수 없는 이유는 무엇이며, 다른 파일 시스템에 걸쳐 생성할 수 없는 이유는 무엇인가?

### 예상 이해 수준
Journaling은 파일 시스템 변경 사항을 적용하기 전에 별도의 로그(journal)에 먼저 기록하는 방식으로, 전원 차단이나 크래시 발생 시 journal을 재실행(redo)하거나 취소(undo)하여 일관성을 복구한다. Soft link(Symbolic link)는 다른 파일의 경로를 저장하는 별도 파일로, 원본 삭제 시 dangling link가 된다. Hard link는 동일한 inode를 가리키는 새로운 디렉터리 항목으로, 원본을 삭제해도 inode의 reference count가 0이 될 때까지 데이터가 유지된다. Hard link는 inode 번호로 동작하므로 다른 파일 시스템에 걸쳐 생성할 수 없고, 순환 구조 방지를 위해 디렉터리에는 허용하지 않는다는 것을 설명할 수 있어야 한다.

## Level 4: 시스템 설계

### 핵심 질문
- Distributed file system(GFS, HDFS)은 단일 노드 파일 시스템과 어떻게 다르며, 어떤 설계 결정이 대규모 분산 환경에서의 요구사항을 충족하는가?

### 꼬리 질문
- GFS/HDFS에서 파일을 큰 청크(64MB~128MB)로 나누는 이유는 무엇이며, 이로 인해 어떤 워크로드에는 부적합한가?
- HDFS의 NameNode는 SPOF(Single Point of Failure)인데, 이를 어떻게 해결하는가?

### 예상 이해 수준
GFS와 HDFS는 수천 개의 범용 서버에 걸쳐 페타바이트 규모의 데이터를 저장하도록 설계되었다. 주요 설계 원칙은 대규모 순차 읽기를 위해 큰 청크 크기를 사용하고, 복제(replication)로 장애 내성을 확보하며, 중앙 Master(NameNode)가 메타데이터를 관리하고 Chunkserver(DataNode)가 실제 데이터를 저장하는 것이다. 큰 청크 크기는 메타데이터 오버헤드를 줄이고 순차 처리에 최적화되지만 작은 파일 처리에 비효율적이다. NameNode SPOF는 Secondary NameNode, Standby NameNode, ZooKeeper 기반 HA 구성으로 해결한다. CAP 이론의 관점에서 HDFS가 어떤 트레이드오프를 선택했는지 논할 수 있으면 우수하다.

## 현실 연결
- Linux의 ext4는 Journaling을 기본으로 지원하는 inode 기반 파일 시스템으로, 대부분의 Linux 서버와 Android 기기에서 사용된다.
- Windows의 NTFS는 inode에 해당하는 MFT(Master File Table)를 사용하며, Access Control List로 세밀한 권한 관리를 지원한다.
- Amazon S3는 파일 시스템이 아닌 Object storage로, 계층적 디렉터리 구조 없이 key-value 방식으로 데이터를 저장하며, HDFS와 함께 빅데이터 분석의 주요 스토리지로 사용된다.

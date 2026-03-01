# Growth-with-AI

AI 시대에 단순히 AI에게 답을 받는 것이 아닌, **스스로 문제를 고민하고 해결하는 역량**을 키우기 위한 소크라테스식 학습 시스템.

Claude Code의 CLAUDE.md와 슬래시 커맨드를 활용하여 AI가 답을 알려주는 대신 **질문을 통해 스스로 깨달음에 도달**하도록 이끕니다.

## 학습 철학

- **답을 주지 않는다**: AI는 답을 직접 알려주지 않고, 질문으로 사고를 유도합니다.
- **소크라테스식 대화**: 한 번에 하나의 질문, 2-3문장 이내로 간결하게.
- **점진적 깊이**: 기본 이해 → 트레이드오프 → 엣지케이스 → 시스템 설계로 단계적 심화.
- **막히면 도와준다**: 여러 번 막히면 힌트 → 이지선다 → 간단 설명 순으로 스캐폴딩 제공.
- **발견을 축하한다**: 올바른 이해에 도달하면 구체적으로 칭찬하고, 더 넓은 개념으로 연결.
- **검증 후 통과**: 꼬리 질문과 엣지케이스로 진짜 이해인지 검증한 후에만 "통과"로 기록.

## 사용법

### 준비

[Claude Code](https://docs.anthropic.com/en/docs/claude-code)가 설치되어 있어야 합니다.

```bash
cd Growth-with-AI
claude
```

### CS 개념 학습

```
> /cs-learn
> "해시 테이블 공부하고 싶어요"
```

6단계 프로토콜(이해 확인 → 깊이 탐구 → 엣지케이스 → 트레이드오프 → 현실 연결 → 종합)을 따라 소크라테스식 질문으로 깊은 이해에 도달합니다. 각 레벨 완료 시 **검증 게이트**를 통과해야 다음 레벨로 진행됩니다.

### 테마 기반 랜덤 드릴

```
> /cs-theme
> "7" (웹 & 프론트엔드 선택)
```

7개 카테고리 중 하나를 선택하면 해당 테마의 토픽들에서 랜덤으로 질문을 던집니다. 이전 학습 기록을 참조하여 이미 마스터한 레벨은 건너뛰고 다음 레벨부터 출제합니다. 토픽당 1-2개 질문 후 빠르게 전환하는 드릴 형식입니다.

### 디버깅 학습

```
> /debug
> "NullPointerException이 발생했는데..."
```

5단계 프로토콜(이해 → 에러 읽기 → 범위 좁히기 → 종합 → 축하 & 일반화)을 따라 스스로 버그의 원인을 찾아갑니다.

### 복습

```
> /review
```

간격 반복(Spaced Repetition) 기반으로 복습이 필요한 토픽을 자동 추천합니다. "학습중" 상태(검증 미통과) 토픽을 우선 추천하여 재도전 기회를 제공합니다.
- 3일+ → 빠른 복습
- 7일+ → 표준 복습
- 14일+ → 심층 복습

### 진행 현황 확인

```
> /progress
```

학습 통계, 7개 카테고리별 커버리지, 각 토픽의 통과/학습중 상태, 추천 토픽을 확인할 수 있습니다.

## 통과 검증 시스템

단순히 학습한 것으로 끝나지 않고, **진짜 이해했는지 검증**한 후에만 통과로 기록합니다.

| 상태 | 의미 |
|------|------|
| `-` | 미학습 |
| `학습중` | 학습했으나 검증 미통과 |
| `통과` | 검증 완료 |

- **0~1회 힌트**: 검증 질문 통과 시 PASS
- **2회 힌트**: 추가 검증 질문 1개 후 PASS 가능
- **3회+ 힌트**: "학습중"으로 기록 (다음에 재도전)

모든 기록은 `progress/log.md`에 파일로 저장되어 **세션이 종료되어도 영구 보존**됩니다.

## 다루는 토픽 (50개)

| 카테고리 | 토픽 수 | 주요 내용 |
|---------|--------|----------|
| 자료구조 & 알고리즘 | 10 | Array, Hash Table, Tree, Sorting, Graph, DP 등 |
| 운영체제 | 7 | Process/Thread, Memory, Scheduling, Synchronization 등 |
| 네트워크 | 7 | TCP/UDP, HTTP/HTTPS, REST API, WebSocket, JWT 등 |
| 데이터베이스 | 6 | Indexing, Transaction, JOIN, NoSQL, Query Optimization 등 |
| 시스템 설계 | 6 | Caching, Load Balancing, CAP, Message Queue 등 |
| 소프트웨어 공학 | 6 | OOP, SOLID, Design Patterns, Testing, CI/CD 등 |
| 웹 & 프론트엔드 | 8 | Event Loop, Closure, Prototype, Browser Rendering, Virtual DOM, Web Security, Performance, Module Systems |

## 프로젝트 구조

```
Growth-with-AI/
├── CLAUDE.md                          # 소크라테스식 페르소나 & 규칙 & 검증 프로토콜
├── README.md                          # 프로젝트 소개
├── .claude/commands/
│   ├── cs-learn.md                    # /project:cs-learn 커맨드
│   ├── cs-theme.md                    # /project:cs-theme 커맨드
│   ├── debug.md                       # /project:debug 커맨드
│   ├── progress.md                    # /project:progress 커맨드
│   └── review.md                      # /project:review 커맨드
├── topics/                            # CS 토픽별 질문 뱅크
│   ├── data-structures-algorithms/    # 10개 토픽
│   ├── operating-systems/             # 7개 토픽
│   ├── network/                       # 7개 토픽
│   ├── database/                      # 6개 토픽
│   ├── system-design/                 # 6개 토픽
│   ├── software-engineering/          # 6개 토픽
│   └── web-frontend/                  # 8개 토픽
├── progress/
│   └── log.md                         # 학습 진행 기록 (자동 업데이트, 영구 보존)
└── debug-cases/
    ├── _template.md                   # 디버깅 시나리오 템플릿
    └── examples/                      # 예시 시나리오
        ├── null-pointer-exception.md
        ├── off-by-one-error.md
        └── async-race-condition.md
```

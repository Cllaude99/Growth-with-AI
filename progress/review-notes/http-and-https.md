# HTTP & HTTPS - 복습 노트

## 최종 학습일: 2026-03-15
## 판정: 통과 (Level 4, 3회 도전)

## Level 1: 기본 이해 (통과)

### 주요 질문
- HTTP의 동작 원리와 request/response 구조는?
- Header와 Body의 역할 차이는?
- HTTP가 stateless하다는 의미와 이로 인한 문제는?
- (검증) 200, 404, 500 상태 코드의 의미는?

### 다시 공부할 내용
- 특별히 없음.

## Level 2: 트레이드오프 (통과)

### 주요 질문
- HTTP/1.1, HTTP/2, HTTP/3의 차이와 각 버전이 해결한 문제는?
- (검증) HTTP/2가 헤더 압축을 도입한 이유는?

### 다시 공부할 내용
- 파이프라이닝은 HTTP/1.1 기능임 (HTTP/2가 아님) — 혼동했음
- HTTP/2의 server push 기능과 잘 사용되지 않는 이유

## Level 3: 심화 (2차 도전에서 통과)

### 주요 질문
- HTTP vs HTTPS의 차이는?
- 비대칭 암호화와 대칭 암호화의 역할 차이는?
- CA(Certificate Authority) 인증서 체계란?
- (검증) TLS에서 비대칭/대칭 암호화를 함께 사용하는 이유는?

### 다시 공부할 내용
- **HTTPS = HTTP + TLS** (SSH와 혼동하지 말 것)
- **비대칭 암호화 → 키 교환** (안전), **대칭 암호화 → 데이터 전송** (빠름) — 역할 혼동 주의
- TLS handshake 세부 단계: ClientHello → ServerHello → 인증서 검증 → 키 교환 → session key 생성
- TLS 1.2 vs TLS 1.3의 handshake 차이

## Level 4: 시스템 설계 (3차 도전에서 통과)

### 주요 질문
- TLS termination이란?
- CDN과 원본 서버 사이의 보안 유지 방법은?
- (검증) SSL stripping 공격 방어 방법은?

### 다시 공부할 내용
- **HSTS**: 서버 응답 헤더로 브라우저에 HTTPS 강제
- **TOFU 문제**: 최초 접속 시 HTTP 노출 가능
- **HSTS Preload List**: hstspreload.org에 등록하면 브라우저 소스코드에 하드코딩, 최초 접속부터 HTTPS 강제

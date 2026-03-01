# NullPointerException in User Service

## 시나리오 정보
- **제목**: 사용자 프로필 조회 시 NullPointerException
- **난이도**: 초급
- **관련 개념**: Null safety, Defensive programming, Optional
- **언어/환경**: Java / Spring Boot

## 상황 설명

사용자 프로필 페이지에서 가끔씩 500 에러가 발생합니다. 모든 사용자에게 발생하는 것은 아니고, 특정 사용자의 프로필을 볼 때만 발생합니다. 최근에 사용자 등록 로직을 변경한 후부터 이 문제가 시작되었습니다.

## 에러 메시지 / 증상

```
java.lang.NullPointerException
    at com.example.service.UserService.getUserProfile(UserService.java:25)
    at com.example.controller.UserController.getProfile(UserController.java:18)
    at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
```

## 문제 코드

```java
public class UserService {

    public UserProfileDto getUserProfile(Long userId) {
        User user = userRepository.findById(userId).orElseThrow();

        // Line 25: 여기서 NPE 발생
        String displayName = user.getProfile().getNickname().toUpperCase();

        return UserProfileDto.builder()
            .id(user.getId())
            .displayName(displayName)
            .email(user.getEmail())
            .bio(user.getProfile().getBio())
            .build();
    }
}
```

## 소크라테스식 가이드 질문

### 1단계: 이해
- 이 코드가 어떤 동작을 하길 기대한 건가요?
- Line 25에서 어떤 연산들이 순서대로 일어나고 있나요?

### 2단계: 에러 읽기
- NullPointerException은 어떤 상황에서 발생하는 에러인가요?
- "모든 사용자"가 아니라 "특정 사용자"에게만 발생한다는 것은 뭘 의미할까요?

### 3단계: 범위 좁히기
- Line 25에서 `.` 연산자가 몇 번 사용되고 있나요? 각각 어떤 객체의 메서드를 호출하나요?
- 이 중에서 null이 될 수 있는 것은 어떤 것일까요?
- user 객체는 null일 수 있나요? (orElseThrow()를 사용하고 있으니까요)

### 4단계: 종합
- getProfile()이 null을 반환하는 사용자가 있다면, 그건 어떤 사용자일까요?
- "최근에 사용자 등록 로직을 변경했다"는 힌트가 어떤 의미일까요?

### 5단계: 일반화
- 이런 종류의 NPE를 미리 방지하려면 어떤 패턴을 사용할 수 있을까요?
- method chaining (a.b().c().d())을 사용할 때 주의할 점은 뭘까요?

## 근본 원인

최근 사용자 등록 로직 변경 후 Profile 없이 User만 생성되는 케이스가 생겼다. `user.getProfile()`이 null을 반환하고, null에 대해 `.getNickname()`을 호출하면서 NPE 발생.

## 해결 방법

```java
public UserProfileDto getUserProfile(Long userId) {
    User user = userRepository.findById(userId).orElseThrow();

    String displayName = Optional.ofNullable(user.getProfile())
        .map(Profile::getNickname)
        .map(String::toUpperCase)
        .orElse("Anonymous");

    String bio = Optional.ofNullable(user.getProfile())
        .map(Profile::getBio)
        .orElse("");

    return UserProfileDto.builder()
        .id(user.getId())
        .displayName(displayName)
        .email(user.getEmail())
        .bio(bio)
        .build();
}
```

## 관련 학습 토픽
- `topics/software-engineering/clean-code.md` - Defensive programming
- `topics/software-engineering/oop-principles.md` - Null Object pattern

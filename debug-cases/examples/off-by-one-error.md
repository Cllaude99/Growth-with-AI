# Off-by-One Error in Pagination

## 시나리오 정보
- **제목**: 게시글 목록 페이지네이션에서 마지막 페이지 데이터 누락
- **난이도**: 초급
- **관련 개념**: Array indexing, Pagination, Boundary conditions
- **언어/환경**: Python / FastAPI

## 상황 설명

게시글 목록 API에서 페이지네이션을 구현했는데, 총 10개의 게시글이 있고 한 페이지에 3개씩 보여줄 때, 마지막 페이지(4페이지)에서 10번째 게시글이 보이지 않습니다.

## 에러 메시지 / 증상

```
# 기대 결과
Page 1: [1, 2, 3]
Page 2: [4, 5, 6]
Page 3: [7, 8, 9]
Page 4: [10]

# 실제 결과
Page 1: [1, 2, 3]
Page 2: [4, 5, 6]
Page 3: [7, 8, 9]
Page 4: []           # 비어있음!
```

## 문제 코드

```python
def get_posts(page: int, page_size: int = 3):
    total_posts = Post.query.count()  # 10
    total_pages = total_posts // page_size  # 10 // 3 = 3

    if page > total_pages:
        return {"posts": [], "total_pages": total_pages}

    offset = (page - 1) * page_size
    posts = Post.query.offset(offset).limit(page_size).all()

    return {
        "posts": posts,
        "total_pages": total_pages,
        "current_page": page
    }
```

## 소크라테스식 가이드 질문

### 1단계: 이해
- 이 코드가 하려는 것을 본인의 말로 설명해볼 수 있나요?
- page=4, page_size=3일 때 각 변수의 값이 어떻게 되나요?

### 2단계: 에러 읽기
- 에러 메시지가 아닌 "빈 배열"이 반환되는 건, 어느 조건문에서 걸린 것일까요?
- total_pages가 3으로 계산되는데, 실제로는 4페이지까지 있어야 하죠?

### 3단계: 범위 좁히기
- `10 // 3`은 몇인가요? 이 계산이 올바른 전체 페이지 수를 나타내나요?
- 나머지가 있을 때, 즉 데이터가 딱 나누어떨어지지 않을 때 어떻게 해야 할까요?
- 정확히 9개의 게시글이 있다면 이 코드가 정상 작동할까요?

### 4단계: 종합
- 정수 나눗셈으로 페이지 수를 계산할 때 나머지를 어떻게 처리해야 할까요?
- 올림 나눗셈을 하는 방법을 알고 있나요?

### 5단계: 일반화
- 이런 off-by-one 에러를 방지하려면 어떤 테스트 케이스를 작성해야 할까요?
- 경계값(boundary) 테스트가 왜 중요한지 이해가 되나요?

## 근본 원인

`total_posts // page_size`는 정수 나눗셈(내림)을 수행하므로 나머지가 있는 경우 실제 필요한 페이지 수보다 1 적게 계산된다. 10 // 3 = 3이지만, 실제로는 4페이지가 필요.

## 해결 방법

```python
import math

def get_posts(page: int, page_size: int = 3):
    total_posts = Post.query.count()
    total_pages = math.ceil(total_posts / page_size)  # ceil(10/3) = 4

    if page > total_pages or page < 1:
        return {"posts": [], "total_pages": total_pages}

    offset = (page - 1) * page_size
    posts = Post.query.offset(offset).limit(page_size).all()

    return {
        "posts": posts,
        "total_pages": total_pages,
        "current_page": page
    }
```

## 관련 학습 토픽
- `topics/data-structures-algorithms/array-and-linked-list.md` - Array indexing
- `topics/data-structures-algorithms/searching.md` - Binary search의 off-by-one

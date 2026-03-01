# Async Race Condition in Shopping Cart

## 시나리오 정보
- **제목**: 장바구니 수량 업데이트 시 데이터 불일치
- **난이도**: 중급
- **관련 개념**: Race condition, Concurrency, Atomic operation
- **언어/환경**: JavaScript / Node.js + Express

## 상황 설명

온라인 쇼핑몰에서 사용자가 장바구니의 상품 수량을 빠르게 여러 번 클릭하면, 최종 수량이 기대값과 다르게 저장됩니다. 예를 들어 수량 1에서 "+" 버튼을 3번 빠르게 누르면 4가 되어야 하는데, 2나 3이 되는 경우가 있습니다.

## 에러 메시지 / 증상

```
# 기대 동작: 1 → 2 → 3 → 4
# 실제 동작: 1 → 2 (최종값이 2 또는 3)

# 서버 로그
[14:30:01.100] GET /cart/item/123 → quantity: 1
[14:30:01.102] GET /cart/item/123 → quantity: 1
[14:30:01.103] GET /cart/item/123 → quantity: 1
[14:30:01.200] PUT /cart/item/123 → quantity: 2
[14:30:01.201] PUT /cart/item/123 → quantity: 2
[14:30:01.202] PUT /cart/item/123 → quantity: 2
```

## 문제 코드

```javascript
// Cart Controller
app.post('/cart/:itemId/increment', async (req, res) => {
    const { itemId } = req.params;

    // 현재 수량 조회
    const cartItem = await CartItem.findById(itemId);

    // 수량 증가
    const newQuantity = cartItem.quantity + 1;

    // 업데이트
    await CartItem.updateOne(
        { _id: itemId },
        { quantity: newQuantity }
    );

    res.json({ quantity: newQuantity });
});
```

## 소크라테스식 가이드 질문

### 1단계: 이해
- 이 API가 호출될 때 어떤 단계를 거쳐서 수량이 업데이트되나요?
- "빠르게 여러 번 클릭"하면 서버에서는 어떤 일이 벌어질까요?

### 2단계: 에러 읽기
- 서버 로그를 보면 GET 요청 3개가 거의 동시에 들어왔어요. 이 3개의 요청이 각각 읽은 quantity 값은 뭘까요?
- 3개의 PUT 요청이 모두 quantity: 2로 업데이트하고 있는데, 왜 그럴까요?

### 3단계: 범위 좁히기
- "읽기"와 "쓰기" 사이에 시간 간격이 있다는 게 문제의 핵심인가요?
- 만약 이 코드가 동기적(순차적)으로 실행된다면 문제가 발생할까요?
- 이런 문제를 CS에서 뭐라고 부르는지 알고 있나요?

### 4단계: 종합
- 이 race condition을 해결하려면 "읽기 → 증가 → 쓰기"를 어떻게 바꿔야 할까요?
- 데이터베이스 레벨에서 이 문제를 해결할 수 있는 방법이 있을까요?

### 5단계: 일반화
- 이 패턴(read-modify-write)에서 race condition이 발생하는 다른 예시를 떠올려볼 수 있나요?
- 이런 문제를 미리 감지할 수 있는 테스트를 어떻게 작성할까요?

## 근본 원인

Read-Modify-Write 패턴에서의 Race Condition. 여러 요청이 동시에 같은 값(quantity: 1)을 읽고, 각각 +1 하여 같은 값(2)으로 업데이트하므로 Lost Update 발생.

## 해결 방법

```javascript
// 방법 1: Atomic Operation (MongoDB의 $inc 사용)
app.post('/cart/:itemId/increment', async (req, res) => {
    const { itemId } = req.params;

    const result = await CartItem.findOneAndUpdate(
        { _id: itemId },
        { $inc: { quantity: 1 } },
        { new: true }
    );

    res.json({ quantity: result.quantity });
});

// 방법 2: Optimistic Locking (version 필드 활용)
app.post('/cart/:itemId/increment', async (req, res) => {
    const { itemId } = req.params;

    let updated = false;
    while (!updated) {
        const cartItem = await CartItem.findById(itemId);
        const result = await CartItem.updateOne(
            { _id: itemId, version: cartItem.version },
            {
                quantity: cartItem.quantity + 1,
                $inc: { version: 1 }
            }
        );
        updated = result.modifiedCount > 0;
    }

    const cartItem = await CartItem.findById(itemId);
    res.json({ quantity: cartItem.quantity });
});
```

## 관련 학습 토픽
- `topics/operating-systems/synchronization.md` - Race condition, Mutex
- `topics/database/transaction-and-acid.md` - Isolation level
- `topics/operating-systems/deadlock.md` - Locking strategies

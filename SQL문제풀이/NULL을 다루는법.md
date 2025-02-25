# SQL 학습 정리 (2025-02-25)

## 학습 문제

### 4. NULL 값 처리와 `IS NOT FALSE` 활용

**문제 설명**  
SQL에서 `NULL` 값은 특별한 의미를 가지며, 비교 연산에서 예상치 못한 결과를 일으킬 수 있다. 특히, `!=` 연산은 `NULL` 값을 `unknown`으로 처리하여 원하는 행이 제외될 수 있다. 이를 방지하기 위해 `IS NOT FALSE`를 활용하는 방법을 알아보자.

**학습 내용**
- `IS NOT FALSE`를 사용하면 `true`와 `unknown` 값을 포함하고, `false`만 제외한다.
- 이를 통해 `NULL` 값이 의도치 않게 누락되는 것을 방지할 수 있다.

**해결 SQL**
```sql
SELECT name
FROM customer
WHERE referee_id != 2 IS NOT FALSE;
```

---

## 학습 포인트

### 공통적으로 배운 내용

1. **SQL의 NULL 비교 특성**
   - `NULL`은 **알 수 없는 값**으로, 어떠한 값과도 직접 비교할 수 없다.
   - `NULL = NULL`도 `false`가 아닌 **`unknown`**으로 평가된다.
   - 예제:
     ```sql
     SELECT name
     FROM customer
     WHERE referee_id != 2;
     ```
     - `referee_id = 1` → ✅ 포함
     - `referee_id = 2` → ❌ 제외
     - `referee_id = NULL` → ⚠️ 제외

2. **`IS NOT FALSE`의 역할**
   - `IS NOT FALSE`는 `true`와 `unknown`을 포함하고, `false`만 제외한다.
   - 이를 활용하면 `NULL` 값을 가진 행도 결과에 포함될 수 있다.
   - 예제:
     ```sql
     SELECT name
     FROM customer
     WHERE referee_id != 2 IS NOT FALSE;
     ```
     - `referee_id = 1` → ✅ 포함
     - `referee_id = 2` → ❌ 제외
     - `referee_id = NULL` → ✅ 포함

3. **명확한 의도 전달**
   - `IS NOT FALSE`는 `NULL` 값을 의도적으로 포함하거나 제외할 수 있는 **의도를 명확히 표현**한다.
   - 이를 통해 데이터 유실을 방지하고, 유지보수 시 코드의 가독성을 높일 수 있다.

4. **실전 예제**
   - 특정 기간 동안 판매되지 않은 제품 찾기:
     ```sql
     SELECT product_id
     FROM Product
     WHERE last_sale_date IS NOT FALSE;
     ```
   - 유저가 자신의 글을 읽지 않은 경우 필터링:
     ```sql
     SELECT article_id, author_id
     FROM Views
     WHERE viewer_id != author_id IS NOT FALSE;
     ```

5. **대체 방법**
   - `IS NOT FALSE` 외에도 `IS NULL`, `IS NOT NULL`을 활용해 더 명확한 조건을 작성할 수 있다.
   - 예제:
     ```sql
     SELECT name
     FROM customer
     WHERE referee_id != 2 OR referee_id IS NULL;
     ```

---

## 결론
- SQL에서 `NULL` 값은 특별한 의미를 가지므로 **명확하게 처리**해야 한다.
- `IS NOT FALSE`는 간단한 조건에서 `NULL` 값을 포함하도록 만들어, 예상치 못한 데이터 누락을 방지할 수 있다.
- 상황에 따라 `IS NULL`, `IS NOT NULL`과 함께 사용하면 더 정밀하게 데이터 처리가 가능하다.

이렇게 하면 `NULL` 처리에 대한 SQL 로직이 더욱 견고해질 것이다! 


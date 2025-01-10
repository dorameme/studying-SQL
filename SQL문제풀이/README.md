# SQL 학습 정리 (2025-01-10)

## 학습 문제

### 1. [Sales Analysis III](https://leetcode.com/problems/sales-analysis-iii/description/)

**문제 설명**  
2019년 1분기(2019-01-01 ~ 2019-03-31) **기간 내에서만** 판매된 제품을 찾아야 함.
제품이 다른 기간에 판매된 기록이 있다면 제외해야 함.

**학습 내용**
- `GROUP BY`와 `HAVING` 절을 사용하여 그룹화된 데이터의 조건을 처리.
- `MIN`과 `MAX` 집계 함수를 활용하여 기간 내의 데이터만 필터링.

**해결 SQL**
```sql
SELECT DISTINCT p.product_id, p.product_name
FROM Product p
JOIN Sales s
ON p.product_id = s.product_id
GROUP BY p.product_id, p.product_name
HAVING MIN(s.sale_date) >= '2019-01-01'
   AND MAX(s.sale_date) <= '2019-03-31';
```

---

### 2. [Article Views I](https://leetcode.com/problems/article-views-i/description/)

**문제 설명**  
유저가 자신의 글을 읽지 않은 경우만 필터링하여 `article_id`와 `author_id`를 반환하는 문제.  
조건: `viewer_id != author_id`.

**학습 내용**
- `WHERE` 절을 사용하여 조건에 맞는 데이터를 필터링.
- 기본적인 SELECT 쿼리로 데이터를 추출.

**해결 SQL**
```sql
SELECT DISTINCT article_id, author_id
FROM Views
WHERE viewer_id != author_id;
```

---

### 3. [Calculate Special Bonus](https://leetcode.com/problems/calculate-special-bonus/description/)

**문제 설명**  
직원 ID가 **홀수**이고, 이름이 **'M'으로 시작하지 않는 경우** 보너스를 지급.  
조건을 만족하는 직원에게는 **급여와 동일한 보너스**를 지급하고, 그렇지 않은 경우 보너스는 `0`.

**학습 내용**
- `IF` 함수를 사용하여 조건에 따라 서로 다른 값을 반환.
- `SUBSTRING` 또는 `LEFT`를 활용하여 문자열의 첫 글자를 확인.
- MySQL에서 문자열 인덱스는 **1부터 시작**함을 이해.

**해결 SQL**
```sql
SELECT 
    employee_id, 
    IF(
        (employee_id % 2 = 1) AND (LEFT(name, 1) != 'M'), 
        salary, 
        0
    ) AS bonus
FROM Employees
ORDER BY employee_id;
```

---

## 학습 포인트

### 공통적으로 배운 내용

1. **MySQL에서는 문자열의 인덱스가 1부터 시작**  
   - MySQL에서 문자열 관련 함수(`SUBSTRING`, `LOCATE`, `LEFT` 등)를 사용할 때 문자열의 첫 글자는 **1번 인덱스**를 사용.
   - 예제:
     ```sql
     SELECT SUBSTRING('Hello', 1, 1); -- 결과: 'H'
     ```

2. **`GROUP BY` + `HAVING` 조건의 동작 방식**  
   - `HAVING` 절은 그룹화된 결과에 조건을 적용하며, 조건에 맞지 않는 **그룹 전체를 제외**.
   - 예제:
     ```sql
     SELECT product_id, MIN(sale_date) AS first_sale
     FROM Sales
     GROUP BY product_id
     HAVING MIN(sale_date) >= '2019-01-01';
     ```

3. **`WHERE EXISTS`는 서브쿼리에 별명이 없어도 됨**  
   - `EXISTS`는 서브쿼리의 반환값이 아닌, 결과 존재 여부(`TRUE`/`FALSE`)를 확인하므로 별명이 필요하지 않음.
   - 예제:
     ```sql
     SELECT product_id
     FROM Product p
     WHERE EXISTS (
         SELECT 1
         FROM Sales s
         WHERE p.product_id = s.product_id
     );
     ```

4. **`UNION`과 `INTERSECT`**  
   - `UNION`: 두 SELECT 문의 **합집합**을 구하며 중복된 데이터를 제거.
     ```sql
     SELECT product_id FROM Sales
     UNION
     SELECT product_id FROM Returns;
     ```
   - `INTERSECT`: 두 SELECT 문의 **교집합**을 구하며 중복된 데이터를 제거(MySQL은 `INTERSECT`를 직접 지원하지 않음).  
     대신 `INNER JOIN`을 활용:
     ```sql
     SELECT s.product_id
     FROM Sales s
     INNER JOIN Returns r ON s.product_id = r.product_id;
     ```

5. **서브쿼리의 별명 필요 여부**  
   - 별명이 필요하지 않은 경우: `EXISTS`와 같이 서브쿼리 결과를 참조하지 않을 때.
   - 별명이 필요한 경우: 서브쿼리가 임시 테이블처럼 작동하거나 특정 컬럼을 참조할 때.
     ```sql
     SELECT t.product_id, t.total_sales
     FROM (
         SELECT product_id, SUM(quantity) AS total_sales
         FROM Sales
         GROUP BY product_id
     ) AS t
     WHERE t.total_sales > 100;
     ```

MySQL에서 `IF`문과 `CASE`문은 **조건부 로직**을 처리할 때 `SELECT`에서 아주 유용하다! 


## 🟩 **1. `IF` 문 사용법 (단순 조건 분기)**

`IF()`는 **세 가지 인자**를 받아서, 조건에 따라 **두 가지 값 중 하나**를 반환한다.

```sql
SELECT IF(조건, 참일 때 반환값, 거짓일 때 반환값);
```

🔸 **예시**: `salary`가 **3000 이상**이면 `"High"` 아니면 `"Low"`라고 표시.

```sql
SELECT 
    employee_name,
    salary,
    IF(salary >= 3000, 'High', 'Low') AS salary_level
FROM employees;
```

🔸 **결과**:

| employee_name | salary | salary_level |
|---------------|--------|--------------|
| Alice         | 3500   | High         |
| Bob           | 2800   | Low          |

---

## 🟩 **2. `CASE` 문 사용법 (복잡한 다중 조건 처리)**

`CASE`는 **여러 개의 조건**을 처리할 수 있고, **`ELSE`**를 사용해 기본값도 지정할 수 있다!

```sql
SELECT 
    CASE 
        WHEN 조건1 THEN 반환값1
        WHEN 조건2 THEN 반환값2
        ELSE 기본값
    END AS 별칭
FROM 테이블명;
```

🔸 **예시**: `score`에 따라 **등급** 부여하기.

```sql
SELECT 
    student_name,
    score,
    CASE
        WHEN score >= 90 THEN 'A'
        WHEN score >= 80 THEN 'B'
        WHEN score >= 70 THEN 'C'
        ELSE 'F'
    END AS grade
FROM students;
```

🔸 **결과**:

| student_name | score | grade |
|--------------|-------|-------|
| John         | 95    | A     |
| Emma         | 82    | B     |
| Mike         | 68    | F     |

---

## 🟩 **3. `CASE` 문으로 여러 컬럼 조건 처리**

`CASE`는 **다른 컬럼**의 값에 따라 동적으로 처리할 수도 있다!

🔸 **예시**: **부서별 상여금** 계산.

```sql
SELECT 
    employee_name,
    department,
    salary,
    CASE department
        WHEN 'Sales' THEN salary * 0.10
        WHEN 'Engineering' THEN salary * 0.15
        WHEN 'HR' THEN salary * 0.05
        ELSE 0
    END AS bonus
FROM employees;
```

🔸 **결과**:

| employee_name | department  | salary | bonus |
|---------------|-------------|--------|-------|
| Alice         | Sales       | 4000   | 400   |
| Bob           | Engineering | 5000   | 750   |
| Charlie       | HR          | 3000   | 150   |

---

## 🟩 **4. `IF` vs `CASE` 언제 사용해야 할까?**

| **기능**                | **IF()**                         | **CASE**                     |
|--------------------------|-----------------------------------|------------------------------|
| **조건 개수**            | **1개 (단순)**                   | **여러 개 (복잡)**           |
| **리턴 값의 다양성**     | **2가지 (참/거짓)**              | **다양한 값**                |
| **코드 가독성**          | **간결함**                       | **명확함 (더 읽기 쉬움)**    |
| **유연성**               | **제한적**                       | **유연하고 다기능적**        |

✅ **단순 조건**은 → `IF`  
✅ **다중 조건, 범위 처리**는 → `CASE`

---

## 🟩 **실전 예시 (두 가지 조합해서 사용하기)**

🔸 **예시**: 직원별 성과에 따라 등급을 부여하고, 고성과자에게 추가 상여금을 계산하기!

```sql
SELECT 
    employee_name,
    salary,
    performance_score,
    IF(performance_score >= 90, 'Top Performer', 'Regular') AS performance_level,
    CASE
        WHEN performance_score >= 90 THEN salary * 0.20
        WHEN performance_score >= 80 THEN salary * 0.10
        ELSE salary * 0.05
    END AS bonus
FROM employees;
```

🔸 **결과**:

| employee_name | salary | performance_score | performance_level | bonus |
|---------------|--------|-------------------|-------------------|-------|
| Alice         | 5000   | 92                | Top Performer     | 1000  |
| Bob           | 4000   | 85                | Regular           | 400   |
| Charlie       | 3500   | 75                | Regular           | 175   |

---

## 🟩 **정리**

- `IF()` → **단순 이분법 조건** (참/거짓).  
- `CASE` → **복잡한 다중 조건, 컬럼별 다른 처리가 필요할 때**.

실제 쿼리 성능상 큰 차이는 없지만, **가독성**과 **유지보수**를 고려하면 복잡한 로직은 `CASE`가 더 깔끔하다!


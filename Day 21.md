# **Common Table Expressions (CTEs)**

CTEs (`WITH` clauses) create **temporary named result sets** that exist **only during query execution**.
They make complex SQL more **readable, modular, and maintainable**.

## **Basic Syntax**

```sql
WITH cte_name AS (
    SELECT columns
    FROM table
    WHERE condition
)
SELECT *
FROM cte_name;
```

## **Multiple CTEs**

```sql
WITH
    cte1 AS (
        SELECT ...
    ),
    cte2 AS (
        SELECT ...
    )
SELECT *
FROM cte1
JOIN cte2 ON ...;
```

---

# Tips

## **1. Break Down Complex Queries Step-by-Step**

Instead of nesting subqueries:

```sql
-- Step-by-step structure using CTEs
WITH
    step1 AS (SELECT ...),
    step2 AS (SELECT ... FROM step1),
    step3 AS (SELECT ... FROM step2)
SELECT *
FROM step3;
```

## **2. CTEs vs Subqueries**

### **CTEs**

* More readable
* Can be referenced multiple times
* Great for modular logic

```sql
WITH avg_age AS (
    SELECT AVG(age) AS avg_age
    FROM patients
)
SELECT *
FROM patients, avg_age
WHERE age > avg_age.avg_age;
```

### **Subquery**

More concise for simple cases:

```sql
SELECT *
FROM patients
WHERE age > (SELECT AVG(age) FROM patients);
```

##  **3. CTEs Are Evaluated Once**

Useful when the same logic is reused multiple times:

```sql
WITH service_avg AS (
    SELECT service, AVG(satisfaction) AS avg_sat
    FROM patients
    GROUP BY service
)
SELECT *
FROM patients p
JOIN service_avg sa ON p.service = sa.service
WHERE p.satisfaction > sa.avg_sat;   -- referencing CTE again
```

## **4. Use Descriptive CTE Names**

❌ `WITH x AS, y AS, z AS`
✔️ `WITH patient_stats AS, staff_summary AS, weekly_trends AS`

Good naming = easier debugging and maintenance.


## **5. Easy to Debug**

Test each CTE alone:

```sql
-- Test first CTE
WITH cte1 AS (SELECT ...)
SELECT * FROM cte1;

-- Then add second layer
WITH
    cte1 AS (...),
    cte2 AS (...)
SELECT * FROM cte2;
```

## **6. Not Materialized by Default**

Some databases **recompute CTEs when referenced multiple times**.
If a CTE is expensive and reused often → consider **temporary tables**.

---

# ** Summary Table**

| Concept                    | Description                                                                          | Example                                          |
| -------------------------- | ------------------------------------------------------------------------------------ | ------------------------------------------------ |
| **Definition**             | Temporary named result set available only during query execution. Helps readability. | `WITH c AS (SELECT ...) SELECT * FROM c;`        |
| **Basic Syntax**           | Single CTE definition followed by a main query.                                      | `WITH cte AS (...) SELECT ... FROM cte;`         |
| **Multiple CTEs**          | You can stack multiple CTEs separated by commas.                                     | `WITH c1 AS (...), c2 AS (...) SELECT ...`       |
| **Reusability**            | CTEs can be referenced **multiple times** in the main query.                         | Use CTE in multiple JOINs or WHERE clauses.      |
| **Readability**            | Break large SQL into logical steps (step1 → step2 → step3).                          | `step2 AS (SELECT ... FROM step1)`               |
| **Debugging**              | Each CTE can be tested independently.                                                | Run `SELECT * FROM cte_name;`                    |
| **CTEs vs Subqueries**     | CTEs = readable & reusable. Subqueries = shorter but nested.                         | `WITH avg AS (...)` vs `(SELECT AVG(...))`       |
| **Performance**            | Not materialized by default; may be re-evaluated.                                    | Use temp tables for heavy repeated calculations. |
| **Naming Best Practices**  | Use meaningful names to describe logic.                                              | `patient_stats`, `weekly_totals`                 |
| **Complex Query Handling** | Useful for multi-step transformations and analytic queries.                          | Building running totals, ranking, aggregates     |

---

## Challenge
Create a comprehensive hospital performance dashboard using CTEs. Calculate: 1) Service-level metrics (total admissions, refusals, avg satisfaction), 2) Staff metrics per service (total staff, avg weeks present), 3) Patient demographics per service (avg age, count). Then combine all three CTEs to create a final report showing service name, all calculated metrics, and an overall performance score (weighted average of admission rate and satisfaction). Order by performance score descending.
```sql


```








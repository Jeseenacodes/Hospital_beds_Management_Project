# Subqueries in SELECT & FROM Clauses

### Key Concepts

* **Subqueries in `SELECT`** add *smart calculated fields*
* **Subqueries in `FROM`** act like *mini temporary tables*
* **Always alias derived tables**
* **Correlated subqueries** run once per row → may impact performance
* **CTEs** simplify complex nested logic
* **Aggregates in subqueries** help compare individual vs overall performance

## Tips 

### Always alias derived tables

```
FROM (SELECT ... ) AS t
```

### Subquery in SELECT must return a single value

```sql
SELECT 
    name,
    (SELECT COUNT(*) FROM orders WHERE customer_id = c.id) AS order_count
FROM customers c;
```
> Retruns 1 value per row, uses aggregates and can be slow


### Use derived tables to organize complex logic

```sql
FROM (
    SELECT service, AVG(satisfaction) AS avg_sat
    FROM services
    GROUP BY service
) AS s
```
> Used like mini temporary table with an alias

### CTEs are cleaner than deeply nested subqueries

```sql
WITH service_avg AS (
    SELECT service, AVG(satisfaction) AS avg_sat
    FROM services
    GROUP BY service
)
SELECT * FROM service_avg;
```

### Correlated subqueries in SELECT run once per row

Be careful — they’re powerful but slower on large datasets.

```sql
SELECT 
    p.patient_id,
    p.name,
    (SELECT AVG(satisfaction) 
     FROM visits v 
     WHERE v.patient_id = p.patient_id) AS personal_avg
FROM patients p;
```
---
### Where Subqueries Live

| Clause             | Purpose                    | Example Use               |
| ------------------ | -------------------------- | ------------------------- |
| **SELECT**         | Add a *calculated field*   | Count orders per customer |
| **FROM**           | Create a *temporary table* | Aggregated service stats  |
| **WHERE / HAVING** | Filter using another query | Find values above avg     |
| **EXISTS / IN**    | Check matching records     | Find related rows         |


---
### Performance  

| Technique                 | Speed | When to Use                        |
| ------------------------- | ----- | ---------------------------------- |
| **IN**                    | ⭐⭐    | Small lists, simple filtering      |
| **EXISTS**                | ⭐⭐⭐   | Large datasets, correlated matches |
| **Correlated Subqueries** | ⭐     | Only when absolutely needed        |
| **Derived Tables**        | ⭐⭐⭐   | Mid-size datasets, transformations |
| **CTEs**                  | ⭐⭐⭐⭐  | Complex logic, readability         |

---
### Summary 
| Clause             | Purpose                    | Example Use               |
| ------------------ | -------------------------- | ------------------------- |
| **SELECT**         | Add a *calculated field*   | Count orders per customer |
| **FROM**           | Create a *temporary table* | Aggregated service stats  |
| **WHERE / HAVING** | Filter using another query | Find values above avg     |
| **EXISTS / IN**    | Check matching records     | Find related rows         |
---

## Challenge
1️⃣  Create a report showing each service with: service name, total patients admitted, 
the difference between their total admissions and the average admissions across all services, and a rank indicator
('Above Average', 'Average', 'Below Average'). Order by total patients admitted descending.

```sql

SELECT st.service, st.total_patients,
    (st.total_patients - oa.avg_patients) AS difference_from_avg,
    CASE
        WHEN st.total_patients > oa.avg_patients THEN 'Above Average'
        WHEN st.total_patients = oa.avg_patients THEN 'Average'
        ELSE 'Below Average'
    END AS rank_indicator
FROM 
    ( SELECT service, SUM(patients_admitted) AS total_patients
        FROM services_weekly
        GROUP BY service
    ) st
CROSS JOIN 
    (
        SELECT AVG(total_patients) AS avg_patients
        FROM 
            (
                SELECT service, SUM(patients_admitted) AS total_patients
                FROM services_weekly
                GROUP BY service
            ) x
    ) oa
ORDER BY st.total_patients DESC;

```

Output:

 <img width="402" height="96" alt="image" src="https://github.com/user-attachments/assets/8900ce22-de66-4bf8-84c9-918f818fbd3f" />


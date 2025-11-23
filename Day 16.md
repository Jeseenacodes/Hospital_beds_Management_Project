
# **SQL Subqueries**

Small queries inside a bigger query that help filter, compare, or validate data.

## **Types of Subqueries**

### **Single-Value Subqueries**

Return **one value** → use:

* `=`
* `<`
* `>`
* `<=`
* `>=`

> *Use for comparisons like “salary > average salary”*


### **Multi-Value Subqueries**

Return **multiple values** → use:

* `IN`
* `NOT IN`

> *Use when comparing lists (e.g., customers in certain regions).*

### **Existence Check Subqueries**

* `EXISTS`
* `NOT EXISTS`

> *Best for checking if related records exist — most efficient for large datasets.*

## **Correlated vs Uncorrelated**

### **Correlated Subquery**

Depends on the **outer query row**.

* Runs *row by row*
* More powerful; may be slower

### **Uncorrelated Subquery**

Independent of outer query.

* Runs *once*
* Faster and simpler

## **Performance Guidance**

### ✔ Use `IN`

→ For small result sets

### Use `EXISTS`

→ For large datasets
→ When checking if a record exists
→ Better in most real-world workloads

## ⚠ Beware: `NOT IN` + NULL

If the subquery returns even **one NULL**,
`NOT IN` ⇒ returns **no rows**.

 Always prefer:
`NOT EXISTS`

---

## **Examples**

### 🔹 Single-value

```sql
WHERE salary > (SELECT AVG(salary) FROM employees)
```

### 🔹 Multi-value

```sql
WHERE customer_id IN (SELECT id FROM vip_customers)
```

### 🔹 EXISTS

```sql
WHERE EXISTS (
    SELECT 1 FROM orders o 
    WHERE o.customer_id = c.customer_id
)
```
---
| **Subquery Type**         | **Returns**                | **Used With**             | **Best Use Case**                                                   | **Notes**                           |
| ------------------------- | -------------------------- | ------------------------- | ------------------------------------------------------------------- | ----------------------------------- |
| **Single-Value Subquery** | One value (scalar)         | `=`, `<`, `>`, `<=`, `>=` | Compare a column to an aggregated value (e.g., salary > avg salary) | Fast, simple                        |
| **Multi-Value Subquery**  | A list of values           | `IN`, `NOT IN`            | Filter based on a list of IDs, names, etc.                          | Avoid `NOT IN` if NULLs exist       |
| **Table Subquery**        | A full result table        | Used in `FROM`            | Treat subquery like a virtual table                                 | Often used for intermediate results |
| **EXISTS Subquery**       | Boolean (TRUE/FALSE)       | `EXISTS`, `NOT EXISTS`    | Check if related rows exist                                         | Best performance for large data     |
| **Correlated Subquery**   | Depends on outer row       | Same as above             | Row-by-row checks (e.g., latest order per customer)                 | More powerful but slower            |
| **Uncorrelated Subquery** | Independent of outer query | Same as above             | Subquery runs once → used for fixed comparisons                     | Faster than correlated              |
---
## Challenge
1️⃣  Find all patients who were admitted to services that had at least one week where patients were refused AND the average patient satisfaction for that service was below the overall hospital average satisfaction. Show patient_id, name, service, and their personal satisfaction score.
```sql
SELECT patient_id, name, service, satisfaction 
FROM patients
WHERE service IN 
( SELECT service FROM services_weekly WHERE patients_refused > 0 )
AND
service IN  (
SELECT service FROM services_weekly
GROUP BY service
HAVING AVG(patient_satisfaction) < (SELECT AVG(patient_satisfaction) FROM services_weekly) );
```

Output:

<img width="332" height="189" alt="Screenshot 2025-11-22 223701" src="https://github.com/user-attachments/assets/5240dcee-ab7a-4387-8fae-9ef065c0d050" />





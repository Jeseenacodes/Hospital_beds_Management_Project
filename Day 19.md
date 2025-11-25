# Window Functions — Clean Summary (Markdown)

Window functions perform calculations **across rows related to the current row**, *without collapsing results* like `GROUP BY`.

## **Basic Syntax**

```sql
window_function() OVER ( [PARTITION BY column] [ORDER BY column])
```

---

## **Ranking Functions**

### **ROW_NUMBER()**

Sequential numbering
`1, 2, 3, 4…` — always unique

### **RANK()**

Same values → same rank
Gaps appear after ties
`1, 2, 2, 4…`

### **DENSE_RANK()**

Same values → same rank
No gaps
`1, 2, 2, 3…`

---
# Tips

### Choosing the Right Ranking Function

| Function         | Use When                             |
| ---------------- | ------------------------------------ |
| **ROW_NUMBER()** | You need unique row numbering        |
| **RANK()**       | Ties should skip numbers (`1,2,2,4`) |
| **DENSE_RANK()** | Ties shouldn't skip (`1,2,2,3`)      |

### PARTITION BY is Optional

* Without `PARTITION BY` → entire dataset treated as one group
* With `PARTITION BY` → window restarts for each group

**Examples**

```sql
-- Rank across all patients
RANK() OVER (ORDER BY satisfaction DESC)

-- Rank within each service
RANK() OVER (PARTITION BY service ORDER BY satisfaction DESC)
```

### Filtering Window Results → Use a Subquery

 **Cannot** use window functions directly in `WHERE`.

❌ Invalid:

```sql
SELECT *, ROW_NUMBER() OVER (...) AS rn
FROM patients
WHERE rn <= 10;
```

✔ Correct:

```sql
SELECT *
FROM (
    SELECT *, ROW_NUMBER() OVER (ORDER BY age DESC) AS rn
    FROM patients
) t
WHERE rn <= 10;   -- Top 10 oldest patients
```

### ORDER BY Inside OVER ≠ Query ORDER BY

```sql
SELECT
    name,
    ROW_NUMBER() OVER (ORDER BY age DESC) AS rn   -- For numbering
FROM patients
ORDER BY name;    -- Final result sorting
```

### Window Functions Do NOT Reduce Rows

Unlike `GROUP BY`, each input row produces **one output row**.

---

### Examples

```sql
-- Number of patients within each service
SELECT
    patient_id,
    name,
    service,
    satisfaction,
    ROW_NUMBER() OVER (
        PARTITION BY service 
        ORDER BY satisfaction DESC
    ) AS row_num
FROM patients;

------------------------------------------------------------

-- Rank patients by satisfaction (with ties)
SELECT
    patient_id,
    name,
    satisfaction,
    RANK() OVER (
        ORDER BY satisfaction DESC
    ) AS rank,
    DENSE_RANK() OVER (
        ORDER BY satisfaction DESC
    ) AS dense_rank
FROM patients;

------------------------------------------------------------

-- Top 3 weeks by satisfaction per service
SELECT *
FROM (
    SELECT
        service,
        week,
        patient_satisfaction,
        RANK() OVER (
            PARTITION BY service 
            ORDER BY patient_satisfaction DESC
        ) AS sat_rank
    FROM services_weekly
) AS ranked_weeks
WHERE sat_rank <= 3;

------------------------------------------------------------

-- Rank services by total admissions
SELECT
    service,
    SUM(patients_admitted) AS total_admitted,
    RANK() OVER (
        ORDER BY SUM(patients_admitted) DESC
    ) AS admission_rank
FROM services_weekly
GROUP BY service;
```

---
## Challenge
1️⃣ For each service, rank the weeks by patient satisfaction score (highest first). Show service, week, patient_satisfaction, patients_admitted, and the rank. Include only the top 3 weeks per service.
```sql
SELECT * FROM
( SELECT week, service, patient_satisfaction, patients_admitted,
 DENSE_RANK() OVER ( PARTITION BY service ORDER BY patient_satisfaction DESC ) AS satisfaction_rank
 FROM services_weekly
) AS week_rank
WHERE satisfaction_rank <= 3; 
```

Output: 

<img width="459" height="379" alt="image" src="https://github.com/user-attachments/assets/0ece5358-0ae8-4050-8fe9-16bf991aa3ba" />


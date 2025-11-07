# Day 5

# SQL Aggregate Functions
Aggregate functions perform **calculations on multiple rows** and return a **single summarized value**.

---

## The Six Core Aggregate Functions

| Function                 | Description                                  |
| ------------------------ | -------------------------------------------- |
| `COUNT(*)`               | Counts **all rows**, including `NULL` values |
| `COUNT(column)`          | Counts **non-NULL** values in a column       |
| `COUNT(DISTINCT column)` | Counts **unique, non-NULL** values           |
| `SUM(column)`            | Adds up numeric values                       |
| `AVG(column)`            | Calculates the average of numeric values     |
| `MIN(column)`            | Finds the minimum value                      |
| `MAX(column)`            | Finds the maximum value                      |

---

## Examples

```sql
-- 1️⃣ Single Aggregate
SELECT COUNT(*) AS total_patients
FROM patients;
```

-- 2️⃣ Multiple Aggregates
```sql
SELECT
    COUNT(*) AS total,
    AVG(age) AS avg_age,
    MIN(age) AS youngest,
    MAX(age) AS oldest,
    SUM(satisfaction) AS total_satisfaction
FROM patients;
```

-- 3️⃣ Aggregate with WHERE Clause
```sql
SELECT AVG(satisfaction)
FROM patients
WHERE service = 'Cardiology';
```

---

## Tips

 **COUNT(*) vs COUNT(column)**

* `COUNT(*)` → counts **all rows** (including NULLs)
* `COUNT(column)` → counts **only non-NULL** values

 **Aggregates ignore NULL** *(except COUNT(*))*
Most aggregate functions (`SUM`, `AVG`, `MIN`, `MAX`) automatically skip `NULL` values.

 **Use DISTINCT with COUNT** to count unique values:

```sql
SELECT COUNT(DISTINCT service) AS unique_services
FROM patients;
```

 **Alias your aggregates** for readable column names:

```sql
SELECT AVG(age) AS average_age
FROM patients;
```

 **Round averages for cleaner output:**

```sql
SELECT ROUND(AVG(age), 2) AS avg_age
FROM patients;
```

---

## Practice Section

1️⃣ 
```sql

```

2️⃣ 
```sql

```

3️⃣ 
```sql

```

---
## Challenge
1️⃣ Calculate the total number of patients admitted, total patients refused, and the average patient satisfaction across all services and weeks. Round the average satisfaction to 2 decimal places.
```sql
SELECT  SUM(patients_admitted) AS Total_Patients_Admitted, 
SUM(patients_refused) AS Total_Patients_Refused, 
ROUND(AVG(patient_satisfaction),2) AS Patients_Satisfaction
FROM services_weekly; 
```

Output:

![Alt text](https://github.com/Jeseenacodes/Hospital_beds_Management_Project/blob/main/Results/Day%205.png)

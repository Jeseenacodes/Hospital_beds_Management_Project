
# **Aggregate Window Functions**

They calculate **running totals**, **moving averages**, and **cumulative statistics** **without reducing rows** (unlike GROUP BY).

## **Common Window Aggregates**

```sql
SUM(column)   OVER (...)   -- Running total
AVG(column)   OVER (...)   -- Moving average
MIN(column)   OVER (...)   -- Running minimum
MAX(column)   OVER (...)   -- Running maximum
COUNT(*)      OVER (...)   -- Running count
```

## **Window Frame Clauses**

```sql
ROWS BETWEEN start AND end
```

**start/end options:**

* `UNBOUNDED PRECEDING` → from the first row
* `N PRECEDING` → N rows before the current row
* `CURRENT ROW` → the current row
* `N FOLLOWING` → N rows after the current row
* `UNBOUNDED FOLLOWING` → to the last row

## **Tips**

### Default Frame (when ORDER BY is used)

```sql
SUM(col) OVER (ORDER BY date)
```

This actually means:

```sql
SUM(col) OVER (
    ORDER BY date
    ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
)
```

### Without ORDER BY → Uses all rows in the partition

```sql
-- Running total per service (with ORDER BY)
SUM(col) OVER (PARTITION BY service ORDER BY week)

-- Total per service (no ORDER BY)
SUM(col) OVER (PARTITION BY service)
```

## **Moving Averages**

### **3-period moving average (current + 2 previous)**

```sql
AVG(col) OVER (
    ORDER BY date
    ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
)
```

### **Centered 5-period moving average (2 before & 2 after)**

```sql
AVG(col) OVER (
    ORDER BY date
    ROWS BETWEEN 2 PRECEDING AND 2 FOLLOWING
)
```

## **Differences vs Aggregates**

```sql
-- Deviation from group average
col - AVG(col) OVER (PARTITION BY group)

-- Percent of total in a group
100.0 * col / SUM(col) OVER (PARTITION BY group)
```

## **Important Note**

❗ **Window functions can reference other window functions**, but **cannot nest** them:

```sql
# ❌ Not allowed:
SUM( AVG(col) OVER (...) ) OVER (...)
```
---
## Examples

### Running Total of Patients Admitted per Service

```sql
SELECT
    service, week, patients_admitted,
    SUM(patients_admitted) OVER ( PARTITION BY service ORDER BY week ) AS cumulative_admissions
FROM services_weekly
ORDER BY service, week;
```

### 3-Week Moving Average of Satisfaction

```sql
SELECT
     service, week, patient_satisfaction,
    ROUND(  AVG(patient_satisfaction) OVER (  PARTITION BY service   ORDER BY week  ROWS BETWEEN 2 PRECEDING AND CURRENT ROW  ), 2 ) AS moving_avg_3week
FROM services_weekly
ORDER BY service, week;
```

### Compare to Service Average

```sql
SELECT
    service, week, patients_admitted,
    AVG(patients_admitted) OVER (PARTITION BY service) AS service_avg,
patients_admitted - AVG(patients_admitted) OVER (PARTITION BY service) AS diff_from_avg
FROM services_weekly;
```

### Running Min/Max Satisfaction

```sql
SELECT
    service, week, patient_satisfaction,
    MIN(patient_satisfaction) OVER ( PARTITION BY service ORDER BY week ) AS min_so_far,
    MAX(patient_satisfaction) OVER ( PARTITION BY service ORDER BY week ) AS max_so_far
FROM services_weekly;
```

---

# **Aggregate Window Functions — Summary Table**

| Concept / Feature                         | Description                                                              | Example                                                                  |
| ----------------------------------------- | ------------------------------------------------------------------------ | ------------------------------------------------------------------------ |
| **Window Aggregate**                      | Performs calculations across related rows **without reducing row count** | `SUM(col) OVER (...)`                                                    |
| **Running Total**                         | Cumulative sum up to current row (requires ORDER BY)                     | `SUM(col) OVER (ORDER BY date)`                                          |
| **Moving Average**                        | Average across a rolling window defined by ROWS BETWEEN                  | `AVG(col) OVER (ORDER BY date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW)` |
| **Running Count**                         | Counts rows up to current position                                       | `COUNT(*) OVER (ORDER BY date)`                                          |
| **Frame Clause**                          | Defines which rows the window should consider                            | `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`                       |
| **UNBOUNDED PRECEDING**                   | Start of partition                                                       | Running totals                                                           |
| **CURRENT ROW**                           | Only the row you're on                                                   | Moving averages                                                          |
| **N PRECEDING / FOLLOWING**               | N rows before/after current row                                          | Centered moving averages                                                 |
| **Partition Without ORDER BY**            | Calculates over entire partition                                         | `SUM(col) OVER (PARTITION BY service)`                                   |
| **Deviation From Average**                | Compare a row to group average                                           | `col - AVG(col) OVER (PARTITION BY group)`                               |
| **Percent of Total**                      | Row value as % of partition total                                        | `col / SUM(col) OVER (PARTITION BY group)`                               |
| **Cannot Nest Window Functions**          | Window functions can’t operate on another window function                | ❌ `SUM(AVG(...))`                                                        |
| **Executes After WHERE, Before ORDER BY** | Window functions run after filtering but before final ordering           | Used with subqueries for filtering                                       |

---
## Challenge

1️⃣ Create a trend analysis showing for each service and week: 
week number, patients_admitted, running total of patients admitted (cumulative), 
3-week moving average of patient satisfaction (current week and 2 prior weeks), 
and the difference between current week admissions and the service average. 
Filter for weeks 10-20 only.
```sql
SELECT 
service AS Service, 
week AS Week, 
patients_admitted AS Patients_Admitted,
SUM(patients_admitted) OVER(PARTITION BY service ORDER BY week) AS Running_Total_Patients_Admitted,
ROUND(AVG(patient_satisfaction) OVER(PARTITION BY service ORDER BY week ROWS BETWEEN 2 PRECEDING AND CURRENT ROW),2) AS Moving_Avg_Patient_Satisfaction,
ROUND(patients_admitted - AVG(patients_admitted) OVER(PARTITION BY service),2) AS Difference_of_service_avg
FROM 
( SELECT * FROM services_weekly WHERE week between 10 AND 20 ) AS filter_week
ORDER BY Service, Week ;
```

<img width="788" height="352" alt="image" src="https://github.com/user-attachments/assets/ae8d30d7-c498-4684-8548-b22aa713a3c8" />





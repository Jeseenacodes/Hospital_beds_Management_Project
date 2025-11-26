
# **Aggregate Window Functions**

Aggregate window functions calculate **running totals**, **moving averages**, and **cumulative statistics** **without reducing rows** (unlike GROUP BY).

## **Common Window Aggregates**

```sql
SUM(column)   OVER (...)   -- Running total
AVG(column)   OVER (...)   -- Moving average
COUNT(*)      OVER (...)   -- Running count
MIN(column)   OVER (...)   -- Running minimum
MAX(column)   OVER (...)   -- Running maximum
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

❌ Not allowed:

```sql
SUM( AVG(col) OVER (...) ) OVER (...)
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

```






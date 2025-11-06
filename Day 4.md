
# Day 4 

###  LIMIT and OFFSET — Pagination & Row Control

## Overview
- **`LIMIT`** restricts the number of rows returned.  
- **`OFFSET`** skips a specified number of rows before returning results.  

---

## Basic Syntax

```sql
-- Return a specific number of rows
SELECT columns 
FROM table_name
LIMIT number_of_rows;

-- Skip a number of rows, then return results
SELECT columns
FROM table_name
LIMIT number_of_rows OFFSET skip_rows;
````

---

## Pagination Example

```sql
-- Page 1: First 10 records 
SELECT * FROM patients 
LIMIT 10 OFFSET 0;

-- Page 2: Next 10 records 
SELECT * FROM patients 
LIMIT 10 OFFSET 10;

-- Page 3: Next 10 records 
SELECT * FROM patients 
LIMIT 10 OFFSET 20;
```

### Pagination Formula

```
OFFSET = (page_number - 1) × page_size
```

## Combine with ORDER BY (for predictable results)

```sql
-- Unpredictable: LIMIT without ORDER BY
SELECT * FROM patients 
LIMIT 10;

-- Predictable and consistent: Always use ORDER BY
SELECT * FROM patients
ORDER BY patient_id
LIMIT 10 OFFSET 20;
```
---

## Tips

*  Test queries with **`LIMIT`** before running on the full dataset.
*  **Database-specific syntax:**

  * **MySQL / PostgreSQL / SQLite:** `LIMIT` and `OFFSET`
  * **SQL Server:** `TOP` or `OFFSET...FETCH`
  * **Oracle:** `ROWNUM` or `FETCH FIRST`
*  **Query execution order:**

  ```
  FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT
  ```

---

 **Pro Tip:**
Use `LIMIT` + `OFFSET` to explore large datasets efficiently — perfect for pagination, sampling, or testing query results.

---

## Practice Section

1️⃣ Display the first 5 patients from the patients table.
```sql
SELECT * FROM patients
LIMIT 5; 
```

2️⃣ Show patients 11-20 using OFFSET.
```sql

SELECT * FROM patients
LIMIT 10 OFFSET 20; 
```

3️⃣ Get the 10 most recent patient admissions based on arrival_date.
```sql
SELECT * FROM patients 
ORDER BY arrival_date DESC
LIMIT 10;
```

---
## Challenge
1️⃣ Find the 3rd to 7th highest patient satisfaction scores from the patients table, showing patient_id, name, service, and satisfaction. Display only these 5 records.
```sql
SELECT patient_id, name, service, satisfaction FROM patients
LIMIT 5 OFFSET 2; 
```

Output:

![Alt text](https://github.com/Jeseenacodes/Hospital_beds_Management_Project/blob/main/Results/Day%204.png)

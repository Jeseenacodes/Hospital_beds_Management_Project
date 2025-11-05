
# Day 3 — SQL `ORDER BY` Clause

The **`ORDER BY`** clause sorts query results based on one or more columns.

## Basic Syntax

```sql
SELECT column1, column2
FROM table_name
ORDER BY column1 [ASC | DESC];
````

## Key Points

| Option           | Meaning                                                        | Example                       |
| ---------------- | -------------------------------------------------------------- | ----------------------------- |
| `ASC`            | Ascending (default) — A→Z, 0→9, oldest → newest                | `ORDER BY age ASC`            |
| `DESC`           | Descending — Z→A, 9→0, newest → oldest                         | `ORDER BY age DESC`           |
| Multiple Columns | Sort by more than one column                                   | `ORDER BY age DESC, name ASC` |
| `NULL` Values    | Appear **first** in `ASC`, **last** in `DESC` (varies by DBMS) | Depends on DBMS               |

---

## Examples

### 🔹 Single Column Sort

```sql
SELECT * 
FROM patients 
ORDER BY age DESC;
```

### 🔹 Multiple Column Sort

```sql
SELECT * 
FROM patients 
ORDER BY age DESC, name ASC;
```

> Sorts primarily by `age` (highest to lowest), and then by `name` (A–Z) **within the same age group**.

### 🔹 Sort by Column Number (not recommended)

```sql
SELECT name, age 
FROM patients 
ORDER BY 2 DESC;  -- sorts by 2nd selected column (age)
```

> Avoid using column numbers in production — if you reorder SELECT columns, your sort will break.

---

## Tips 

### 🔸 Multi-column Sorting Order Matters

```sql
ORDER BY service, age;   -- First by service, then age within each service
ORDER BY age, service;   -- First by age, then service within same ages
```

### 🔸 You Can Order by Columns Not in SELECT

```sql
SELECT name, age 
FROM patients 
ORDER BY satisfaction DESC;
```

### 🔸 Use `DESC` for “Top N” Queries

```sql
SELECT * 
FROM patients
ORDER BY satisfaction DESC
LIMIT 10;  -- Top 10 most satisfied patients
```

## Advanced Sorting

### 🟣 𝐍𝐔𝐋𝐋𝐬 Handling

By default, **`NULL` values** appear:

* **First** in `DESC` order
* **Last** in `ASC` order
  (*Behavior may vary by database system*)

You can explicitly control this in **PostgreSQL** or **Oracle**:

```sql
ORDER BY salary DESC NULLS LAST;
```

> Ensures that rows with missing (`NULL`) salaries appear at the bottom.


### 🟢 Using Expressions in ORDER BY

You can sort results based on **calculated values or expressions**, not just existing columns:

```sql
-- Sort by the length of the name
ORDER BY LENGTH(name);

-- Sort by a computed total
ORDER BY price * quantity DESC;
```

> This is useful for **custom rankings** or **derived metrics** in queries.

### 🔸 Clause Order Reminder

`ORDER BY` is usually the **last clause** in a query, **except when used with** `LIMIT`.

### 🔸 Performance Note

Sorting can be **computationally expensive**. Hence create **indexes** on frequently sorted columns for faster performance.

---

## 🧾 Summary

| Keyword              | Purpose                     | Example                           |
| -------------------- | --------------------------- | --------------------------------- |
| `ORDER BY`           | Sorts rows in query results | `ORDER BY age ASC`                |
| `ASC`                | Ascending order (default)   | `ORDER BY name ASC`               |
| `DESC`               | Descending order            | `ORDER BY salary DESC`            |
| `LIMIT`              | Restrict output count       | `LIMIT 10`                        |
| `NULLS FIRST / LAST` | Control `NULL` sort order   | `ORDER BY salary DESC NULLS LAST` |
| Expressions          | Sort by calculated fields   | `ORDER BY price * quantity`       |

---

## Practice Section

1️⃣ List all patients sorted by age in descending order.
```sql
SELECT * FROM patients
ORDER BY age DESC
```

2️⃣ Show all services_weekly data sorted by week number ascending and patients_request descending.
```sql
SELECT * FROM services_weekly
ORDER BY week, patients_request DESC; 
```

3️⃣ Display staff members sorted alphabetically by their names.
```sql
SELECT * FROM staff
ORDER BY staff_name; 
```

---
## Challenge
1️⃣ Retrieve the top 5 weeks with the highest patient refusals across all services, showing week, service, patients_refused, and patients_request. Sort by patients_refused in descending order.
```sql
SELECT week, service, patients_refused, patients_request FROM services_weekly
ORDER BY patients_request DESC
LIMIT 5; 
```

Output:

![Alt text](https://github.com/Jeseenacodes/Hospital_beds_Management_Project/blob/main/Results/Day%203.png)

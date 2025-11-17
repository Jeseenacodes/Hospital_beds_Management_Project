
# LEFT JOIN & RIGHT JOIN — Explained Clearly

* **LEFT JOIN** → Returns **all rows** from the **left table**, plus matching rows from the right table.
  If no match → right-side columns become **NULL**.
* **RIGHT JOIN** → Opposite of LEFT JOIN (returns all rows from right table).

## **Basic Syntax**

### **LEFT JOIN (most common)**

```sql
SELECT columns
FROM table1
LEFT JOIN table2
    ON table1.column = table2.column;
```

### **RIGHT JOIN**

```sql
SELECT columns
FROM table1
RIGHT JOIN table2
    ON table1.column = table2.column;
```

---

## **Key Differences**

| Join Type      | What It Returns                                                      |
| -------------- | -------------------------------------------------------------------- |
| **INNER JOIN** | Only matching rows from both tables                                  |
| **LEFT JOIN**  | All left-table rows + matching right-table rows (NULL when no match) |
| **RIGHT JOIN** | All right-table rows + matching left-table rows (NULL when no match) |

---

## **Examples**

### **1. All staff + their schedule (NULL if no schedule)**

```sql
SELECT
    s.staff_id,
    s.staff_name,
    s.role,
    s.service,
    COUNT(ss.week) AS weeks_scheduled,
    SUM(COALESCE(ss.present, 0)) AS weeks_present
FROM staff s
LEFT JOIN staff_schedule ss
    ON s.staff_id = ss.staff_id
GROUP BY s.staff_id, s.staff_name, s.role, s.service;
```

---

### **2. Find staff with *no schedule records***

```sql
SELECT s.*
FROM staff s
LEFT JOIN staff_schedule ss
    ON s.staff_id = ss.staff_id
WHERE ss.staff_id IS NULL;
```

---

### **3. All services & their patient counts (even if zero)**

```sql
SELECT
    sw.service,
    sw.week,
    COUNT(p.patient_id) AS patient_count
FROM services_weekly sw
LEFT JOIN patients p
    ON sw.service = p.service
GROUP BY sw.service, sw.week;
```

---

# Tips

### **LEFT JOIN is more common**

RIGHT JOIN can always be rewritten as LEFT JOIN:

```sql
-- RIGHT JOIN version
FROM table1
RIGHT JOIN table2 ON table1.id = table2.id;

-- Equivalent LEFT JOIN
FROM table2
LEFT JOIN table1 ON table1.id = table2.id;
```

---

### **Use COALESCE to replace NULLs**

```sql
SELECT
    s.staff_name,
    COALESCE(SUM(ss.present), 0) AS weeks_present
FROM staff s
LEFT JOIN staff_schedule ss
    ON s.staff_id = ss.staff_id;
```

---

### **Find non-matching rows with IS NULL**

```sql
SELECT s.*
FROM staff s
LEFT JOIN staff_schedule ss
    ON s.staff_id = ss.staff_id
WHERE ss.staff_id IS NULL;
```

---

### **ON vs WHERE in LEFT JOIN**

Important difference:

```sql
-- Condition inside ON → keeps all left rows
LEFT JOIN table2
    ON table1.id = table2.id
    AND table2.type = 'A';

-- Condition inside WHERE → removes NULL rows (acts like INNER JOIN)
LEFT JOIN table2
    ON table1.id = table2.id
WHERE table2.type = 'A';
```

---

### **LEFT JOIN preserves all rows from the left side**

(Unless WHERE filters them out later.)

---


## Practice Section

1️⃣ Join patients and staff based on their common service field (show patient and staff who work in same service).
```sql

```

2️⃣ Join services_weekly with staff to show weekly service data with staff information.
```sql

```

3️⃣ Create a report showing patient information along with staff assigned to their service.
```sql

```

---
## Challenge
1️⃣ Create a staff utilisation report showing all staff members (staff_id, staff_name, role, service) 
and the count of weeks they were present (from staff_schedule).
Include staff members even if they have no schedule records. Order by weeks present descending.

```sql

```




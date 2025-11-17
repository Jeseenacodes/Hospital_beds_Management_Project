
# INNER JOIN 

**INNER JOIN** combines rows from two tables based on a matching column.
It returns **only the rows that have matching values in BOTH tables**.

## **Basic Syntax**

```sql
SELECT columns
FROM table1
INNER JOIN table2
    ON table1.column = table2.column;
```

## **How INNER JOIN Works (Step-by-Step)**

1. Takes each row from **table1**
2. Looks for matching rows in **table2** using the ON condition
3. Returns only the rows **where matches exist**
4. Rows without matches in either table are **excluded**

## **Examples**

### **1. Join patients with staff (matching service)**

```sql
SELECT
    p.patient_id,
    p.name AS patient_name,
    p.service,
    s.staff_name,
    s.role
FROM patients p
INNER JOIN staff s ON p.service = s.service
ORDER BY p.service, p.name;
```

### **2. Count staff members per patient service**

```sql
SELECT
    p.patient_id,
    p.name,
    p.service,
    COUNT(s.staff_id) AS staff_count
FROM patients p
INNER JOIN staff s ON p.service = s.service
GROUP BY p.patient_id, p.name, p.service;
```

---

### **3. Multiple join conditions**

```sql
SELECT *
FROM services_weekly sw
INNER JOIN staff_schedule ss
    ON sw.service = ss.service AND sw.week = ss.week;
```

---

#  Tips 

### **Use Table Aliases**

Cleaner and easier to read:

```sql
FROM patients p
JOIN staff s ON ...
```

### **Qualify Columns to Avoid Ambiguity**

Bad:

```sql
SELECT service
```

Good:

```sql
SELECT p.service
```

### **INNER is Optional**

These are the same:

```sql
JOIN staff ON ...
INNER JOIN staff ON ...
```

### **Chain Multiple Joins**

```sql
FROM table1 t1
JOIN table2 t2 ON t1.id = t2.id
JOIN table3 t3 ON t2.id = t3.id;
```

### **Use WHERE for Extra Filters**

```sql
FROM patients p
JOIN staff s ON p.service = s.service
WHERE p.age > 65;
```

---

| Join Column Type          | Allowed?    | Recommended?           |
| ------------------------- | ----------- | ---------------------- |
| Primary Key → Foreign Key | ✅ Yes       | ⭐ Best Practice        |
| Unique column             | ✅ Yes       | Good                   |
| Codes (date, category)    | ✅ Yes       | Good                   |
| Non-unique columns        | ⚠️ Possible | Avoid unless necessary |
| Free text                 | ⚠️ Possible | Not recommended        |

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
1️⃣ Create a comprehensive report showing patient_id, patient name, age, service, 
and the total number of staff members available in their service. 
Only include patients from services that have more than 5 staff members. 
Order by number of staff descending, then by patient name.

```sql
SELECT 
p.patient_id AS Patient_id, 
p.name AS Name, 
p.age AS Age, 
p.service AS Service,
COUNT(s.staff_id) AS Total_Staff
FROM patients AS p
INNER JOIN staff AS s ON p.service = s.service 
GROUP BY p.patient_id, p.name, p.age, p.service
HAVING COUNT(s.staff_id) > 5
ORDER BY Total_Staff DESC, p.name ASC;

-- For large datasets
SELECT p.patient_id, p.name, p.age, p.service, s.Total_Staff
FROM patients AS p
JOIN (
    SELECT service, COUNT(*) AS total_staff
    FROM staff
    GROUP BY service
    HAVING COUNT(*) > 5
) s
    ON p.service = s.service
ORDER BY s.total_staff DESC, p.name;
```

Output:

![Alt text](https://github.com/Jeseenacodes/Hospital_beds_Management_Project/blob/main/Results/Day%2013.png)

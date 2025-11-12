## **SQL HAVING Clause**

**Definition:**
`HAVING` filters **groups created by `GROUP BY`**, similar to how `WHERE` filters rows.

### **Basic Syntax**

```sql
SELECT column1, aggregate_function(column2)
FROM table_name
GROUP BY column1
HAVING aggregate_condition;
```

### **WHERE vs HAVING**

| **Feature**             | **WHERE**                  | **HAVING**                       |
| ----------------------- | -------------------------- | -------------------------------- |
| **Filters**             | Individual **rows**        | **Groups** created by `GROUP BY` |
| **When applied**        | **Before** grouping        | **After** grouping               |
| **Can use aggregates?** | ❌ No                       | ✅ Yes                            |
| **Common use**          | Filter data before summary | Filter summary results           |


### **Examples**

```sql
-- 1️⃣ Services with more than 100 patients
SELECT service, COUNT(*) AS patient_count
FROM patients
GROUP BY service
HAVING COUNT(*) > 100;
```

```sql
-- 2️⃣ Combining WHERE and HAVING
SELECT service, COUNT(*) AS elderly_count
FROM patients
WHERE age >= 65              -- Filter rows first
GROUP BY service
HAVING COUNT(*) > 20;        -- Filter groups after
```

```sql
-- 3️⃣ Multiple HAVING conditions
SELECT 
    service,
    AVG(satisfaction) AS avg_sat,
    COUNT(*) AS count
FROM patients
GROUP BY service
HAVING AVG(satisfaction) > 80 AND COUNT(*) > 50;
```

---

### Tips

✅ **Execution Order:**
`WHERE → GROUP BY → HAVING → ORDER BY`

✅ **Use WHERE for row filtering, HAVING for group filtering**

```sql
-- ❌ Inefficient
SELECT service, COUNT(*) 
FROM patients
GROUP BY service
HAVING age > 65;

-- ✅ Efficient
SELECT service, COUNT(*) 
FROM patients
WHERE age > 65          -- Filter before grouping (faster)
GROUP BY service;
```

✅ **HAVING requires GROUP BY**
You can’t use `HAVING` without grouping.

✅ **You can reference column aliases in HAVING** *(database-dependent)*

```sql
SELECT service, COUNT(*) AS count
FROM patients
GROUP BY service
HAVING count > 100;  -- Works in some SQL engines
```

✅ **Combine multiple conditions with AND / OR**

```sql
SELECT service, COUNT(*) AS patient_count
FROM patients
GROUP BY service
HAVING COUNT(*) > 100 OR AVG(satisfaction) > 90;
```
---

## Challenge
- Identify services that refused more than 100 patients in total and had an average patient satisfaction below 80. Show service name, total refused, and average satisfaction.

``` sql
SELECT service, 
SUM(patients_refused) AS Total_Patients_Refused, 
ROUND(AVG(patient_satisfaction),2) AS Avg_Patient_Satisfaction
FROM services_weekly 
GROUP BY service
HAVING Total_Patients_Refused > 100 AND Avg_Patient_Satisfaction < 80;
```

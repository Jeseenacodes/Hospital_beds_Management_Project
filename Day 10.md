
# Day 10

CASE statements add conditional logic to your SQL queries — similar to if-else statements in programming.
There are two syntaxes for CASE:

## Basic Syntax

```sql
# Simple Case
CASE column_name
    WHEN value1 THEN result1
    WHEN value2 THEN result2
    ELSE default_result
END
````

```sql
# Searched Case
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ELSE default_result
END
```

## Key Points

---

## Examples

### Categorize Patient Satisfaction

```sql
SELECT
    name,
    satisfaction,
    CASE
        WHEN satisfaction >= 90 THEN 'Excellent'
        WHEN satisfaction >= 75 THEN 'Good'
        WHEN satisfaction >= 60 THEN 'Fair'
        ELSE 'Needs Improvement'
    END AS satisfaction_category
FROM patients;
```
### Create Age groups
```sql
SELECT
    name,
    age,
    CASE
        WHEN age < 18 THEN 'Pediatric'
        WHEN age BETWEEN 18 AND 65 THEN 'Adult'
        ELSE 'Senior'
    END AS age_group
FROM patients;
```

### Conditional Aggregation
```sql
SELECT
    service,
    COUNT(*) AS total,
    SUM(CASE WHEN satisfaction >= 80 THEN 1 ELSE 0 END) AS high_satisfaction_count,
    SUM(CASE WHEN satisfaction < 60 THEN 1 ELSE 0 END) AS low_satisfaction_count
FROM patients
GROUP BY service;
```
---

## Tips 
- Always include ELSE → handles unexpected values (otherwise returns NULL)
-  CASE is an expression, not a statement → you can use it anywhere a column is allowed
-  Use CASE in ORDER BY for custom sorting:
```sql
ORDER BY CASE
    WHEN service = 'Emergency' THEN 1
    WHEN service = 'ICU' THEN 2
    ELSE 3
END;
```

- Conditional Aggregation Patterns:
```sql
-- Count matching rows
SUM(CASE WHEN condition THEN 1 ELSE 0 END)

-- Conditional average
AVG(CASE WHEN condition THEN value ELSE NULL END)
```

- Order matters → CASE evaluates top to bottom; first match wins
- You can nest CASE statements, but keep readability in mind


## Practice Section
 
1️⃣ Categorise patients as 'High', 'Medium', or 'Low' satisfaction based on their scores.
```sql
SELECT 
    name,
    satisfaction,
    CASE
        WHEN satisfaction >= 80 THEN 'High'
        WHEN satisfaction >= 60 THEN 'Medium'
        ELSE 'Low'
    END AS satisfaction_category
FROM patients;
```

2️⃣ Label staff roles as 'Medical' or 'Support' based on role type.
```sql
SELECT staff_name, role,
CASE 
	WHEN role IN( 'doctor', 'nurse') THEN 'Medical'
    ELSE 'Support'
    END AS Role_type
FROM staff;

-- Grouping by role (summaries)
SELECT 
    role, COUNT(*) AS staff_count,
    CASE 
        WHEN role IN ('doctor', 'nurse') THEN 'Medical'
        ELSE 'Support'
    END AS role_type
FROM staff
GROUP BY role;
```

3️⃣ Create age groups for patients (0-18, 19-40, 41-65, 65+).
```sql
SELECT 
    name, age,
    CASE
        WHEN age BETWEEN 0 AND 18 THEN '0-18'
        WHEN age BETWEEN 19 AND 40 THEN '19-40'
        WHEN age BETWEEN 41 AND 65 THEN '41-65'
        ELSE '65+'
    END AS age_group
FROM patients;
```

---
## Challenge
1️⃣ Create a service performance report showing service name, 
total patients admitted, and a 
performance category based on the following: 'Excellent' 
if avg satisfaction >= 85, 'Good' if >= 75, 'Fair' if >= 65, 
otherwise 'Needs Improvement'. Order by average satisfaction descending.

```sql
SELECT 
service AS Service, 
SUM(patients_admitted) AS Total_Patients_Admitted,
 AVG(patient_satisfaction) AS Average_Patient_Satisfaction_score,
CASE
	WHEN AVG(patient_satisfaction) >= 85 THEN 'Excellent'
	WHEN AVG(patient_satisfaction) >= 75 THEN 'Good'
	WHEN AVG(patient_satisfaction) >= 65 THEN 'Fair'
Else 'Needs Improvement'
END AS Performance_Category
FROM services_weekly
GROUP BY service 
ORDER BY AVG(patient_satisfaction) DESC ; 

```
Output:

![Alt text](https://github.com/Jeseenacodes/Hospital_beds_Management_Project/blob/main/Results/Day%2010.png)

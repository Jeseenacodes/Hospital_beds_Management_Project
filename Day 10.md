
# Day 10
## Basic Syntax

```sql

````

## Key Points

---

## Examples

## 

```sql

```

---

## Tips 

##
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

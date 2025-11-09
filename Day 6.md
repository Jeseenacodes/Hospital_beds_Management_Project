
# Day 6

## GROUP BY, aggregating by categories

## Challenge
1️⃣ For each hospital service, calculate the total number of patients admitted, total patients refused, and the admission rate (percentage of requests that were admitted). Order by admission rate descending.

```sql
SELECT service, SUM(patients_admitted) AS Total_patients_admitted, 
SUM(patients_refused) AS Total_patients_refused, 
ROUND(SUM(patients_admitted)/ SUM(patients_request) *100,2) AS Admission_rate 
FROM services_weekly 
GROUP BY service 
ORDER BY Admission_rate DESC ;
````
Output:

![Alt text](https://github.com/Jeseenacodes/Hospital_beds_Management_Project/blob/main/Results/Day%206.png)

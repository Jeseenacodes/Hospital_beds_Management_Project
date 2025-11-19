-- Day 15

### Challenge
- Create a comprehensive service analysis report for week 20 showing: service name, total patients admitted that week, total patients refused, 
 average patient satisfaction, count of staff assigned to service, and count of staff present that week. Order by patients admitted descending.




```sql
SELECT
sw.service AS Service_Name,
SUM(sw.patients_admitted) AS Total_Patients_admitted,
SUM(sw.patients_refused) AS Total_Patients_refused,
ROUND (AVG(sw.patient_satisfaction), 2) AS Average_Patient_satisfaction,
COUNT(DISTINCT ss.staff_id) AS Staff_assigned_to_service,
SUM(ss.present) AS Staff_present_that_week
FROM services_weekly AS sw
LEFT JOIN staff_schedule AS ss ON sw.service = ss.service AND sw.week = ss.week 
WHERE sw.week = 20
GROUP BY sw.service
ORDER BY Total_Patients_admitted DESC;
```

<img width="824" height="106" alt="image" src="https://github.com/user-attachments/assets/fa838097-b47d-4dc0-8977-c1fd58eee45f" />

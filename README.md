<p align="center">
  <img src="https://img.shields.io/badge/Project-Hospital_Beds-blue?style=for-the-badge&logo=database&logoColor=white" />
  <img src="https://img.shields.io/badge/Database-MySQL-005C84?style=for-the-badge&logo=mysql&logoColor=white" />
  <img src="https://img.shields.io/badge/Focus-Data_Analytics-orange?style=for-the-badge&logo=databricks&logoColor=white" />
  <img src="https://img.shields.io/badge/Status-In_Process-yellow?style=for-the-badge&logo=clockify&logoColor=white" />
</p>


# Hospital Beds Database 

This document provides SQL queries to **inspect data** and **view table structures** in the `hospital_beds` MySQL database.

---

## 1. Patients Table

### View All Records
```sql
SELECT * FROM patients;  -- Patient records
```

## Table Structure
```sql
DESCRIBE patients;

SHOW COLUMNS FROM patients;

SELECT COLUMN_NAME, DATA_TYPE, IS_NULLABLE, COLUMN_DEFAULT
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_SCHEMA = 'hospital_beds'
  AND TABLE_NAME = 'patients';
```
| Column Name    | Description                    |
| -------------- | ------------------------------ |
| patient_id     | Unique patient identifier      |
| name           | Patient full name              |
| age            | Age of the patient             |
| arrival_date   | Date of arrival                |
| departure_date | Date of discharge              |
| service        | Department/service admitted to |
| satisfaction   | Patient satisfaction score     |

---

## 2. Services Weekly Table
### View All Records
```sql
SELECT * FROM services_weekly;  -- Weekly service-level data
```

## Table Structure
```sql
DESCRIBE services_weekly;

SHOW COLUMNS FROM services_weekly;

SELECT COLUMN_NAME, DATA_TYPE, IS_NULLABLE, COLUMN_DEFAULT
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_SCHEMA = 'hospital_beds'
  AND TABLE_NAME = 'services_weekly';

```
| Column Name          | Description                              |
| -------------------- | ---------------------------------------- |
| week                 | Week number                              |
| month                | Month name/number                        |
| service              | Hospital service                         |
| available_beds       | Beds available during the week           |
| patients_request     | Number of patient requests               |
| patients_admitted    | Number of patients admitted              |
| patients_refused     | Number of patients refused               |
| patient_satisfaction | Average satisfaction score               |
| staff_morale         | Staff morale score                       |
| event                | Special event or notes                   |
---

## 3. Staff Table

### View All Records
```sql
SELECT * FROM staff;  -- List of hospital staff
```
## Table Structure
```sql
SHOW COLUMNS FROM staff;

SELECT COLUMN_NAME, DATA_TYPE, IS_NULLABLE, COLUMN_DEFAULT
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_SCHEMA = 'hospital_beds'
  AND TABLE_NAME = 'staff';


```
| Column Name | Description                         |
| ----------- | ----------------------------------- |
| staff_id    | Unique staff identifier             |
| staff_name  | Name of the staff member            |
| role        | Position/role                       |
| service     | Department/service                  |

## 4. Staff Schedule Table
### View All Records
```sql
SELECT * FROM staff_schedule;  -- Weekly staff schedule
```

## Table Structure
```sql
DESCRIBE staff_schedule;

SHOW COLUMNS FROM staff_schedule;

SELECT COLUMN_NAME, DATA_TYPE, IS_NULLABLE, COLUMN_DEFAULT
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_SCHEMA = 'hospital_beds'
  AND TABLE_NAME = 'staff_schedule';

```
| Column Name | Description                         |
| ----------- | ----------------------------------- |
| week        | Week number                         |
| staff_id    | Unique staff identifier             |
| staff_name  | Name of the staff member            |
| role        | Position/role                       |
| service     | Department/service                  |
| present     | Attendance status                   |
---


--- 
Author: Jeseena Parveen K  
Database: hospital_beds  
Purpose: Database documentation for hospital bed, staff, and patient tracking.

`DISTINCT` removes duplicate rows from your result set, returning only **unique values**.

## Basic Syntax

```sql
SELECT DISTINCT column1, column2
FROM table_name;
```

## Key Concepts

* `DISTINCT` considers **all selected columns together**
* Acts like returning **unique combinations**
* Can impact **performance** on large datasets
* Only returns **one NULL**, even if many exist

## Examples

### 1️⃣ Unique services

```sql
SELECT DISTINCT service
FROM patients;
```

### 2️⃣ Unique combinations with CASE expression

```sql
SELECT DISTINCT
    service,
    CASE
        WHEN age < 18 THEN 'Pediatric'
        WHEN age BETWEEN 18 AND 65 THEN 'Adult'
        ELSE 'Senior'
    END AS age_group
FROM patients;
```

### 3️⃣ Count distinct values

```sql
SELECT COUNT(DISTINCT service) AS unique_services
FROM patients;
```

### 4️⃣ Distinct with multiple columns

```sql
SELECT DISTINCT service, arrival_date
FROM patients
ORDER BY service, arrival_date;
```

## Tips

### DISTINCT vs GROUP BY

```sql
-- These produce the same result:
SELECT DISTINCT service
FROM patients;

SELECT service
FROM patients
GROUP BY service;
```

**Use DISTINCT** → when you only need unique rows
**Use GROUP BY** → when you need aggregates (COUNT, SUM, AVG…)

### DISTINCT applies to the *entire row*

```sql
# This returns unique **(service, name)** pairs.

SELECT DISTINCT service, name
FROM patients;
```


### COUNT(DISTINCT column)

```sql
# Use to count unique values:

COUNT(DISTINCT service)
```

### Performance tip
`DISTINCT` can be expensive on large tables → avoid if `GROUP BY` or indexing can achieve the same result faster.

### DISTINCT + NULL behavior
NULL values are treated as equal → only **one** NULL returned.

---
## Practice Section

1️⃣ List all unique services in the patients table.

```sql
SELECT DISTINCT service FROM patients; 
/*surgery
general_medicine
emergency
ICU*/

```

2️⃣ Find all unique staff roles in the hospital.

```sql
SELECT DISTINCT role FROM staff;
-- doctor nurse nursing_assistant
```

3️⃣ Get distinct months from the services_weekly table.
```sql
SELECT DISTINCT month FROM services_weekly;
```

---
## Challenge 
1️⃣ Find all unique combinations of service and event type 
from the services_weekly table where events are not null or none, 
along with the count of occurrences for each combination. Order by count 
descending.

```sql
SELECT DISTINCT service, event,
COUNT(*) AS event_count 
FROM services_weekly 
WHERE event IS NOT NULL AND LOWER(event) <> 'none'
GROUP BY service, event
ORDER BY event_count DESC; 
```

Output:

![Alt text]()

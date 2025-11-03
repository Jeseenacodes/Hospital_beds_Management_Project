
### Practice Questions

#### 1️⃣ Retrieve all columns from the `patients` table
```sql
SELECT * FROM patients;
````

#### 2️⃣ Select only the `patient_id`, `name`, and `age` columns from the `patients` table

```sql
SELECT patient_id, name, age FROM patients;
```

#### 3️⃣ Display the first 10 records from the `services_weekly` table

```sql
SELECT * FROM services_weekly
LIMIT 10;
```

### Challenge

#### List all unique hospital services available in the hospital

```sql
SELECT * FROM services_weekly; -- service available in services_weekly table

SELECT DISTINCT service FROM services_weekly;
```

**Output:**

```
emergency  
surgery  
general_medicine  
ICU
```

---



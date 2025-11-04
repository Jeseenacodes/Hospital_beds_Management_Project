## Day 2
### Practice Questions

#### The `WHERE` Clause : filters records based on conditions, returning only rows that meet your criteria.

#### Operators

| Type | Operators | Example |
|------|------------|----------|
| **Comparison** | `=`, `!=`, `<>`, `>`, `<`, `>=`, `<=` | `age > 60` |
| **Logical** | `AND`, `OR`, `NOT` | `age > 60 AND service = 'Cardiology'` |
| **Pattern** | `LIKE`, `IN`, `BETWEEN` | `service IN ('Emergency', 'Cardiology')` |

---

## Examples

```sql
-- Single condition
SELECT * FROM patients
WHERE age > 60;

-- Multiple conditions
SELECT * FROM patients
WHERE age > 60 AND service = 'Cardiology';

-- OR condition
SELECT * FROM patients
WHERE service = 'Emergency' OR service = 'Cardiology';

-- IN operator (cleaner than multiple ORs)
SELECT * FROM patients
WHERE service IN ('Emergency', 'Cardiology', 'Neurology');
````

**Tip:** Use `IN` instead of multiple `OR`s — it's more readable and faster.

```sql
WHERE service = 'A' OR service = 'B' OR service = 'C'; -- Avoid:

WHERE service IN ('A', 'B', 'C'); -- Better:
```

---

## Notes

* Strings need **single quotes**, numbers don’t.

  ```sql
  WHERE age = 50       --  correct
  WHERE age = '50'     --  works but not ideal
  WHERE name = 'John'  --  correct
  WHERE name = John    --  ERROR
  ```
* `BETWEEN` is **inclusive** → includes both endpoints:
  ```sql
  WHERE age BETWEEN 18 AND 65;
  ```
* Check for missing values using:
  ```sql
  WHERE column IS NULL;
  WHERE column IS NOT NULL;
  ```
---

## Practice Questions

### 1️⃣ Find all patients who are older than 60 years.

```sql
SELECT * 
FROM patients 
WHERE age > 60
ORDER BY age;
```

### 2️⃣ Retrieve all staff members who work in the 'Emergency' service.

```sql
SELECT * 
FROM staff 
WHERE service IN ('Emergency');
```

### 3️⃣ List all weeks where more than 100 patients requested admission in any service.

```sql
SELECT week 
FROM services_weekly 
WHERE patient_request >= 100;
```

---

### Challenge

Find all patients admitted to **'Surgery'** service with a **satisfaction score below 70**,
showing their `patient_id`, `name`, `age`, and `satisfaction`.

```sql
SELECT 
    patient_id, 
    name, 
    age, 
    satisfaction
FROM patients
WHERE service = 'surgery'
  AND satisfaction < 70;
```

Output: 

![Alt text](https://github.com/Jeseenacodes/Hospital_beds_Management_Project/blob/main/Results/Day%202.png)

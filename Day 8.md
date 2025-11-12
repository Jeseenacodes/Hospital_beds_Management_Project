## **SQL String Functions**

**Purpose:**
String functions help **manipulate and format text data** in your SQL queries.

### 🔠 **Common String Functions**

| **Function**               | **Description**             | **Example Usage**                     |
| -------------------------- | --------------------------- | ------------------------------------- |
| `UPPER(column)`            | Converts text to uppercase  | `UPPER(name)` → “JOHN DOE”            |
| `LOWER(column)`            | Converts text to lowercase  | `LOWER(name)` → “john doe”            |
| `LENGTH(column)`           | Returns string length       | `LENGTH(name)` → `8`                  |
| `CONCAT(str1, str2)`       | Combines (joins) strings    | `CONCAT(name, ' - ', service)`        |
| `SUBSTRING(str, pos, len)` | Extracts part of a string   | `SUBSTRING(name, 1, 3)` → “Joh”       |
| `TRIM(column)`             | Removes spaces at both ends | `TRIM(name)`                          |
| `REPLACE(str, old, new)`   | Replaces text               | `REPLACE(service, 'Emergency', 'ER')` |

###  **Examples**

```sql
-- 1️⃣ Convert to uppercase
SELECT UPPER(name) AS name_upper
FROM patients;

-- 2️⃣ Concatenate columns
SELECT CONCAT(name, ' - ', service) AS patient_info
FROM patients;

-- 3️⃣ Get name length
SELECT name, LENGTH(name) AS name_length
FROM patients
WHERE LENGTH(name) > 15;

-- 4️⃣ Extract substring (first 3 characters)
SELECT SUBSTRING(name, 1, 3) AS name_abbr
FROM patients;

-- 5️⃣ Replace text
SELECT REPLACE(service, 'Emergency', 'ER') AS service_abbr
FROM patients;
```

### Tips

✅ **String concatenation syntax varies by database:**

```sql
-- SQLite / PostgreSQL:
SELECT name || ' - ' || service FROM patients;

-- MySQL:
SELECT CONCAT(name, ' - ', service) FROM patients;
```

✅ **TRIM variants:**

* `LTRIM()` → removes spaces on the **left**
* `RTRIM()` → removes spaces on the **right**
* `TRIM()` → removes spaces on **both sides**

✅ **Case-insensitive comparison:**

```sql
WHERE LOWER(name) = LOWER('john smith');  -- Matches any case
```

✅ **Performance tip:**
Avoid using string functions inside `WHERE` when possible — they can **disable index usage** and slow queries.

✅ **Combine with CASE for custom logic:**

```sql
SELECT
    name,
    CASE
        WHEN LENGTH(name) > 20 THEN SUBSTRING(name, 1, 20) || '...'
        ELSE name
    END AS display_name
FROM patients;
```

---
## Challenge
- Create a patient summary that shows patient_id, full name in uppercase, service in lowercase, age category (if age >= 65 then 'Senior', if age >= 18 then 'Adult', else 'Minor'), and name length. Only show patients  whose name length is greater than 10 characters.*/

```sql
SELECT patient_id, upper(name) as Name, LOWER(service) as Service, LENGTH(name) as Name_length,
 CASE
 when age >= 65 then "Senior"
 when age >= 18 then "Adult"
 else "Minor"
 end as age_group
FROM patients
where LENGTH(name) > 10
```
Output:

![Alt text](https://github.com/Jeseenacodes/Hospital_beds_Management_Project/blob/main/Results/Day%208.png)


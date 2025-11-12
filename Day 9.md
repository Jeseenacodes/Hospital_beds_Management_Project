

##  **SQL Date Functions**

Date functions help you **manipulate, extract, and calculate** information from date and time fields in SQL.


###  **Common Date Functions** *(SQLite syntax shown — varies by database)*

| **Function**           | **Description**                                              | **Example**                    |
| ---------------------- | ------------------------------------------------------------ | ------------------------------ |
| `DATE('now')`          | Returns the current date                                     | `2025-11-11`                   |
| `JULIANDAY(date)`      | Converts a date to Julian day number (used for calculations) | `JULIANDAY('2025-01-01')`      |
| `DATE(date, '+1 day')` | Adds or subtracts time intervals                             | `DATE(arrival_date, '+1 day')` |
| `strftime('%Y', date)` | Extracts the **year**                                        | `strftime('%Y', arrival_date)` |
| `strftime('%m', date)` | Extracts the **month**                                       | `strftime('%m', arrival_date)` |
| `strftime('%d', date)` | Extracts the **day**                                         | `strftime('%d', arrival_date)` |


### **Examples**

```sql
-- 1️⃣ Calculate length of stay in days
SELECT
    patient_id,
    name,
    arrival_date,
    departure_date,
    CAST(JULIANDAY(departure_date) - JULIANDAY(arrival_date) AS INTEGER) AS stay_days
FROM patients;
```

```sql
-- 2️⃣ Extract year and month from date
SELECT
    patient_id,
    strftime('%Y', arrival_date) AS arrival_year,
    strftime('%m', arrival_date) AS arrival_month
FROM patients;
```

```sql
-- 3️⃣ Filter by date range
SELECT *
FROM patients
WHERE arrival_date BETWEEN '2024-01-01' AND '2024-12-31';
```

```sql
-- 4️⃣ Find patients admitted in a specific month (June)
SELECT *
FROM patients
WHERE strftime('%m', arrival_date) = '06';
```


### Tips

✅ **Use ISO format (`YYYY-MM-DD`)**
Ensures compatibility across databases.

✅ **Date difference functions vary by database:**

| **Database**   | **Syntax**                            |
| -------------- | ------------------------------------- |
| **SQLite**     | `JULIANDAY(date2) - JULIANDAY(date1)` |
| **MySQL**      | `DATEDIFF(date2, date1)`              |
| **PostgreSQL** | `date2 - date1`                       |

✅ **Extract date parts using `strftime` (SQLite):**

```sql
strftime('%Y', date)  -- Year (2024)
strftime('%m', date)  -- Month (01–12)
strftime('%d', date)  -- Day (01–31)
strftime('%W', date)  -- Week number
```

✅ **Use date functions cautiously in WHERE**
They can slow queries on large datasets by preventing index use.

✅ **Cast date calculations to correct type**
Use `CAST(... AS INTEGER)` or `REAL` for consistency.

---







Output:

![Alt text]()

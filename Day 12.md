

# NULL in SQL (Clear & Simple)

`NULL` represents **missing or unknown data** in SQL.
It is **not**:

* Zero
* An empty string
* but the absence of a value


## NULL Handling Basics

```sql
-- Check for NULL
WHERE col IS NULL
WHERE col IS NOT NULL

-- Replace NULL with default value
COALESCE(column, default_value)

-- NULL-safe comparison (varies by database)
column IS DISTINCT FROM value
```
```sql
# DO NOT USE
WHERE column = NULL;
WHERE column != NULL;
```

## Counting NULLs
```sql
SELECT
    COUNT(*) AS total_rows,            -- includes NULLs
    COUNT(event) AS non_null_events,   -- excludes NULLs
    COUNT(*) - COUNT(event) AS null_events
FROM services_weekly;
```

## Filter Out Empty + NULL Values
```sql
SELECT *
FROM services_weekly
WHERE event IS NOT NULL
  AND event != '';
```

---

## Practical Examples

### 1️⃣ Find rows where event is NULL

```sql
SELECT *
FROM services_weekly
WHERE event IS NULL;
```

### 2️⃣ Find rows where event is not NULL

```sql
SELECT *
FROM services_weekly
WHERE event IS NOT NULL;
```

### 3️⃣ Replace NULL with a default value

```sql
SELECT 
    service,
    week,
    COALESCE(event, 'No Event') AS event_status
FROM services_weekly;
```

### 4️⃣ Count NULL vs non-NULL values

```sql
SELECT
    COUNT(*) AS total_rows,
    COUNT(event) AS non_null_events,
    COUNT(*) - COUNT(event) AS null_events
FROM services_weekly;
```

### 5️⃣ Exclude NULL and empty strings

```sql
SELECT *
FROM services_weekly
WHERE event IS NOT NULL 
  AND event != '';
```

---

## Tips 

### Do NOT use `=` or `!=` with NULL

```sql
-- Wrong
WHERE event = NULL
WHERE event != NULL

-- Correct
WHERE event IS NULL
WHERE event IS NOT NULL
```

### ➗ NULL in arithmetic results in NULL

```sql
5 + NULL = NULL
NULL * 10 = NULL
```

### COALESCE returns the **first non-NULL** value

```sql
COALESCE(column1, column2, 'default')
```

### COUNT rules

* `COUNT(*)` → counts **all rows** (including NULLs)
* `COUNT(column)` → counts **only non-NULL** entries

### ↕ Sorting NULLs

```sql
-- Move NULLs to the end
ORDER BY COALESCE(event, 'ZZZZ');
```

### Empty string ('') is NOT NULL

They are different.
If needed, check both:

```sql
WHERE column IS NULL OR column = ''
```

---
## Practice question

-- Find all weeks in services_weekly where no special event occurred.
```sql


```

-- Count how many records have null or empty event values.
```sql


```

-- List all services that had at least one week with a special event.
```sql


```
---

## Challenge
-- Analyze the event impact by comparing weeks with events vs 
weeks without events. Show: event status ('With Event' or 'No Event'), 
count of weeks, average patient satisfaction, and average staff morale. 
Order by average patient satisfaction descending.

```sql


```

Output:

![Alt text]()

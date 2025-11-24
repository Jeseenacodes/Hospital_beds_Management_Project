
#  **UNION vs UNION ALL**

### **Key Differences**

* **`UNION`**

  * Removes duplicates
  * Performs a DISTINCT operation → **slower**
* **`UNION ALL`**

  * Keeps all rows (duplicates included)
  * No DISTINCT → **faster**

---

#  **Rules for Using UNION / UNION ALL**

* **Both SELECT statements must have:** The **same number of columns** & **Compatible data types** in each column position (e.g., string with string, number with number)
* **Column Naming Rules** - The **column names come from the first SELECT statement**.
* **ORDER BY Placement** - `ORDER BY` can only appear **once**, at the end of the final UNION:

```sql
SELECT ...
UNION
SELECT ...
ORDER BY column_name;
```

---

# **Performance Tip**

Use **`UNION ALL`** when:

* You don’t need to remove duplicates
* You want the fastest performance
* Your data sets are guaranteed not to overlap

Use **`UNION`** when:

* You must remove duplicate rows across datasets

---
| Feature / Rule             | **UNION**                                      | **UNION ALL**                                  |
| -------------------------- | ---------------------------------------------- | ---------------------------------------------- |
| **Duplicates removed?**    |  Yes                                          | No (keeps all rows)                          |
| **Performance**            | Slower (deduplication required)                | Faster (no DISTINCT check)                     |
| **Use case**               | When you must remove duplicate rows            | When duplicates don’t matter or won’t occur    |
| **Column requirements**    | Same number of columns + compatible data types | Same number of columns + compatible data types |
| **Column names come from** | First SELECT                                   | First SELECT                                   |
| **Allows ORDER BY?**       | Only at the very end                           | Only at the very end                           |
| **Typical scenario**       | Combine similar tables but need unique results | Stack/append datasets for reporting            |
---

## Challenge
1️⃣

```sql
SELECT patient_id AS Id, name AS Name, "Patient" AS Type, service AS Service
FROM patients 
WHERE service IN ("emergency", "surgery")
UNION ALL
SELECT staff_id AS Id, staff_name AS Name,  "Staff" AS Type, service AS Service
FROM staff 
WHERE service IN ("emergency", "surgery")
ORDER BY Type, service, Name;
```

Output:

<img width="316" height="209" alt="Screenshot 2025-11-23 214947" src="https://github.com/user-attachments/assets/2f66e152-ec7a-433f-8c8d-b1281e47618f" />

<img width="315" height="198" alt="image" src="https://github.com/user-attachments/assets/6e2ef5d8-c968-4238-bbd3-be8b65662bbc" />


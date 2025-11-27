
#  SQL Murder Mystery : How I Identified the Killer

### **1. Identifing where and when the crime happened**

```sql
SELECT *
FROM evidence
WHERE room = 'CEO Office'
  AND found_time BETWEEN 
      '2025-10-15 20:00:00'
  AND '2025-10-15 22:00:00';

```

### **Output:**

| evidence_id | room       | description                 | found_time       |
| ----------- | ---------- | --------------------------- | ---------------- |
| 1           | CEO Office | Fingerprint on desk         | 2025-10-15 21:05 |
| 2           | CEO Office | Keycard swipe logs mismatch | 2025-10-15 21:10 |

### **Conclusion:**

> The murder happened **in the CEO Office**, and the crime occurred **around 9 PM**, setting the **time window** for the rest of the investigation.

---
### 2. Analyzed who accessed critical areas at the time

```sql
SELECT k.employee_id, e.name, k.entry_time, k.exit_time
FROM keycard_logs k
JOIN employees e ON k.employee_id = e.employee_id
WHERE k.room = 'CEO Office'
  AND k.entry_time BETWEEN '2025-10-15 20:45:00' AND '2025-10-15 21:15:00'
ORDER BY k.entry_time;
```

### **Output:**

| employee_id | name        | entry_time | exit_time |
| ----------- | ----------- | ---------- | --------- |
| 4           | David Kumar | 20:50      | 21:00     |

### **Conclusion:**

> Only **one person** entered the CEO Office in the critical time window: **David Kumar** which makes him the **primary suspect**.

---

### 3. Cross-check alibis with actual logs

```sql

SELECT a.employee_id, emp.name, a.claimed_location, a.claim_time, k.room, k.entry_time
FROM alibis a
JOIN employees emp ON a.employee_id = emp.employee_id
JOIN keycard_logs k ON k.employee_id = a.employee_id
WHERE a.claim_time BETWEEN '2025-10-15 20:30:00' AND '2025-10-15 21:30:00'
  AND a.claimed_location <> 'CEO Office'
  AND k.room = 'CEO Office'
  AND k.entry_time BETWEEN '2025-10-15 20:45:00' AND '2025-10-15 21:15:00';
```

### **Output:**

| employee_id | name        | claimed_location | claim_time | room_actual | entry_time |
| ----------- | ----------- | ---------------- | ---------- | ----------- | ---------- |
| 4           | David Kumar | Server Room      | 20:50      | CEO Office  | 20:50      |

### **Conclusion:**
> David claimed he was in the **Server Room**, But keycard logs show he was in the **CEO Office** at the same time. **David lied about his whereabouts which makes him more suspicious. 

---

### 4. Investigate suspicious calls made around the time 

```sql
SELECT c.call_id, c.call_time, c.duration_sec,
       caller.name AS caller, receiver.name AS receiver
FROM calls c
LEFT JOIN employees caller ON c.caller_id = caller.employee_id
LEFT JOIN employees receiver ON c.receiver_id = receiver.employee_id
WHERE c.call_time BETWEEN '2025-10-15 20:50:00' AND '2025-10-15 21:00:00'
ORDER BY c.call_time;
```

### **Output:**

| call_id | call_time | duration | caller      | receiver      |
| ------- | --------- | -------- | ----------- | ------------- |
| 1       | 20:55     | 45 sec   | David Kumar | Alice Johnson |

### **Conclusion:**

> David made a **brief call** shortly before the murder, which could indicate planning, coordination, or stress. This is the third red flag where an unusual call is made right before murder.**

---

### 5. Match evidence with movements and claims

```sql
SELECT 
    e.evidence_id, e.description, e.found_time,
    k.employee_id, emp.name, k.entry_time
FROM evidence e
JOIN keycard_logs k ON e.room = k.room
JOIN employees emp ON k.employee_id = emp.employee_id
WHERE e.room = 'CEO Office'
  AND k.entry_time BETWEEN 
        DATE_SUB(e.found_time, INTERVAL 2 HOUR)
    AND DATE_ADD(e.found_time, INTERVAL 2 HOUR);
```

### **Output:**

| evidence              | found_time | employee    | entry_time |
| --------------------- | ---------- | ----------- | ---------- |
| Keycard logs mismatch | 21:10      | David Kumar | 20:50      |
| Fingerprint on desk   | 21:05      | David Kumar | 20:50      |

### **Conclusion:**

> **Both pieces of evidence** in the CEO Office tie back to David’s entry window. Fingerprints + keycard irregularities strongly link him to the scene, which is the **Fourth red flag: physical evidence matches David's presence.**
---


### 6. Combining All Findings 

* People who entered the CEO Office
* People with suspicious calls
* People with alibi conflicts
* Presence of evidence

```sql
WITH entered AS (
  SELECT DISTINCT employee_id
  FROM keycard_logs
  WHERE room = 'CEO Office'
    AND entry_time BETWEEN '2025-10-15 20:45:00' AND '2025-10-15 21:15:00'
),
calls AS (
  SELECT DISTINCT caller_id AS employee_id FROM calls
  WHERE call_time BETWEEN '2025-10-15 20:50:00' AND '2025-10-15 21:00:00'
  UNION
  SELECT DISTINCT receiver_id FROM calls
  WHERE call_time BETWEEN '2025-10-15 20:50:00' AND '2025-10-15 21:00:00'
),
alibi_conflict AS (
  SELECT DISTINCT a.employee_id
  FROM alibis a
  JOIN keycard_logs k ON a.employee_id = k.employee_id
  WHERE a.claim_time BETWEEN '2025-10-15 20:30:00' AND '2025-10-15 21:30:00'
    AND a.claimed_location <> 'CEO Office'
    AND k.room = 'CEO Office'
    AND k.entry_time BETWEEN '2025-10-15 20:45:00' AND '2025-10-15 21:15:00'
),
evidence_here AS (
  SELECT 1 FROM evidence WHERE room = 'CEO Office' LIMIT 1
)
SELECT emp.name AS killer
FROM employees emp
JOIN entered e ON emp.employee_id = e.employee_id
LEFT JOIN calls c ON emp.employee_id = c.employee_id
LEFT JOIN alibi_conflict a ON emp.employee_id = a.employee_id
WHERE (c.employee_id IS NOT NULL OR a.employee_id IS NOT NULL)
  AND EXISTS (SELECT 1 FROM evidence_here)
LIMIT 1;


   WHERE k.employee_id = a.employee_id
     AND k.room = 'CEO Office'
     AND k.entry_time BETWEEN '2025-10-15 20:45:00' AND '2025-10-15 21:15:00'
 );
```

The final CTE query returns:

| killer          |
| --------------- |
| **David Kumar** |

<img width="77" height="46" alt="image" src="https://github.com/user-attachments/assets/19c7e3b3-058c-48ec-b6e3-aed63649838f" />


##  **Final Conclusion: David Kumar is the Killer**

 1. David Kumar was the ONLY person who entered the CEO Office during the murder window.
 2. He lied about his location, claiming he was in the Server Room while keycard logs place him at the crime scene.
 3. He made a suspicious phone call minutes before the murder.
 4. Physical evidence (fingerprints + keycard mismatch) matches his movements.

Hence the SQL investigation clearly identifies:

#  **Killer: David Kumar**





-- Identify where and when the crime happened	

```sql
SELECT *
FROM evidence
WHERE room = 'CEO Office'
  AND found_time BETWEEN 
      '2025-10-15 20:00:00'
  AND '2025-10-15 22:00:00';

```
Output: 

1	CEO Office	Fingerprint on desk	2025-10-15 21:05:00
2	CEO Office	Keycard swipe logs mismatch	2025-10-15 21:10:00
		

-- Analyze who accessed critical areas at the time
```sql
SELECT k.employee_id, e.name, k.entry_time, k.exit_time
FROM keycard_logs k
JOIN employees e ON k.employee_id = e.employee_id
WHERE k.room = 'CEO Office'
  AND k.entry_time BETWEEN '2025-10-15 20:45:00' AND '2025-10-15 21:15:00'
ORDER BY k.entry_time;
```
Output: 
 4	David Kumar	2025-10-15 20:50:00	2025-10-15 21:00:00

-- Cross-check alibis with actual logs	J
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
Output:

4	David Kumar	Server Room	2025-10-15 20:50:00	CEO Office	2025-10-15 20:50:00

-- Investigate suspicious calls made around the time	
```sql
SELECT c.call_id, c.call_time, c.duration_sec,
       caller.name AS caller, receiver.name AS receiver
FROM calls c
LEFT JOIN employees caller ON c.caller_id = caller.employee_id
LEFT JOIN employees receiver ON c.receiver_id = receiver.employee_id
WHERE c.call_time BETWEEN '2025-10-15 20:50:00' AND '2025-10-15 21:00:00'
ORDER BY c.call_time;
```
Output:

1	2025-10-15 20:55:00	45	David Kumar	Alice Johnson

-- Match evidence with movements and claims	JOIN, WHERE
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
Output:

2	Keycard swipe logs mismatch	2025-10-15 21:10:00	4	David Kumar	2025-10-15 20:50:00
1	Fingerprint on desk	2025-10-15 21:05:00	4	David Kumar	2025-10-15 20:50:00   


-- Combine all findings to identify the killer	
-- Using ctes & joins

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

Output:

<img width="77" height="46" alt="image" src="https://github.com/user-attachments/assets/19c7e3b3-058c-48ec-b6e3-aed63649838f" />



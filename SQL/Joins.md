### Types of Joins

Example Tables
### Table-A (Employee)

| EmployeeID | Name    |
| ---------- | ------- |
| 1          | Alice   |
| 2          | Bob     |
| 3          | Charlie |
| 4          | David   |
### Table-B (Departments)

| EmployeeID | Department  |
| ---------- | ----------- |
| 2          | HR          |
| 3          | Engineering |
| 5          | Marketing   |


#### 1. **INNER JOIN**

- **What it does:** Returns only the rows where there is a match in both tables.
- **Result:** Only the matching records from both tables.
- **Use case:** When you want to see only the records that exist in both tables.

| EmployeeID | Name    | Department  |
| ---------- | ------- | ----------- |
| 2          | Bob     | HR          |
| 3          | Charlie | Engineering |

**Example:**

```sql
SELECT a.*, b.*
FROM TableA a
INNER JOIN TableB b ON a.id = b.id;
```

This returns rows where `TableA.id` equals `TableB.id`.

---

#### 2. **LEFT JOIN (or LEFT OUTER JOIN)**

- **What it does:** Returns all rows from the **left** table, and matched rows from the right table. If there is no match, NULLs are returned for columns from the right table.
- **Result:** All rows from left table + matching rows from right table (or NULL if no match).
- **Use case:** When you want to keep all records from the left table regardless of matches in the right table
> The left join return the data by combining the left and right tables. if the values are not matched in the both tables. for the right table it add NULL

| EmployeeID | Name    | Department  |
| ---------- | ------- | ----------- |
| 1          | Alice   | NULL        |
| 2          | Bob     | HR          |
| 3          | Charlie | Engineering |
| 4          | David   | NU

**Example:**

```sql
SELECT a.*, b.*
FROM TableA a
LEFT JOIN TableB b ON a.id = b.id;
```

Returns all rows from `TableA` and matches from `TableB`, or NULL for `TableB` columns if no match.

---

#### 3. **RIGHT JOIN (or RIGHT OUTER JOIN)**

- **What it does:** Opposite of LEFT JOIN. Returns all rows from the **right** table, and matched rows from the left table. NULLs for left table columns when no match.
- **Result:** All rows from right table + matching rows from left table (or NULL if no match).
- **Use case:** When you want to keep all records from the right table regardless of matches in the left table.

| EmployeeID | Name    | Department  |
| ---------- | ------- | ----------- |
| 2          | Bob     | HR          |
| 3          | Charlie | Engineering |
| 5          | NULL    | Marketing   |

**Example:**

```sql
SELECT a.*, b.*
FROM TableA a
RIGHT JOIN TableB b ON a.id = b.id;
```

Returns all rows from `TableB` and matching rows from `TableA`, or NULL if no match.

---

#### 4. **FULL JOIN (or FULL OUTER JOIN)**

- **What it does:** Combines the results of LEFT and RIGHT joins. Returns all rows from both tables, matching where possible. If no match, NULLs appear for missing side.
- **Result:** All rows from both tables with NULLs for non-matching columns.
- **Use case:** When you want a complete set of records from both tables, including unmatched rows.

| EmployeeID | Name    | Department  |
| ---------- | ------- | ----------- |
| 2          | Bob     | HR          |
| 3          | Charlie | Engineering |
| 5          | NULL    | Marketing   |

**Example:**

```sql
SELECT a.*, b.*
FROM TableA a
FULL OUTER JOIN TableB b ON a.id = b.id;
```

Returns all rows from both tables, matching where available, otherwise NULLs for missing matches.

---

### How to Choose Which Join to Use?

|Scenario|Which Join?|Why?|
|---|---|---|
|You want only records where both tables have matching keys|**INNER JOIN**|To filter out unmatched rows|
|You want all records from the left table, and matching records from the right|**LEFT JOIN**|Keep all left table data regardless of right table matches|
|You want all records from the right table, and matching records from the left|**RIGHT JOIN**|Keep all right table data regardless of left table matches|
|You want all records from both tables, with matches where possible|**FULL OUTER JOIN**|To see the complete data set, including unmatched rows on either side|

---

### Quick Visual Example

Assuming:

- TableA IDs: 1, 2, 3, 4
- TableB IDs: 3, 4, 5, 6

|Join Type|Resulting IDs|
|---|---|
|INNER JOIN|3, 4|
|LEFT JOIN|1, 2, 3, 4|
|RIGHT JOIN|3, 4, 5, 6|
|FULL OUTER JOIN|1, 2, 3, 4, 5, 6|

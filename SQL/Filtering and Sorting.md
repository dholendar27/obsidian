---
date created: 2025-10-06 16:59
date updated: 2025-10-06 17:01
---

## 1. Filtering in SQL

Filtering means **selecting rows** from a table that meet certain conditions. This is done primarily using the `WHERE` clause.

### Basic Syntax:

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

### How filtering works:

- The `WHERE` clause specifies one or more conditions.
- Only rows that satisfy these conditions are returned.
- Conditions can use comparison operators, logical operators, pattern matching, and more.

---

### Common Filtering Operators:

| Operator     | Description                 | Example                                    |
| ------------ | --------------------------- | ------------------------------------------ |
| `=`          | Equal to                    | `WHERE age = 25`                           |
| `<>` or `!=` | Not equal to                | `WHERE status <> 'active'`                 |
| `<`          | Less than                   | `WHERE price < 100`                        |
| `>`          | Greater than                | `WHERE salary > 50000`                     |
| `<=`         | Less than or equal to       | `WHERE date <= '2025-01-01'`               |
| `>=`         | Greater than or equal to    | `WHERE rating >= 4`                        |
| `BETWEEN`    | Between a range (inclusive) | `WHERE age BETWEEN 18 AND 30`              |
| `IN`         | Match any value in a list   | `WHERE country IN ('USA', 'UK', 'Canada')` |
| `LIKE`       | Pattern matching            | `WHERE name LIKE 'J%'`                     |
| `IS NULL`    | Check for NULL values       | `WHERE address IS NULL`                    |

#### 1. `BETWEEN`

##### Purpose:

- Used to filter the result set to include values **within a specific range**.
- The range is **inclusive** of the boundary values.

##### Syntax:

```sql
column_name BETWEEN value1 AND value2
```

##### Explanation:

- Returns rows where the column value is greater than or equal to `value1` and less than or equal to `value2`.

- Works for numbers, dates, and other comparable data types.

##### Example:

```sql
SELECT * FROM employees
WHERE salary BETWEEN 30000 AND 60000;
```

- Selects employees with salaries between 30,000 and 60,000 inclusive.

##### Important:

- `BETWEEN` can also work with dates:

```sql
SELECT * FROM orders
WHERE order_date BETWEEN '2025-01-01' AND '2025-01-31';
```

- Gets orders placed in January 2025.

---

### 2. `LIKE`

#### Purpose:

- Used for **pattern matching** in text/string columns.

#### Syntax:

```sql
column_name LIKE pattern
```

#### Wildcards used in `LIKE`:

| Wildcard | Meaning                         | Example                                        |
| -------- | ------------------------------- | ---------------------------------------------- |
| `%`      | Matches zero or more characters | `LIKE 'A%'` matches anything starting with "A" |
| `_`      | Matches exactly one character   | `LIKE '_at'` matches "cat", "bat", etc.        |

#### Example:

```sql
SELECT * FROM customers
WHERE name LIKE 'J%';
```

- Finds all customers whose names start with "J".

#### More examples:

- Names ending with "son":

```sql
WHERE name LIKE '%son'
```

- Names with "an" in the middle:

```sql
WHERE name LIKE '%an%'
```

- Names with exactly 4 letters starting with "A":

```sql
WHERE name LIKE 'A___'
```

---

### 3. `NULL` in SQL

#### What is `NULL`?

- `NULL` represents a **missing or unknown** value in a database.

- It is **not** the same as zero, empty string, or false — it means "no data" or "value is unknown."

#### Important points about `NULL`:

- You **cannot** use `=` or `<>` to check for NULL because `NULL` is not a value.
- Comparisons involving `NULL` always return **unknown** (which behaves like false in WHERE clauses).

---

### 4. `IS NULL` and `IS NOT NULL`

#### Purpose:

- Used to test whether a column contains `NULL` or not.

#### Syntax:

```sql
column_name IS NULL
column_name IS NOT NULL
```

#### Example:

```sql
SELECT * FROM employees
WHERE manager_id IS NULL;
```

- Selects employees who **do not have a manager** assigned (maybe top-level managers).

```sql
SELECT * FROM orders
WHERE shipped_date IS NOT NULL;
```

- Selects orders that **have been shipped**.

---

### 5. Other Common Filtering Operators in SQL

#### `IN`

- Checks if a value matches **any value** in a list.

```sql
SELECT * FROM products
WHERE category IN ('Electronics', 'Books', 'Toys');
```

#### `NOT IN`

- Checks if a value is **not** in the list.

```sql
SELECT * FROM products
WHERE category NOT IN ('Electronics', 'Books');
```

#### `<>` or `!=`

- Means **not equal to**.

```sql
SELECT * FROM employees
WHERE department <> 'HR';
```

#### `>`, `<`, `>=`, `<=`

- Standard comparison operators (greater than, less than, etc.).

---

## Summary Table

| Operator      | Purpose                         | Example                           |
| ------------- | ------------------------------- | --------------------------------- |
| `BETWEEN`     | Filter within range (inclusive) | `salary BETWEEN 30000 AND 60000`  |
| `LIKE`        | Pattern matching in strings     | `name LIKE 'J%'`                  |
| `IS NULL`     | Checks if value is NULL         | `manager_id IS NULL`              |
| `IS NOT NULL` | Checks if value is NOT NULL     | `shipped_date IS NOT NULL`        |
| `IN`          | Check if value is in a list     | `category IN ('Books', 'Toys')`   |
| `NOT IN`      | Check if value is NOT in a list | `category NOT IN ('Electronics')` |
| `<>` or `!=`  | Not equal to                    | `department <> 'Sales'`           |

---

### Combining Conditions with Logical Operators:

- `AND`: All conditions must be true.
- `OR`: At least one condition must be true.
- `NOT`: Negates a condition.

Example:

```sql
SELECT * FROM employees
WHERE department = 'Sales'
  AND salary > 50000;
```

---

## 2. Sorting in SQL

Sorting arranges the result rows in a specified order using the `ORDER BY` clause.

### Basic Syntax:

```sql
SELECT column1, column2, ...
FROM table_name
ORDER BY column1 [ASC|DESC], column2 [ASC|DESC], ...;
```

- `ASC` means ascending order (default).
- `DESC` means descending order.

---

### Example:

```sql
SELECT name, salary
FROM employees
ORDER BY salary DESC, name ASC;
```

This sorts employees first by salary (highest first), then by name alphabetically for those with the same salary.

---

## Combining Filtering and Sorting

You can use both together:

```sql
SELECT name, age, salary
FROM employees
WHERE age > 30
ORDER BY salary DESC;
```

- Filters employees older than 30.
- Sorts the filtered list by salary in descending order.

```SQL
SELECT column1, column2, ...
FROM table_name
WHERE condition        -- filters rows before grouping (if any)
ORDER BY column        -- sorts the final result
```
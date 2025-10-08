---
date created: 2025-10-08 09:15
---

## What is a Window Function?

A **window function** performs a calculation across a **"window" of rows** related to the current row. This window is defined by the `OVER()` clause.

> [!info]+ Note
> A **window function** does a calculation **for each row**, but it can **look at other rows** around it while doing that.\
> You tell SQL **which rows to look at** using the `OVER()` clause — that's your **"window"**.

**Basic Syntax:**

```sql
function_name (expression) OVER (
    [PARTITION BY ...]
    [ORDER BY ...]
    [ROWS BETWEEN ...]
)
```

---

## Common Window Functions

| Function       | Description                                     |
| -------------- | ----------------------------------------------- |
| `ROW_NUMBER()` | Assigns a unique row number within a partition  |
| `RANK()`       | Assigns a rank to each row (ties get same rank) |
| `DENSE_RANK()` | Like `RANK()` but without gaps in ranking       |
| `NTILE(n)`     | Divides rows into `n` buckets                   |
| `LAG()`        | Gets value from previous row                    |
| `LEAD()`       | Gets value from next row                        |
| `SUM()`        | Running total (or custom aggregate)             |
| `AVG()`        | Running average (or partitioned average)        |

---

## Example Table: `sales`

| id | employee | region | sales |
| -- | -------- | ------ | ----- |
| 1  | Alice    | East   | 100   |
| 2  | Bob      | East   | 200   |
| 3  | Alice    | East   | 150   |
| 4  | Carol    | West   | 300   |
| 5  | Alice    | East   | 250   |
| 6  | Carol    | West   | 200   |

---

## 1. `ROW_NUMBER()`

**Goal:** Assign a unique number to each row **per employee**, ordered by sales.

```sql
SELECT
  employee,
  sales,
  ROW_NUMBER() OVER (PARTITION BY employee ORDER BY sales DESC) AS row_num
FROM sales;
```

### Output:

| employee | sales | row_num |
| -------- | ----- | ------- |
| Alice    | 250   | 1       |
| Alice    | 150   | 2       |
| Alice    | 100   | 3       |
| Bob      | 200   | 1       |
| Carol    | 300   | 1       |
| Carol    | 200   | 2       |

---

## 2. `SUM()` as a Window Function

**Goal:** Running total of sales **per employee**.

```sql
SELECT
  employee,
  sales,
  SUM(sales) OVER (PARTITION BY employee ORDER BY id) AS running_total
FROM sales;
```

### Output:

| employee | sales | running_total |
| -------- | ----- | ------------- |
| Alice    | 100   | 100           |
| Alice    | 150   | 250           |
| Alice    | 250   | 500           |
| Bob      | 200   | 200           |
| Carol    | 300   | 300           |
| Carol    | 200   | 500           |

---

## 3. `RANK()` vs `DENSE_RANK()`

**Goal:** Rank employees within their region by sales.

```sql
SELECT
  employee,
  region,
  sales,
  RANK() OVER (PARTITION BY region ORDER BY sales DESC) AS sales_rank,
  DENSE_RANK() OVER (PARTITION BY region ORDER BY sales DESC) AS dense_rank
FROM sales;
```

### Output:

| employee | region | sales | sales_rank | dense_rank |
| -------- | ------ | ----- | ---------- | ---------- |
| Bob      | East   | 200   | 1          | 1          |
| Alice    | East   | 150   | 2          | 2          |
| Alice    | East   | 100   | 3          | 3          |
| Carol    | West   | 300   | 1          | 1          |
| Carol    | West   | 200   | 2          | 2          |

---

## 4. `LAG()` and `LEAD()`

**Goal:** Compare each employee’s sales with previous and next row.

```sql
SELECT
  employee,
  sales,
  LAG(sales) OVER (PARTITION BY employee ORDER BY id) AS prev_sale,
  LEAD(sales) OVER (PARTITION BY employee ORDER BY id) AS next_sale
FROM sales;
```

### Output:

| employee | sales | prev_sale | next_sale |
| -------- | ----- | --------- | --------- |
| Alice    | 100   | NULL      | 150       |
| Alice    | 150   | 100       | 250       |
| Alice    | 250   | 150       | NULL      |
| Bob      | 200   | NULL      | NULL      |
| Carol    | 300   | NULL      | 200       |
| Carol    | 200   | 300       | NULL      |

---

## Why Use Window Functions?

- Get **rankings**, **percentiles**, or **running totals** without collapsing rows
- Compare values in **different rows** (e.g. month-over-month sales)
- Perform **analytics and reporting** more efficiently

---

## Summary Table

| Window Function | Use Case Example                    |
| --------------- | ----------------------------------- |
| `ROW_NUMBER()`  | Pagination, unique ordering         |
| `RANK()`        | Leaderboards, competition ranking   |
| `DENSE_RANK()`  | Ranking without gaps                |
| `NTILE(n)`      | Bucketing data (quartiles, deciles) |
| `SUM()/AVG()`   | Running totals, cumulative averages |
| `LAG()/LEAD()`  | Compare previous/next row's value   |


##  Aggregation Functions (COUNT, SUM, AVG, etc.)

These functions operate on **groups of rows** (or the entire table if no grouping is done).

|Function|Description|
|---|---|
|`COUNT()`|Counts the number of rows|
|`SUM()`|Adds up values|
|`AVG()`|Calculates the average|
|`MIN()`|Finds the smallest value|
|`MAX()`|Finds the largest value|

### Example Table: `sales`

|id|customer|amount|product|
|---|---|---|---|
|1|Alice|100|Pen|
|2|Bob|200|Book|
|3|Alice|150|Book|
|4|Bob|50|Pen|
|5|Charlie|300|Notebook|

---

###  Example of COUNT, SUM, AVG

```sql
SELECT COUNT(*) FROM sales;
-- Output: 5 (there are 5 rows in the table)

SELECT SUM(amount) FROM sales;
-- Output: 800 (100 + 200 + 150 + 50 + 300)

SELECT AVG(amount) FROM sales;
-- Output: 160 (800 / 5)
```

---

##  GROUP BY

### Purpose:

To group **rows that have the same values** in specified columns into summary rows, like "total sales per customer".

### Syntax:

```sql
SELECT column, AGG_FUNC(column)
FROM table
GROUP BY column;
```

###  Example: Total sales per customer

```sql
SELECT customer, SUM(amount) AS total_sales
FROM sales
GROUP BY customer;
```

**Result:**

|customer|total_sales|
|---|---|
|Alice|250|
|Bob|250|
|Charlie|300|

**Explanation:**

- It groups rows **by customer**.
- For each customer, it **adds the `amount`**.

---

## HAVING

### Purpose:

To filter **groups created by `GROUP BY`** based on aggregate conditions.

> `HAVING` is like `WHERE`, but for **groups**, not rows.

### Syntax:

```sql
SELECT column, AGG_FUNC(column)
FROM table
GROUP BY column
HAVING condition_on_aggregates;
```

###  Example: Show customers with total sales over 250

```sql
SELECT customer, SUM(amount) AS total_sales
FROM sales
GROUP BY customer
HAVING SUM(amount) > 250;
```

**Result:**

|customer|total_sales|
|---|---|
|Charlie|300|

**Explanation:**

- Groups by `customer`
- Calculates total `amount`
- Filters **only** those whose total is **greater than 250**

---

## Difference Between WHERE and HAVING

|Clause|Used With|Filters On|
|---|---|---|
|`WHERE`|Rows (before grouping)|Columns/Row-level values|
|`HAVING`|Groups (after grouping)|Aggregate functions (like SUM)|

### Example:

```sql
-- Only include rows with amount > 100
-- Then group by customer
-- Then filter groups with total_sales > 250

SELECT customer, SUM(amount) AS total_sales
FROM sales
WHERE amount > 100
GROUP BY customer
HAVING SUM(amount) > 250;
```

---

## Summary

|Concept|Description|
|---|---|
|`COUNT()`|Counts rows|
|`SUM()`|Total of numeric values|
|`AVG()`|Average value|
|`GROUP BY`|Groups rows by column values|
|`HAVING`|Filters aggregated/grouped results|

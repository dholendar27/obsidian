## What is transaction
Transaction refers to a sequence of one or more operations performed in the database as the single logical unit of work.
- A transaction ensures that either all the operations are successfully executed (committed) or none of them take effect (rolled back).
***Example:*** Let’s consider an online banking application:
***Transaction**: When a user performs a ***money transfer**, several operations occur, such as:
- **Reading** the account balance of the sender.
- ***Writing** the deducted amount from the sender’s account.
- **Writing*** the added amount to the recipient’s account.
---
## ACID
### 1. **Atomicity**
**Meaning:** A transaction is "all or nothing." If any part of the transaction fails, the entire transaction fails and the database stays as it was before the transaction started.
**Example:**  
Imagine you want to transfer $100 from **Account A** to **Account B**. This involves two steps:
- Deduct $100 from Account A
- Add $100 to Account B

If the deduction succeeds but the addition fails (maybe due to a power outage), **atomicity** ensures that the deduction is also undone, so no money disappears or appears out of nowhere.

---
### 2. **Consistency**
**Meaning:** The database rules are never broken. After a transaction finishes, the database must still follow all constraints, such as uniqueness, foreign keys, and valid data types.
**Example:**  
Suppose there is a rule that a bank account balance cannot be negative. If a transaction tries to withdraw more money than available, **consistency** prevents this transaction from completing because it would break the rule. The database remains consistent before and after.

---
### 3. **Isolation**

**Meaning:** Multiple transactions happening at the same time don’t affect each other’s outcomes. Each transaction behaves like it's running alone.
**Example:**  
Two customers try to book the **last available seat** on a flight at the same time. Isolation ensures that one transaction will complete first and reserve the seat, while the other transaction will fail or wait, so only one person gets the seat.

---
### 4. **Durability**

**Meaning:** Once a transaction is committed (finished successfully), the changes are permanent—even if the system crashes immediately after.

**Example:**  
You save a document and the system crashes suddenly. Thanks to durability, when the system restarts, the document is still saved exactly as you left it, with no loss of data.

## **Commit & Rollback**
These are **transaction control statements**:
- **COMMIT**
    - Saves all changes made during the transaction.
    - Makes the changes **permanent** and visible to other transactions.
- **ROLLBACK**
    - Undoes all changes made during the transaction.
    - Used when an error occurs or if the changes are not to be saved.
    - Restores the database to its state before the transaction began.

### Dirty Read
> A transaction reads uncommitted data from another transaction.

**Example:**
1. **Transaction A**:
    
    ```sql
    UPDATE accounts SET balance = balance - 100 WHERE id = 1;
    -- Not committed yet
    ```
1. **Transaction B** (at Read Uncommitted level):
    ```sql
    SELECT balance FROM accounts WHERE id = 1; -- Sees the reduced balance
    ```
2. Then **Transaction A rolls back**, meaning the update is undone — but **Transaction B already read a value that never officially existed.**
---
###  Non-Repeatable Read

> A transaction reads the same row twice and gets different results due to another transaction's update and commit.

**Example:**
1. **Transaction A**:
    ```sql
    SELECT balance FROM accounts WHERE id = 1; -- Gets $500
    ```
2. **Transaction B**:
    ```sql
    UPDATE accounts SET balance = 300 WHERE id = 1;
    COMMIT;
    ```
3. **Transaction A (again)**: 
``` sql
SELECT balance FROM accounts WHERE id = 1; -- Now gets $300
```
→ The value changed **during** the transaction — this is a **non-repeatable read**.

---
### Phantom Read

> A transaction reads a set of rows that match a condition, and later a new row that matches the condition is inserted/removed by another transaction.

**Example:**

1. **Transaction A**:
```
SELECT * FROM orders WHERE amount > 100; -- Returns 5 rows
```
1. **Transaction B**:
 ```sql
  INSERT INTO orders (id, amount) VALUES (101, 150);   
  COMMIT;   
```
    
3. **Transaction A (again)**:
 
  ```sql
  SELECT * FROM orders WHERE amount > 100;
   -- Now returns 6 rows
   ```

→ The new row is a **phantom**, not seen before — it appears due to another transaction’s insert.

---
### Summary Table 

| Isolation Level      | Dirty Read | Non-Repeatable Read | Phantom Read | Example Behavior                                                             |
| -------------------- | ---------- | ------------------- | ------------ | ---------------------------------------------------------------------------- |
| **Read Uncommitted** | Yes        | Yes                 | Yes          | Can read uncommitted data (dirty), and same row may show different values.   |
| **Read Committed**   | No         | Yes                 | Yes          | Cannot read dirty data, but row value may change between reads.              |
| **Repeatable Read**  | No         | No                  | Yes          | Same rows will return same data, but new rows can still "appear" (phantoms). |
| **Serializable**     | No         | No                  |  No          | Fully isolated – behaves like transactions are executed one at a time.       |

---

### Real-world Analogy (Bank Scenario)
Let’s say two users are accessing a shared bank account:
- **Read Uncommitted**: You see the account has $10,000. But the bank is still processing someone else’s withdrawal of $9,000 — you’re seeing **in-progress, possibly incorrect data**.
- **Read Committed**: You see only committed transactions. But if you refresh, the balance might change.
- **Repeatable Read**: The balance stays the same during your session, but someone might still create a **new transaction** that you don’t see initially.
- **Serializable**: You get a snapshot. No one else can modify or add data that would affect your view until you're done.
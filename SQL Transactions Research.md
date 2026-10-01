# Transactions in PostgreSQL

## Introduction

A transaction is one of the fundamental concepts of database systems. A transaction groups multiple database operations into a single unit of work that follows an **all-or-nothing** approach. This means that either all operations within the transaction are completed successfully, or none of them are applied to the database.

Transactions are essential for maintaining data consistency, integrity, and reliability, especially when multiple users are accessing and modifying data simultaneously.

---

# What is a Transaction?

A transaction is a sequence of one or more SQL statements executed as a single logical operation.

The main purpose of a transaction is to ensure that database changes occur completely or not at all. Intermediate changes made during a transaction are not visible to other users until the transaction is successfully completed.

### Example Scenario

Consider a banking system where money is transferred from Alice's account to Bob's account.

The process involves several operations:

1. Deduct money from Alice's account.
2. Update Alice's branch balance.
3. Add money to Bob's account.
4. Update Bob's branch balance.

If the system fails after deducting money from Alice but before crediting Bob, the database would become inconsistent.

Transactions prevent this problem by ensuring that all related operations are treated as a single unit.

---

# Atomicity

A transaction is said to be **atomic**.

Atomicity means:

* All operations succeed together.
* If any operation fails, all previous operations are reversed.
* Partial updates are not allowed.

From the perspective of other users, a transaction either happens completely or does not happen at all.

### Benefits of Atomicity

* Prevents inconsistent data.
* Protects against system failures.
* Maintains database integrity.
* Ensures reliable business operations.

---

# Durability

Another important property of transactions is **durability**.

Durability guarantees that once a transaction has been successfully completed and committed, its changes are permanently stored in the database.

Even if a system crash occurs immediately after the transaction completes, the committed changes will not be lost.

### Example

If a customer withdraws money from an ATM and the transaction is committed successfully, the withdrawal information remains permanently stored even if the system crashes afterward.

---

# Transaction Isolation

Transactions often run concurrently in multi-user environments.

Isolation ensures that:

* Transactions cannot see incomplete changes made by other transactions.
* Uncommitted data remains hidden from other users.
* Database consistency is maintained.

### Example

Suppose one transaction is calculating total branch balances while another transaction is transferring money between accounts.

Without isolation, the first transaction might see only part of the transfer process, resulting in incorrect totals.

PostgreSQL prevents this by ensuring that incomplete transaction changes remain invisible until the transaction is committed.

---

# Transaction Control Commands

PostgreSQL provides several commands for managing transactions.

## BEGIN

The `BEGIN` command starts a transaction block.

Example:

```sql id="9g2j1s"
BEGIN;
```

After this command, all subsequent SQL statements become part of the same transaction.

---

## COMMIT

The `COMMIT` command permanently saves all changes made during the transaction.

Example:

```sql id="mj6l0v"
COMMIT;
```

Once committed:

* Changes become permanent.
* Changes become visible to other users.
* The transaction is completed successfully.

---

## ROLLBACK

The `ROLLBACK` command cancels all changes made during a transaction.

Example:

```sql id="e5yqbh"
ROLLBACK;
```

When a rollback occurs:

* All updates are undone.
* The database returns to its previous state.
* No changes are saved.

---

# Transaction Example

A typical PostgreSQL transaction follows this structure:

```sql id="9u9gxp"
BEGIN;

UPDATE accounts
SET balance = balance - 100.00
WHERE name = 'Alice';

UPDATE accounts
SET balance = balance + 100.00
WHERE name = 'Bob';

COMMIT;
```

### Process

1. Transaction begins.
2. Alice's account is debited.
3. Bob's account is credited.
4. Changes are committed.
5. Updates become visible to other users.

If an error occurs before the `COMMIT`, a `ROLLBACK` can be performed to undo all changes.

---

# Implicit Transactions

PostgreSQL automatically executes every SQL statement within a transaction.

If no explicit transaction is started using `BEGIN`, PostgreSQL wraps each statement in its own transaction.

For example:

```sql id="1kax3l"
UPDATE employees
SET salary = salary + 1000;
```

Internally, PostgreSQL treats this as:

```sql id="e8mubg"
BEGIN;
UPDATE employees
SET salary = salary + 1000;
COMMIT;
```

This behavior ensures database consistency even when transaction blocks are not explicitly defined.

---

# Transaction Blocks

A group of SQL statements enclosed between `BEGIN` and `COMMIT` is known as a **transaction block**.

Example:

```sql id="4hwdl4"
BEGIN;

SQL Statement 1;
SQL Statement 2;
SQL Statement 3;

COMMIT;
```

Transaction blocks allow multiple related operations to be treated as a single unit of work.

---

# Savepoints

PostgreSQL provides **savepoints** for more granular transaction control.

A savepoint creates a checkpoint within a transaction.

If an error occurs later, the transaction can roll back to the savepoint instead of canceling the entire transaction.

---

## Creating a Savepoint

A savepoint is created using:

```sql id="rgr5mb"
SAVEPOINT my_savepoint;
```

---

## Rolling Back to a Savepoint

To undo changes made after a savepoint:

```sql id="dj8j0m"
ROLLBACK TO my_savepoint;
```

Only changes made after the savepoint are removed.

Changes made before the savepoint remain intact.

---

## Releasing a Savepoint

A savepoint can be removed using:

```sql id="rbajmt"
RELEASE SAVEPOINT my_savepoint;
```

This frees system resources and indicates that the savepoint is no longer needed.

---

# Savepoint Example

Consider a transaction where money is initially transferred to the wrong account.

```sql id="b7oeqk"
BEGIN;

UPDATE accounts
SET balance = balance - 100.00
WHERE name = 'Alice';

SAVEPOINT my_savepoint;

UPDATE accounts
SET balance = balance + 100.00
WHERE name = 'Bob';

ROLLBACK TO my_savepoint;

UPDATE accounts
SET balance = balance + 100.00
WHERE name = 'Wally';

COMMIT;
```

### Process

1. Alice's account is debited.
2. A savepoint is created.
3. Money is credited to Bob.
4. The mistake is discovered.
5. The transaction rolls back to the savepoint.
6. The credit to Bob is removed.
7. Money is credited to Wally instead.
8. The transaction is committed.

This allows partial corrections without restarting the entire transaction.

---

# Advantages of Savepoints

Savepoints provide several benefits:

* Allow partial rollbacks.
* Reduce the need to restart transactions.
* Improve error recovery.
* Increase flexibility during complex operations.
* Help manage large transaction blocks.

---

# Importance of Transactions

Transactions are critical in database systems because they:

* Maintain data consistency.
* Protect against failures.
* Ensure reliable data processing.
* Support concurrent user access.
* Prevent partial updates.
* Improve application reliability.

Common use cases include:

* Banking systems
* E-commerce applications
* Payroll systems
* Inventory management systems
* Reservation and booking systems

---

# Conclusion

Transactions are a fundamental feature of PostgreSQL that ensure database operations are executed safely and reliably. By using commands such as `BEGIN`, `COMMIT`, `ROLLBACK`, and `SAVEPOINT`, developers can control how changes are applied to the database. Transactions guarantee atomicity, durability, and isolation, helping maintain data integrity even when failures or concurrent operations occur. These capabilities make transactions essential for building secure, consistent, and reliable database applications.

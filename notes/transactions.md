---
title: Transactions
layout: note
---

# Transactions

A SQL transaction ensures that several insert/update/delete actions are committed all at once, and no other code that is reading the database sees these changes before they are completed. Also, if an error occurs in the middle of the process, they can all be reverted or "rolled back."

## SQL syntax for transactions

```sql
START TRANSACTION;
INSERT INTO ...
UPDATE ...
DELETE FROM ...
COMMIT;
```

## PHP syntax for transactions

First, turn off auto-commit, which would normally occur after each SQL statement:

```php
$conn->autocommit(false);
```

Now, perform start the transaction, perform some operations, and commit or roll back depending:

```
$conn->query("INSERT INTO ...");
if(...) {
  $conn->rollback();
} else {
  $conn->commit();
}
```


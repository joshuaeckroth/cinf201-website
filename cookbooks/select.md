---
title: Select tricks
layout: note
---

# Select tricks

These entries may be used when performing SELECT queries.

## Convert text to upper/lower case

This cookbook entry was contributed by Ou Zheng.

``` sql
-- convert to uppercase
SELECT UCASE(column) FROM ...
-- same as:
SELECT UPPER(column) FROM ...

-- convert to lowercase
SELECT LCASE(column) FROM ...
-- same as:
SELECT LOWER(column) FROM ...
```

## Compute the length of a column

This cookbook entry was contributed by Roberto Pagan.

```sql
-- use the LENGTH() function to find/limit by the length of a column
SELECT msg FROM messages WHERE LENGTH(msg) < 140;
```

## Copy data from one table to another

```sql
INSERT INTO table2 (col1, col2) SELECT col1, col2 FROM table1
```

## Between function

```sql
SELECT name FROM candy WHERE total_pieces_of_candy BETWEEN 100 and 300
SELECT name FROM candy WHERE total_pieces_of_candy NOT BETWEEN 100 and 300
```


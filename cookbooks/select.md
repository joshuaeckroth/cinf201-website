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


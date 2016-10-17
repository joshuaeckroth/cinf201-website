---
title: Views
layout: note
---

# Views

A SQL "view" is a predefined SELECT query. Since a SELECT query returns columns and rows, the result of a SELECT query can be thought of as a table of data. A view gives the SELECT query, and the corresponding table of data, a name just like a table name. You can then SELECT from the view.

## Motivation

Sometimes, SELECT queries can be very complicated and include a lot of carefully crafted logic. Whether we're typing these queries by handing or writing code, we do not want to retype/recode these queries over and over again because it's tedious and prone to mistakes. Instead, we can define a view that gives a name to the query and treats the query's results as a new table. After the view is made, we can then write SELECT queries against the view.

## Creating views

```
CREATE [OR REPLACE]
    VIEW view_name
    AS select_statement
```




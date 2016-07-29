---
title: Select
layout: note
---

# Select

- high
- level
- points

## Select grammar

~~~
SELECT
    [DISTINCT]
    select_expr [, select_expr ...]
    [FROM table_references]
    [WHERE where_condition]
    [ORDER BY col_name [ASC | DESC], ...]
    [LIMIT {[offset,] row_count | row_count OFFSET offset}]
~~~

Simplified from [MySQL docs](http://dev.mysql.com/doc/refman/5.7/en/select.html).

## Selecting fields

~~~ sql
select concat(firstname, ' ', lastname) as fullname
~~~



## Filtering with "where" clauses

## Ordering

~~~ sql
order by column
~~~

~~~ sql
order by column asc
~~~

~~~ sql
order by column desc
~~~

~~~ sql
order by 
~~~

## Limitting results

~~~ sql
limit 10
~~~

~~~ sql
limit 10,20
~~~


~~~ sql
limit 10 offset 50
~~~


---
title: Aggregation
layout: note
---

# Aggregation

Aggregation means combining or summarizing multiple rows into a single row. Up to this point, our queries always returned every individual row that met the query constraints. Aggregation allows us to combine rows and find the count, compute statistical summaries, or join text to create a single row.

## Counting

If you only need to count how many rows match some criteria, use the `COUNT()` aggregation method.

~~~ sql
SELECT COUNT(*) FROM ...
~~~

The above technique, `COUNT(*)`, returns the count of rows. If you want to count only those rows for which a particular column is not null, then apply count to that column:

~~~ sql
SELECT COUNT(col_name) FROM ...
~~~

You can also count just the distinct rows:

~~~ sql
SELECT COUNT(DISTINCT col_name) FROM ...
~~~

## Finding the sum, min, max, average

These are self-explanatory.

~~~ sql
SELECT SUM(col_name) FROM ...
SELECT SUM(DISTINCT col_name) FROM ...
SELECT MIN(col_name) FROM ...
SELECT MIN(DISTINCT col_name) FROM ...
SELECT MAX(col_name) FROM ...
SELECT MAX(DISTINCT col_name) FROM ...
SELECT AVG(col_name) FROM ...
SELECT AVG(DISTINCT col_name) FROM ...
~~~

## Grouping

Grouping allows us to aggregate multiple times in the same query. Rows can be grouped before finding the count/min/max/etc. according to a `GROUP BY` clause. Typically, you'll include the group by columns in your query so you can see the group value as well as the aggregated value.

~~~ sql
SELECT group_col_name, COUNT(agg_col_name) FROM ... WHERE ... GROUP BY group_col_name
~~~

For example, consider the shopping cart table:

| Customer | Product | Count |
|--|
| Josh  | Biscoff Family Pack                            |        1 |
| Josh  | Bayes Theorem Examples                         |        3 |
| Tracy | The Great Florida Birding Wildlife Trail Guide |        1 |
| Tracy | Canon EF-S 55-255mm Telephoto Zoom Lens        |        1 |

We can query for the total number of items in each customer's cart by grouping by the customer:

~~~ sql
SELECT customer, SUM(count) FROM shopping_cart GROUP BY customer
~~~

The result would be:

| Customer | SUM(count) |
|---|
| Josh | 4 |
| Tracy | 2 |

## Aggregation limitations

A query using aggregation can only select columns that are either aggregated with a function like count/min/max/etc. or included in a group by clause. Additional columns cannot be included, because it wouldn't be able to determine their value. For example, this does not work, because it's not clear what the value for 'product' should be:

~~~ sql
-- doesn't work!
SELECT customer, product, SUM(count) FROM shopping_cart GROUP BY customer
~~~

MySQL gives this error:

> ERROR 1055 (42000): Expression #2 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'shopping_cart.product' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by


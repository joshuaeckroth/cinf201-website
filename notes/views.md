---
title: Views
layout: note
---

# Views

A SQL "view" is a predefined SELECT query. Since a SELECT query returns columns and rows, the result of a SELECT query can be thought of as a table of data. A view gives the SELECT query, and the corresponding table of data, a name just like a table name. You can then SELECT from the view.

## Motivation

Sometimes, SELECT queries can be very complicated and include a lot of carefully crafted logic. This complexity is often due to normalization: recall that a normalized database design often has many tables, thus requiring that our queries involve multiple joins. Whether we're typing these queries by handing or writing code, we do not want to retype/recode these queries over and over again because it's tedious and prone to mistakes. Instead, we can define a view that gives a name to the query and treats the query's results as a new table. After the view is made, we can then write SELECT queries against the view.

## Creating views

```
CREATE [OR REPLACE]
    VIEW view_name
    AS select_statement
```

## Showing views

Get a list of all views in a given database:

~~~ sql
SHOW FULL TABLES IN [database name, e.g., cinf201] WHERE TABLE_TYPE LIKE 'VIEW';
~~~

Show the "create" definition for a particular view:

~~~ sql
show create view [your view name]\G
~~~

Note, you can use `\G` at the end of any query instead of `;` (which is the same as `\g`) to get row-by-row view, as shown above, rather than a table view.

## Examples

First, we'll create a view out of our JOIN query for product inventory. Recall that product titles were stored separately from inventory counts, so a join was necessary to bring the two bits of info back together.

~~~ sql
create view shop_product_inventory as
  select title, invcount from shop_products
  join shop_inventory on shop_products.id = shop_inventory.product_id;
~~~

We can treat the view as a normal table, and select against it:

~~~
mysql> select * from shop_product_inventory;
+------------------------------------------------+----------+
| title                                          | invcount |
+------------------------------------------------+----------+
| Biscoff Family Pack                            |        4 |
| Brother DR360 Drum Unit                        |        2 |
| Bayes Theorem Examples                         |        5 |
| The Great Florida Birding Wildlife Trail Guide |       10 |
| LIXIT Plastic Wide Mouth Water Bottle          |        6 |
| Canon EF-S 55-255mm Telephoto Zoom Lens        |        2 |
+------------------------------------------------+----------+
~~~

We can even use more interesting SELECT queries against the view:

~~~
mysql> select * from shop_product_inventory where invcount < 5 order by invcount;
+-----------------------------------------+----------+
| title                                   | invcount |
+-----------------------------------------+----------+
| Brother DR360 Drum Unit                 |        2 |
| Canon EF-S 55-255mm Telephoto Zoom Lens |        2 |
| Biscoff Family Pack                     |        4 |
+-----------------------------------------+----------+
~~~

The following query will show all the views we have created in this database:

```
mysql> SHOW FULL TABLES IN cinf201 WHERE TABLE_TYPE LIKE 'VIEW';
+------------------------+------------+
| Tables_in_cinf201      | Table_type |
+------------------------+------------+
| shop_product_inventory | VIEW       |
+------------------------+------------+
```

And this query will show us the definition of the `shop_product_inventory` view:

```
mysql> show create view shop_product_inventory\G
*************************** 1. row ***************************
                View: shop_product_inventory
         Create View: CREATE ALGORITHM=UNDEFINED DEFINER=`root`@`localhost` SQL SECURITY DEFINER
            VIEW `shop_product_inventory` AS
              select `shop_products`.`title` AS `title`,`shop_inventory`.`invcount` AS `invcount`
              from (`shop_products` join `shop_inventory`
              on((`shop_products`.`id` = `shop_inventory`.`product_id`)))
character_set_client: utf8
collation_connection: utf8_general_ci
1 row in set (0.00 sec)
```

For another example, let's take a complex IMDB query for listing female lead roles:

~~~ sql
create view female_lead_roles as
  select distinct title, character_name, fname, lname
  from movies m
  join acted_in ai on m.idmovies = ai.idmovies
  join actors on actors.idactors = ai.idactors
  where billing_position = 1 and gender = 0;
~~~

And here is an example of using the view:

```
mysql> select * from female_lead_roles where lname != '' limit 3;
+-------------------------+----------------+---------+---------+
| title                   | character_name | fname   | lname   |
+-------------------------+----------------+---------+---------+
| Tangents & the Times    | Marissa        | Marissa | A. Ross |
| Diary of a Clippers Fan | Susan          | Mariann | Aalda   |
| Pieni rakkaustarina     | Niina Lhde     | Elina   | Aalto   |
+-------------------------+----------------+---------+---------+
3 rows in set (26.81 sec)
```


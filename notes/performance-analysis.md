---
title: Performance analysis
layout: note
---

# Performance analysis

As database tables grow into hundreds-of-thousands or millions of rows, simple SELECT queries might begin to take significant time, and JOINs will take even longer. In many cases, we can recover high performance by making a small effort to add [indexes](/notes/indexes.html) and analyze our queries for any surprising behavior.

## Adding indexes

Most performance issues are the result of querying or joining on columns that do no have indexes. The rule of thumb is to add an index for any column that will be mentioned in a WHERE or JOIN ... ON clause. Refer to the [indexes](/notes/indexes.html) notes for details.

## EXPLAIN queries

The EXPLAIN feature shows how MySQL processes a SELECT query. It does not run the query, rather it shows some details about how it plans to run the query.

~~~
EXPLAIN SELECT ...
~~~

Here is a small example on the `cinf201_imdb_noindex` database:

~~~
mysql> explain select * from actors;
+----+-------------+--------+------------+------+---------------+------+---------+------+---------+----------+-------+
| id | select_type | table  | partitions | type | possible_keys | key  | key_len | ref  | rows    | filtered | Extra |
+----+-------------+--------+------------+------+---------------+------+---------+------+---------+----------+-------+
|  1 | SIMPLE      | actors | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 3754702 |   100.00 | NULL  |
+----+-------------+--------+------------+------+---------------+------+---------+------+---------+----------+-------+
~~~

Here is an explanation of the various columns:

- id: the select identifier (not interesting)
- select_type: this will be "SIMPLE" for all of our queries (most likely)
- table: the table that's being examined; when doing a JOIN, each row in the EXPLAIN output will refer to a different table
- partitions: ignore this column
- type: the JOIN type; see below for JOIN examples
- possible_keys: if a WHERE or JOIN ... ON clause is used in the query, this column shows the indexes (keys) that can be used to select the rows; if it shows NULL but you are using a WHERE or JOIN clause, then you probably want to add an index to speed up the query
- key: the actual key (index) used, among those listed in possible_keys
- key_len: when a key (index) is used, the length (in bytes) of that key; the length depends on the data type (e.g., INT will be 4 bytes)
- ref: the columns that are being referred to by the key
- rows: an estimate of the count of rows that will be extracted from the table; this is only an estimate because the query has not actually executed
- filtered: an estimate of the percent of rows that are filtered (left out) of the results due to the WHERE or JOIN ... ON clause
- extra: sometimes shows additional details like "Using filesort", meaning the results will be sorted because you have ORDER BY, and this sort will be slow (it could be made fast by creating an index on the ORDER BY column(s)).

### An example with JOINS

First, an example with no index, from the database `cinf201_imdb_noindexes`.

~~~
mysql> explain select fname, lname from actors join acted_in on acted_in.idactors = actors.idactors where billing_position = 1;
+----+-------------+----------+------------+------+---------------+------+---------+------+----------+----------+----------------------------------------------------+
| id | select_type | table    | partitions | type | possible_keys | key  | key_len | ref  | rows     | filtered | Extra                                              |
+----+-------------+----------+------------+------+---------------+------+---------+------+----------+----------+----------------------------------------------------+
|  1 | SIMPLE      | acted_in | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 26913770 |    10.00 | Using where                                        |
|  1 | SIMPLE      | actors   | NULL       | ALL  | NULL          | NULL | NULL    | NULL |  3750380 |    10.00 | Using where; Using join buffer (Block Nested Loop) |
+----+-------------+----------+------------+------+---------------+------+---------+------+----------+----------+----------------------------------------------------+
~~~

This result tells use that no keys (indexes) were used. Also, even though our query lists actors first in the SELECT, we're really selecting all rows from acted_in, and for each such row, grabbing the actor's first and last name. We should have as many rows for output as there are acted_in rows where billing position is 1. The EXPLAIN query shows that all rows of the actors table will be examined, which is not strictly necessary.

Compare with the output from indexed tables (using database `cinf201_imdb`):

~~~
mysql> explain select fname, lname from actors join acted_in on acted_in.idactors = actors.idactors where billing_position = 1;
+----+-------------+----------+------------+--------+-------------------------+---------+---------+--------------------------------+----------+----------+-------------+
| id | select_type | table    | partitions | type   | possible_keys           | key     | key_len | ref                            | rows     | filtered | Extra       |
+----+-------------+----------+------------+--------+-------------------------+---------+---------+--------------------------------+----------+----------+-------------+
|  1 | SIMPLE      | acted_in | NULL       | ALL    | ACTED_IN_IDACTORS_INDEX | NULL    | NULL    | NULL                           | 24924220 |    10.00 | Using where |
|  1 | SIMPLE      | actors   | NULL       | eq_ref | PRIMARY                 | PRIMARY | 4       | cinf201_imdb.acted_in.idactors |        1 |   100.00 | NULL        |
+----+-------------+----------+------------+--------+-------------------------+---------+---------+--------------------------------+----------+----------+-------------+
~~~

We see now that the primary key (idactors) in the actors table is used to join with the acted_in table, so for each acted_in row, exactly one actors row is retrieved (rather than scanning them all).

In this last EXPLAIN query, the acted_in table didn't have an index on the billing_position column. Thus, the EXPLAIN query shows "Using where" in the Extra column. We can get faster query execution by addding an index on billing_position. Notice the resulting EXPLAIN query now uses a key for the acted_in table as well:

~~~
mysql> explain select fname, lname from actors join acted_in on acted_in.idactors = actors.idactors where billing_position = 1;
+----+-------------+----------+------------+--------+------------------------------------------+------------------+---------+--------------------------------+---------+----------+-------+
| id | select_type | table    | partitions | type   | possible_keys                            | key              | key_len | ref                            | rows    | filtered | Extra |
+----+-------------+----------+------------+--------+------------------------------------------+------------------+---------+--------------------------------+---------+----------+-------+
|  1 | SIMPLE      | acted_in | NULL       | ref    | ACTED_IN_IDACTORS_INDEX,billing_position | billing_position | 5       | const                          | 2518866 |   100.00 | NULL  |
|  1 | SIMPLE      | actors   | NULL       | eq_ref | PRIMARY                                  | PRIMARY          | 4       | cinf201_imdb.acted_in.idactors |       1 |   100.00 | NULL  |
+----+-------------+----------+------------+--------+------------------------------------------+------------------+---------+--------------------------------+---------+----------+-------+
~~~




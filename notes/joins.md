---
title: Joins
layout: note
---

# Joins

Usually, data is stored in relational databases in multiple tables. There are two reasons for this: (1) data duplication is reduced and (2) complex queries can combine data from various tables in new ways that were not necessarily planned in advance. In order to build some queries, we must "join" data from multiple tables.

## Join logic

Much of the SQL language is inspired by mathematical set theory. A SELECT query is how we describe the content of a set. So far, we have only described a set of results from a single table. With joins, we can describe unions and intersections of sets of results from multiple tables.

The diagram below is commonly found on the internet when searching for "sql joins" (from [here](http://www.codeproject.com/Articles/33052/Visual-Representation-of-SQL-Joins)). It's a quick summary of the various kinds of joins that MySQL supports (I removed one kind of join MySQL doesn't support: full outer joins).

![SQL Joins](/images/joins.jpg)

Joins use the following syntax:

~~~
SELECT
    select_expr
    FROM [table1]
    (JOIN | LEFT JOIN | RIGHT JOIN) [table2]
    [ON match_condition]
    ... possibly more joins
    [WHERE where_condition]
    ... etc. just like normal SELECT
~~~

Note that the `ON match_condition` constrains the join and is performed before the `WHERE`. A temporary new table is constructed out of the data from the joined tables, and it is this temporary table that is subjected to the `WHERE` clause. This means that it is more efficient to put your constraints in an `ON` condition if possible, rather than a `WHERE` condition.

## Inner joins

Inner joins include only those rows where the `ON match_condition` is met from both tables. It is the default join type, and most common, so you can specify just the word `JOIN` (but, if you choose, you can write `INNER JOIN` also).

## Left joins

Left joins may return rows from the "left" (first) table even in cases where the `ON match_condition` is not met. In other words, only the right table will be subjected to the match condition.

## Right joins

A right join is just like a left join except only the left (first) table is subjected to the match conditions. If you reverse the order that the tables are mentioned in the query, you can change a right join into an equivalent left join and vice versa.

## Example: Shopping

Here are some examples from class with the various `shop` tables in the `cinf201` database.



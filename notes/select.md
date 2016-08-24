---
title: Select
layout: note
---

# Select

The SQL SELECT command will be our most used SQL command. It has more capabilities than what is listed in the following notes (e.g., see notes about [Joins](/notes/joins.html)), but it is important to become comfortable with simpler SELECT queries first.

## Select grammar

The simple use cases of SELECT can be defined by the following "grammar". A grammar shows the required syntax (words and symbols) without brackets, such as `SELECT` and `select_expr`, and optional syntax inside brackets. Upper-case words like `SELECT` and `FROM` must be typed as-is (though they can be lower-case), while lower-case words like `select_expr` and `table_references` are stand-ins for whatever is appropriate for your particular needs. For example, `table_references` will be whichever table(s) you are querying.

~~~
SELECT
    [DISTINCT]
    select_expr [, select_expr ...]
    [FROM table_references]
    [WHERE where_condition]
    [ORDER BY col_name [ASC | DESC], ...]
    [LIMIT row_count]
~~~

Simplified from [MySQL docs](http://dev.mysql.com/doc/refman/5.7/en/select.html).

## Selecting fields

~~~ sql
select field1, field2 from mytable;
~~~

You can rename the fields:

~~~ sql
select field1 as myname, field2 as othername from mytable;
~~~

You can concetenate (combine) two fields with a seperator:

~~~ sql
select concat(firstname, ' ', lastname) as fullname from mytable;
~~~

If you want only unique ("distinct") values in the output, use the DISTINCT keyword:

~~~ sql
select distinct field1, field2 from mytable;
~~~

## Filtering with "where" clauses

A "where" clause allows you to select only a subset of all rows in a table. The clause should contain true/false ("Boolean") expressions and combined with AND/OR if you have multiple constraints:

~~~
select ... from ... where field1 > 0;
select ... from ... where field1 != 'value';
select ... from ... where field1 != 'value' and field2 = 37;
select ... from ... where field1 <= 10 and field2 >= -7;
select ... from ... where field1 is null;
select ... from ... where field1 is not null;
~~~

Note the last two examples. A particular field in a particular row may have no value (not zero, not blank, ...), which we call `NULL`.

Columns containing text can be searched using the LIKE keyword. Use a percent-sign `%` before/after your search phrase:

~~~
select ... from ... where field1 like '%search phrase%';
~~~

## Ordering

The selected rows can be sorted according to one or more fields. The sorting (or "ordering") can be ascendending, typed `asc` (which is the default), meaning early values come first, or descending, typed `desc`.

~~~ sql
select ... from ... order by field1;
select ... from ... order by field1 asc;
select ... from ... order by field1 desc
select ... from ... order by field1 asc, field2 desc;
~~~

## Limitting results

In almost all selects, you'll probably want to limit the output so that you don't get a dump of hundreds-of-thousands of rows.

~~~ sql
select ... from ... limit 10;
~~~

## Examples

Here are some examples of SELECT queries for the MySQL database on londo. Remember to run `use cinf201;` first if you plan to try these queries.

~~~
mysql> select gamedate from nflpbp limit 10;
+------------+
| gamedate   |
+------------+
| 2013-09-09 |
| 2013-09-29 |
| 2013-11-10 |
| 2013-09-15 |
| 2013-09-15 |
| 2013-11-11 |
| 2013-11-24 |
| 2013-09-15 |
| 2013-12-22 |
| 2013-12-12 |
+------------+
10 rows in set (0.00 sec)

mysql> select distinct playtype from nflpbp where playtype != '' order by playtype;
+----------------------+
| playtype             |
+----------------------+
| CLOCK STOP           |
| EXCEPTION            |
| EXTRA POINT          |
| FIELD GOAL           |
| FUMBLES              |
| KICK OFF             |
| NO PLAY              |
| PASS                 |
| PENALTY              |
| PUNT                 |
| QB KNEEL             |
| RUSH                 |
| SACK                 |
| SCRAMBLE             |
| TIMEOUT              |
| TWO-POINT CONVERSION |
+----------------------+
16 rows in set (0.19 sec)

mysql> select gamedate, offenseteam, defenseteam, description from nflpbp where description like '%kneels%' limit 20;
+------------+-------------+-------------+----------------------------------------------------------+
| gamedate   | offenseteam | defenseteam | description                                              |
+------------+-------------+-------------+----------------------------------------------------------+
| 2013-09-15 | WAS         | GB          | (:09) 10-R.GRIFFIN KNEELS TO WAS 19 FOR -1 YARDS.        |
| 2013-09-29 | DET         | CHI         | (:38) 9-M.STAFFORD KNEELS TO DET 49 FOR NO GAIN.         |
| 2013-09-29 | DET         | CHI         | (:35) 9-M.STAFFORD KNEELS TO DET 48 FOR -1 YARDS.        |
| 2013-09-29 | TEN         | NYJ         | (1:12) 4-R.FITZPATRICK KNEELS TO TEN 28 FOR -1 YARDS.    |
| 2013-09-29 | TEN         | NYJ         | (:38) 4-R.FITZPATRICK KNEELS TO TEN 27 FOR -1 YARDS.     |
| 2013-09-29 | NE          | ATL         | (:35) 12-T.BRADY KNEELS TO NE 9 FOR -1 YARDS.            |
| 2013-09-29 | NE          | ATL         | (:28) 12-T.BRADY KNEELS TO NE 8 FOR -1 YARDS.            |
| 2013-10-27 | DEN         | WAS         | (1:14) 18-P.MANNING KNEELS TO WAS 32 FOR NO GAIN.        |
| 2013-10-27 | DEN         | WAS         | (:39) 18-P.MANNING KNEELS TO WAS 33 FOR -1 YARDS.        |
| 2013-11-17 | PIT         | DET         | (1:39) 7-B.ROETHLISBERGER KNEELS TO DET 23 FOR -1 YARDS. |
| 2013-11-17 | PIT         | DET         | (1:01) 7-B.ROETHLISBERGER KNEELS TO DET 24 FOR -1 YARDS. |
| 2013-11-17 | PIT         | DET         | (:36) 7-B.ROETHLISBERGER KNEELS TO DET 25 FOR -1 YARDS.  |
| 2013-11-24 | TB          | DET         | (:44) 8-M.GLENNON KNEELS TO TB 2 FOR -7 YARDS.           |
| 2013-11-28 | DAL         | OAK         | (:35) 9-T.ROMO KNEELS TO OAK 43 FOR -1 YARDS.            |
| 2013-11-28 | BAL         | PIT         | (:38) 5-J.FLACCO KNEELS TO PIT 38 FOR -1 YARDS.          |
| 2013-12-08 | CIN         | IND         | (:20) 14-A.DALTON KNEELS TO CIN 18 FOR -1 YARDS.         |
| 2013-12-22 | IND         | KC          | (:02) 12-A.LUCK KNEELS TO IND 36 FOR -1 YARDS.           |
| 2013-12-22 | NYJ         | CLE         | (1:01) 7-G.SMITH KNEELS TO CLE 48 FOR NO GAIN.           |
| 2013-12-22 | NYJ         | CLE         | (:33) 7-G.SMITH KNEELS TO CLE 49 FOR -1 YARDS.           |
| 2013-12-22 | ARI         | SEA         | (:17) 3-C.PALMER KNEELS TO ARI 19 FOR -1 YARDS.          |
+------------+-------------+-------------+----------------------------------------------------------+
20 rows in set (0.00 sec)
~~~


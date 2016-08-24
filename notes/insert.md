---
title: Insert
layout: note
---

# Insert

The SQL INSERT command adds new rows to a table.

## Insert grammar

The grammar for INSERT specifies that a list of column names (field names) may or may not be specified. If not specified, the database has no way of knowing which values provided correspond to which fields --- thus, if fields are not listed before the values, then the value for every field for the new row must be specified. If, instead, a list of fields is specified, then only those values (in the same order) should be given.

Also, it is worth noting that more than one field may be inserted with a singe command by providing another set of values, each set in parentheses and separated by commas. See the examples below.

~~~
INSERT
    [INTO] tbl_name
    [(col_name,...)]
    {VALUES | VALUE} ({expr | DEFAULT},...),(...),...
~~~

Simplified from [MySQL docs](http://dev.mysql.com/doc/refman/5.7/en/insert.html).

## Inserting full rows

Suppose the `students` table has four columns: first name, lastname, age, and gpa.

Insert a single row (all fields):

~~~
insert into students values ('maria', 'franklin', 20, 3.5);
~~~

Insert multiple rows (all fields):

~~~
insert into students values ('maria', 'franklin', 20, 3.5), ('seth', 'burkshire', 18, 3.25);
~~~

## Inserting partial rows

If you want to insert a subset of the fields, you must specify the fields you want to insert:

~~~
insert into students (firstname, lastname) values ('maria', 'franklin');
insert into students (firstname, lastname) values ('maria', 'franklin'), ('seth', 'burkshire');
~~~

The unspecified fields will have their default values, or NULL if no default values are specified when the table is created. If a field cannot be NULL and has no default value, then the insert must specify a value for that field or the insert will fail.


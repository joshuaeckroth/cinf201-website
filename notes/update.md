---
title: Update
layout: note
---

# Update

The SQL UPDATE command modifies an existing row (or rows) in a table.

## Update grammar

The grammar for UPDATE is a little odd at first glance. An UPDATE statement must indicate as least one new value for one field, and possibly values for more fields. But the statement does not need to indicate which row gets the update. Without any such indication, all rows in the table get the update. So you must think carefully before executing an UPDATE statement to be sure that you have properly limited the rows that will receive the update. This limitation is done with the WHERE clause, which works just like a WHERE clause in a [SELECT](/notes/select.html) statement.

Note that the value specified by `expr1` can be a complicated expression or a single value. The expression can also refer to other fields in the table. See the examples below.

~~~
UPDATE
    table_reference
    SET col_name1={expr1|DEFAULT} [, col_name2={expr2|DEFAULT}] ...
    [WHERE where_condition]
~~~

Simplified from [MySQL docs](http://dev.mysql.com/doc/refman/5.7/en/update.html).

## Examples

Suppose the `students` table has four columns: first name, lastname, age, and gpa.

Update all rows:

~~~
update students set age = 22, gpa = 3.0;
~~~

Update certain rows matching the WHERE clause:

~~~
update students set age = 22, gpa = 3.0 where firstname = 'maria' and lastname = 'franklin';
~~~

Add 0.1 pts to every gpa:

~~~
update students set gpa = gpa + 0.1;
~~~



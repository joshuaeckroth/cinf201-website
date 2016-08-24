---
title: Delete
layout: note
---

# Delete

The SQL DELETE command deletes rows from a table.

## Delete grammar

The DELETE command is very straightforward. All it needs is a WHERE clause like we have in [SELECT](/notes/select.html) and [UPDATE](/notes/update.html). Naturally, be careful with the delete command! There is no "undo" operation apart from restoring from backups.

~~~
DELETE
    FROM tbl_name
    [WHERE where_condition]
~~~

## Examples

~~~
delete from students where firstname = 'maria' and lastname = 'franklin';
delete from students where gpa < 2.0;
~~~


---
title: Indexes
layout: note
---

# Indexes

Whenever a query includes a WHERE clause or JOIN ... ON condition, MySQL has to determine the subset of rows that meet the given criteria. Without an index, MySQL does this by scanning the entire table, top-to-bottom, to find the matching rows. This can be extremely slow. By adding an index on the columns that define the WHERE or ON criteria, MySQL can much more quickly identify matching rows. In computer science terms, the slow non-index approach is "linear time" (i.e., slow) while the indexed approach is either "logarithmic time" or "constant time" (i.e., much faster).

## Showing, adding, removing indexes

~~~
SHOW INDEXES FROM tbl_name
ALTER TABLE tbl_name ADD PRIMARY KEY (col_name, ...)
ALTER TABLE tbl_name ADD UNIQUE (col_name, ...)
ALTER TABLE tbl_name ADD INDEX (col_name, ...)
ALTER TABLE tbl_name ADD FOREIGN KEY (col_name, ...) REFERENCES tbl_name (col_name, ...)
ALTER TABLE tbl_name DROP PRIMARY KEY
ALTER TABLE tbl_name DROP INDEX index_name
ALTER TABLE tbl_name DROP FOREIGN KEY index_name
~~~

### "Show" output

- non unique: whether or not the index includes duplicates (1) or all values are unique (0); note that primary key and unique indexes are only allowed on unique columns (where non unique = 0).
- key name: name of the index; use this for dropping
- column name: column or columns that are indexed
- cardinality: number of distinct values in the index

### Extra features for foreign keys

Foreign keys add constraints: you cannot add a row if the foreign key column does not point to a proper row in the other table, and you cannot remove a row in the other table if it is being pointed to via a foreign key.

However, MySQL supports "cascading" deletes so that if you delete a row pointing to a foreign key or a row being pointed at, the delete will propagate or cascade to the other table and delete all pointed-at/pointed-to rows as well. This ensures the constraint is still met.

~~~
ALTER TABLE tbl_name ADD FOREIGN KEY (col_name, ...) REFERENCES tbl_name (col_name, ...)
  ON DELETE CASCADE
~~~

You have other options as well:

~~~
  ON DELETE SET NULL
  ON DELETE RESTRICT -- same as normal case
~~~


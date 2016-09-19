---
title: DDL
layout: note
---

# Data definition language (DDL)

The language known as "SQL" usually refers to the various CRUD operations: select, insert, update, delete. A separate language known as "DDL" allows us to create tables and relations between tables.

## Creating tables

Here is the grammar for creating a table:

~~~
CREATE TABLE [IF NOT EXISTS] tbl_name
    (create_definition,...)
    [table_options]

create_definition:
    col_name column_definition

column_definition:
    data_type [NOT NULL | NULL] [DEFAULT default_value]
      [AUTO_INCREMENT] [UNIQUE | PRIMARY KEY]

data_type:
    BIT[(length)]
  | INT
  | FLOAT
  | DECIMAL[(length[,decimals])]
  | DATE
  | TIME
  | TIMESTAMP
  | YEAR
  | VARCHAR(length)
  | TEXT
~~~

Simplified from [MySQL docs](http://dev.mysql.com/doc/refman/5.7/en/create-table.html).

The grammar shows that the "create definition" (i.e., column names and types) must be in parentheses, while optional "table options" may follow.

### Column types

Numeric types:

- `BIT` for one bit or `BIT(M)` for M bits, each with value 0 or 1; M cannot be larger than 64
- `INT`: -2bil to +2bil integer range 
- `FLOAT`: inexact decimal values for large or tiny decimal values
- `DECIMAL`: exact decimal values, e.g., for storing money amounts

Date/time types:

- `DATE`: just dates, no times; values written as 'YYYY-MM-DD'
- `TIME`: just times; values written as 'HHH:MM:SS'
- `TIMESTAMP`: dates with times, always stored internally in UTC time zone; values written as 'YYYY-MM-DD HH:MM:SS'
- `YEAR`: just the year; values written as 'YYYY'

String types:

- `VARCHAR(length)`: a sequence of characters (a "string") with a specified maximum length
- `TEXT`: for larger string objects, too big for varchars; stores up to 65k characters; if you need more than than, you can use MEDIUMTEXT (16mil characters) or LONGTEXT (4bil characters).

### Column attributes

Besides the column name and type, a column can have additional attributes:

- `NOT NULL`: this column cannot have a NULL value
- `NULL`: (default) this column can have a NULL value
- `DEFAULT xyz`: the default value for this column if no value is provided (would otherwise be NULL if allowed)
- `AUTO_INCREMENT`: must only be used on primary keys (of integer type); auto increments the key value on new row insertions
- `UNIQUE`: every row in the table must have a different value for this column; this can also be specified in the "table options" section
- `PRIMARY KEY`: this row is the primary key for the table; this can also be specified in the "table options" section

### Table options

- `PRIMARY KEY (column_name)`: set which column is the primary key
- `UNIQUE (column_name(s))`: set which column(s) must be unique
- `FOREIGN KEY (column_name) REFERENCES table_name (column_name)`: establish a foreign key; see [table design](/notes/table-design.html) notes

## Deleting tables

Tables may deleted with the "drop" command:

``` sql
DROP TABLE [IF EXISTS] tbl_name [, tbl_name] ...
```

One or multiple table names may be provided.

## Altering tables

```
ALTER TABLE tbl_name
    alter_specification, ...

alter_specification:
   ADD COLUMN col_name col_definition
 | ADD INDEX index_name (col_name, ...)
 | ADD PRIMARY KEY (col_name, ...)
 | ADD UNIQUE (col_name, ...)
 | ADD FOREIGN KEY fk_index_name (col_name, ...) REFERENCES tbl_name (col_name, ...)
 | CHANGE old_col_name new_col_name col_definition
 | MODIFY col_name col_definition
 | DROP col_name
 | DROP PRIMARY KEY
 | DROP INDEX index_name
 | DROP FOREIGN KEY fk_index_name
 | RENAME new_tbl_name
```







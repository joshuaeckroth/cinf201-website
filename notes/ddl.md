---
title: DDL
layout: note
---

# Data definition language (DDL)

## Creating databases

## Creating tables

~~~
CREATE TABLE [IF NOT EXISTS] tbl_name
    (create_definition,...)
    [table_options]

create_definition:
    col_name column_definition

column_definition:
    data_type [NOT NULL | NULL] [DEFAULT default_value]
      [AUTO_INCREMENT] [UNIQUE [KEY] | [PRIMARY] KEY]

data_type:
    BIT[(length)]
  | INT[(length)]
  | REAL[(length,decimals)]
  | DECIMAL[(length[,decimals])]
  | DATE
  | TIME[(fsp)]
  | DATETIME[(fsp)]
  | YEAR
  | CHAR[(length)]
  | VARCHAR(length)
  | TEXT
~~~

Simplified from [MySQL docs](http://dev.mysql.com/doc/refman/5.7/en/create-table.html).

### Date/time data types

> The DATE type is used for values with a date part but no time
part. MySQL retrieves and displays DATE values in `YYYY-MM-DD`
format. The supported range is `1000-01-01` to `9999-12-31`.
>
> The DATETIME type is used for values that contain both date and time parts. MySQL retrieves and displays DATETIME values in `YYYY-MM-DD HH:MM:SS` format. The supported range is `1000-01-01 00:00:00` to `9999-12-31 23:59:59`.
>
> MySQL retrieves and displays TIME values in `HH:MM:SS` format (or `HHH:MM:SS` format for large hours values). TIME values may range from `-838:59:59` to `838:59:59`. The hours part may be so large because the TIME type can be used not only to represent a time of day (which must be less than 24 hours), but also elapsed time or a time interval between two events (which may be much greater than 24 hours, or even negative).
>
> Be careful about assigning abbreviated values to a TIME column. MySQL interprets abbreviated TIME values with colons as time of the day. That is, `11:12` means `11:12:00`, not `00:11:12`.

Quoted from [MySQL docs](http://dev.mysql.com/doc/refman/5.7/en/date-and-time-types.html).



## Altering tables


## Adding constraints






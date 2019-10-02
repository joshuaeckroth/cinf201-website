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

## Example

```
SET foreign_key_checks = 0;
drop table if exists restaurants;
drop table if exists employees;
drop table if exists products;
drop table if exists managers;
drop table if exists restaurants_managers;
drop table if exists menu;
drop table if exists orders;
drop table if exists orderitem;
drop table if exists inventory;
SET foreign_key_checks = 1;


create table employees (
	id int not null primary key auto_increment,
	name varchar(255) not null,
	dob date not null,
	title varchar(255) not null,
	hourly_rate decimal(3,2) not null,
	restaurant_id int not null
);

create table products (
	id int not null primary key auto_increment,
	name varchar(255) not null,
	category enum('entree', 'drink', 'sides', 'dessert') not null
);

create table restaurants (
	id int not null primary key auto_increment,
	city varchar(255) not null,
	state varchar(2) not null,
	postal varchar(5) not null,
	street_address varchar(255) not null
);

create table managers (
	id int not null primary key auto_increment,
	name varchar(255) not null,
	dob date not null,
	title varchar(255) not null,
	salary decimal(12,2) not null
);

create table restaurants_managers (
	restaurant_id int not null,
	manager_id int not null
);

create table menu (
	id int not null primary key auto_increment,
	price decimal(4,2) not null,
	product_id int not null,
	restaurant_id int not null
);

create table orders (
	id int not null primary key auto_increment,
	time_of_order timestamp not null default current_timestamp,
	total_cost decimal(10,2) not null,
	restaurant_id int not null
);

create table orderitem (
	id int not null primary key auto_increment,
	cnt int not null,
	menu_id int not null
);

create table inventory (
	id int not null primary key auto_increment,
	name varchar(255) not null,
	vendor varchar(255) not null,
	cnt int not null,
	cost decimal(12,2) not null,
	restaurant_id int not null
);

alter table employees add foreign key (restaurant_id) references restaurants (id);

alter table restaurants_managers add foreign key (restaurant_id) references restaurants (id);
alter table restaurants_managers add foreign key (manager_id) references managers (id);

alter table inventory add foreign key (restaurant_id) references restaurants (id);

alter table menu add foreign key (restaurant_id) references restaurants (id);
alter table menu add foreign key (product_id) references products (id);

alter table orderitem add foreign key (menu_id) references menu (id);

alter table orders add foreign key (restaurant_id) references restaurants (id);
```





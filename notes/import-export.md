---
title: Import/export
layout: note
---

# Import/export

Database systems typically support data import and export. Data import is useful when the data already exist in some external system or files, but needs to be imported into a different database. Export is handy for backups and transferring data to another database. 

## Import/export in SQL format

The simplest format to import/export data for MySQL is using the database's own SQL language. Supposing you have a SQL file full of table creation and data insert commands, "importing" the data is as simple as executing the SQL code:

```
mysql < myfile.sql
```

In order to create such a SQL file from an existing database, you can use the `mysqldump` command from the Linux command line (not from the mysql prompt):

```
mysqldump cinf201_jeckroth > myfile.sql
```

If you only want the tables but no data in the tables, use the `-d` option:

```
mysqldump -d cinf201_jeckroth > myfile-only-tables.sql
```

## Import from CSV

When we need to import data from a non-SQL source, such as a spreadsheet, we can use the CSV (comma-separated values) format. A CSV file looks like this:

```
colname1,colname,colname3
35.2,0.0,foo
6.1,2.2,"bar, baz"
```

In other words, the first row contains column names, and the following rows contain values for each column. Columns are separated by commas. If a value has commas, like "bar, baz", the value will be surrounded in quotes. Sometimes the first row, containing column names, is missing. The data types (e.g., INT vs. DOUBLE vs. DATETIME, etc.) must be inferred or known ahead of time; the CSV file does not tell you anything about data types.

In order to import a CSV file, you must first create a table. Then, use the `LOAD DATA` command:

```
LOAD DATA INFILE '/tmp/myfile.csv'
    [REPLACE | IGNORE]
    INTO TABLE tbl_name
    [{FIELDS | COLUMNS}
        [TERMINATED BY 'string']
        [[OPTIONALLY] ENCLOSED BY 'char']
    ]
    [IGNORE number LINES]
    [(col_name,...)]
    [SET col_name = expr,...]
```

The LOAD DATA command supports different kinds of files, not just CSV. But if you wish to import CSV data, indicate `FIELDS TERMINATED BY ','` and, if you expect any fields to have commas (like a "Lastname, Firstname" column), be sure to quote the field in the CSV file and add `OPTIONALLY ENCLOSED BY '"'`, e.g.,

```
LOAD DATA INFILE '/tmp/myfile.csv' INTO TABLE mytable FIELDS DELIMITED BY ',' OPTIONALLY ENCLOSED BY '"';
```

Also note that the MySQL server must be able to read your file. On londo, normal file permissions are insufficient for these purposes. Thus, we'll use the following strategies:

- Copy your CSV file to the `/tmp` directory; give your file a funny name so it does not conflict with anyone else's. E.g., use this copy command: `cp myfile.csv /tmp/jeckroth-myfile.csv`.
- Next, fix the permissions on the file so MySQL can read it: `chmod a+r /tmp/jeckroth-myfile.csv`. (FYI: `a+r` means "all can read").

The additional elements of the `LOAD DATA` command can be used for the following purposes:

- `REPLACE`: 
- `IGNORE number LINES`: skip a header line in the CSV file with this feature.
- `(col_name, …)`: indicate which columns should be filled from the CSV data; if not specified, all columns will be filled in the order specified when the table was created.
- `SET col_name = expr, …`: for each row, set some other columns; the `expr` value may refer to other columns on the same row.

## Export to CSV

```
SELECT select_statement...
    INTO OUTFILE '/tmp/myfile.csv'
    FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '"'
```

Due to the same file permission issues on londo, be sure to save the outfile in `/tmp`. Then copy it to your own directory.
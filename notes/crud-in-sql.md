---
title: CRUD in SQL
layout: note
---

# CRUD in SQL

CRUD is an acronym for Create, Read, Update, and Delete. These are the most common operations that a database must perform. SQL is an acronym for Structured Query Language, the language we will use for CRUD operations (until we learn about NoSQL databases).

SQL has been standardized and most popular databases support the same set of SQL commands. These popular databases include Oracle Database, Microsoft SQL Server, MySQL (the one we'll use), PostgreSQL, etc.

## Create

"Create" means to add or insert a record in the database. Typically, a record contains multiple values for various fields, e.g., a person's first name, last name, phone number, etc. may all make up a single record. The SQL language supports the Create operation with the [INSERT](/notes/insert.html) command.

## Read

"Read" means to retrieve a record or multiple records from the database. Typically, some kind of identifier is used to obtain a specific record, or some kind of constraints are defined in order to retrieve multiple records that meet those constraints. The SQL language supports the Read operation with the [SELECT](/notes/select.html) command.

## Update

"Update" means to change some values in an existing record or many records. The SQL language supports the Update operation with the [UPDATE](/notes/update.html) command.

## Delete

"Delete" means to delete a record, or multiple records, from the database. The SQL language supports the Delete operation with the [DELETE](/notes/delete.html) command.

## Some useful MySQL commands

In order to know how to use CRUD operations, you may need to know which tables exist and which fields are defined in those tables. The following MySQL commands perform these operations.

First, switch to the database you plan to use:

~~~
use cinf201;
~~~

Show all tables:

~~~
show tables;
~~~

Show fields for a particular table, e.g, the `nflpbp` table:

~~~
describe nflpbp;
~~~


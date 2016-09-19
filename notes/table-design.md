---
title: Table design and normalization
layout: note
---

# Table design and normalization

Relational databases (those that use SQL) are widely used because they support compact data storage and allow the creation of new and interesting SELECT queries (often with JOINs) that were not necessarily designed ahead of time. To gain these benefits, we must design our tables so that we reduce (eliminate) data redundancy and support complex joins. The three rules of normalization will ensure we meet these design criteria.

## Normal forms

"Normalization" of a database means to reconstruct the database tables so that certain constraints are met and the result is "normal" or canonical. There are three common rules of normalization, detailed below. The benefits of each form are explained after an example of a table that violates the form.

### First normal form (1NF)

> Every column on every row contains only a single, indivisible value.

The first normal form is somewhat obvious but still important to state. It means that you should not put two values in the same column for any row. For example, the products in a customer's shopping cart cannot be stored in the shopping cart table since no column should have multiple values:

Violates 1NF:

| Customer | Product and count |
|--|
| Josh  | Biscoff Family Pack (1), Bayes Theorem Examples (3) |
| Tracy | The Great Florida Birding Wildlife Trail Guide (1), Canon EF-S 55-255mm Telephoto Zoom Lens (1) |

Satisfies 1NF:

| Customer | Product | Count |
|--|
| Josh  | Biscoff Family Pack                            |        1 |
| Josh  | Bayes Theorem Examples                         |        3 |
| Tracy | The Great Florida Birding Wildlife Trail Guide |        1 |
| Tracy | Canon EF-S 55-255mm Telephoto Zoom Lens        |        1 |

Now, the pair of columns (Customer, Product) acts as the key for the table, and the Count column depends on that key (Count is a "fact" about a (Customer, Product) pair). Often, 1NF requires that we introduce one or more columns to act as the key for the table. In practice, actually, we almost always have a special key column, often called "id" or similar.

The benefit of 1NF is that we can be sure that selecting a value from a column only gives us one single value. Without this guarantee, we might have to extract bits of data out of a column value like extracting the product count from "Biscoff Family Pack (1)". It's not impossible to do so, but we are losing some of the benefits of a database. Ideally, the database handles all of the data for us, and allows us to query it, update it, compare values from one column to another column, etc. without any additional work.

### Second normal form (2NF)

To define 2NF, we first need to define "candidate key":

> A candidate key of a table is a group of columns that is unique across all rows of the table.

A candidate key may be one or more columns. A candidate key may be used to identify any individual row in the table. For example, in the table below, one candidate key is (Manufacturer, Product) since that pair of columns can uniquely identify every row in the table (another candidate key is (Website, Product)).

The rule of 2NF is as follows:

> The table is in 1NF and furthermore no column that is not a key column depends on any subset of a candidate key.

The Price column depends on the candidate key and no subset (i.e., just Manufacturer or just Product) is sufficient to determine the price. But the column Website depends on a subset of the candidate key (just Manufacturer), so this table violates 2NF.

| Manufacturer | Website | Product | Price |
|---|
| Biscoff | biscoff.com | Family Pack                            | 31.93 |
| Biscoff | biscoff.com | Singles Pack                            | 6.60 |
| Brother | brother-usa.com | DR360 Drum Unit                        | 59.87 |
| V4INK | v4ink.com | DR360 Drum Unit                        | 18.99 |
| Amazon Digital | amazon.com | Bayes Theorem Examples                         |  3.99 |
| Amazon Digital | amazon.com | Every Word (Kindle game)                         | 0.00 |
| Wildlife Foundation of Florida | fishwildlifeflorida.org | The Great Florida Birding Wildlife Trail Guide |  2.99 |
| LIXIT | lixit.com | Plastic Wide Mouth Water Bottle          |   6.60 |
| Canon | usa.canon.com | Canon EF-S 55-255mm Telephoto Zoom Lens        |   137.00 |

The benefit of 2NF is that tables have less or no data redundancy. In the table above, Website is repeated on multiple rows even though it doesn't describe the product. Should the table be updated by changing one of the Manufacturer values, we would have to take special care to remember to update Website appropriately. Data is duplicated but also subject to data integrity errors. The solution would be to extract the Website information into another table that just contains Manufacturer / Website pairs.

### Third normal form (3NF)

> The table is in 2NF and furthermore no column has values that depend on any other columns except the candidate keys.

In other words, all columns besides the key column(s) must depend only on the key column(s). Stated another way (from [Wikipedia](https://en.wikipedia.org/wiki/Third_normal_form#.22Nothing_but_the_key.22)):

> Every non-key column must provide a fact about the key (1NF), the whole key (2NF), and nothing but the key (3NF).

Consider the following violation of 3NF. Here, the customers table has a key Customer, though Email would be an equally good key. The column Favorite Product depends on the customer (the key), so that column does not violate any normal forms. However, the column Favorite Product Price violates 3NF because it depends on Favorite Product, which is not the key.

| Customer | Email | Favorite Product | Favorite Product Price |
|---|
| Josh | jeckroth@stetson.edu | Biscoff Family Pack | 31.93 |
| Tracy | tracy@example.com | The Great Florida Birding Wildlife Trail Guide | 2.99 |

The benefit of 3NF is that every table represents just one set of information rather than mixing information of different kinds together. When 3NF is satisfied, selecting or updating from a table is less error prone and more predictable, since the table only contains one kind of information. Data redundancy is reduced since, as in the example above, presumably the unnecessary information (Favorite Product Price) is already recorded elsewhere. Data integrity is improved because data updates, for example updates to product pricing, will only impact one table if 3NF is met. 3NF also encourages new, complex SELCT queries since we can easily JOIN together data from various tables and construct a new "virtual" table from those columns. 



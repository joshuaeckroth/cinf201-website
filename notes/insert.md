---
title: Insert
layout: note
---

# Insert

- high
- level
- points

## Insert grammar

~~~
INSERT
    [INTO] tbl_name
    [(col_name,...)]
    {VALUES | VALUE} ({expr | DEFAULT},...),(...),...
    [ ON DUPLICATE KEY UPDATE
      col_name=expr
        [, col_name=expr] ... ]
~~~

Simplified from [MySQL docs](http://dev.mysql.com/doc/refman/5.7/en/insert.html).

## Inserting full rows

one row

multiple rows all at once

## Inserting partial rows

## Inserting possibly duplicate rows


---
layout: post
title: "SQL - drop and delete"
categories:
  - Blog Posts
top_tags: General
tags:
- Data Toolbox
---

<br> 

## Deleting Databases

Deleting databases is done using the GUI after entering authentication and confirming your selection. 

<br> 

## Deleting Tables

Another simple command, use the `DROP` function to delete a table. 

```
DROP TABLE example_table;
```

<br> 

## Deleting Columns

First you must specify the table you're altering, then the row you wish to drop.

```
ALTER TABLE example_table_2
  DROP COLUMN email;
```

<br> 

## Deleting Rows

You can delete a single row by referencing a primary key, or a number of rows using logic with the where clause. 

Deleting a hypothetical single row:

```
DELETE FROM example_table
  WHERE 
    emp_no = 99903;
```

<br> 

Deleting many hypothetical rows:
```
DELETE FROM example_table
  WHERE
    start_date <= '2015-01-01'
```

<br>

That's all for now!

<br> 
<br> 

 

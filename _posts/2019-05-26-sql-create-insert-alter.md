---
layout: post
title: "SQL - create, insert, alter"
categories:
  - Blog Posts
top_tags: General
tags:
- Data Toolbox
---

<br> 

## Databases and Schema 

In MySQL there is no distinction between create database and create schema. Virtually all others (except SQLite) there is a difference. Schema collection of tables, database collection of schema. 

``` 
CREATE DATABASE IF NOT EXISTS example_database;
CREATE SCHEMA IF NOT EXISTS example_schema;
```

If using MySQL workbench, remember to refresh. These are flavored for MySQL so your results may vary if using PostgreSQL, Snowflake, Access, etc. 


```
USE example_database;
```

<br> 

## Tables and Keys

creating a table and it's variables

```
CREATE TABLE example_table {
  purchase_num INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(255),
  item_code VARCHAR(10),
  date_of_purchase DATE NOT NULL,
  price NUMERIC(10,2),
};
```

<br>

another way to set keys

```
CREATE TABLE example_table_2 { 
  item_code VARCHAR(10) NOT NULL,
  example_date DATE,
  example_string VARCHAR(255),
  PRIMARY KEY (item_code)
};
```

<br>

final way to set up keys


```
CREATE TABLE example_table_3 { 
  customer_id VARCHAR(10) NOT NULL,
  name VARCHAR(255) NOT NULL,
  example_int INT,
  example_date DATE
};

ALTER TABLE example_table_3
  ADD CONSTRAINT customer_id_pk
  PRIMARY KEY (customer_id)

ALTER TABLE example_table_3
  ADD FOREIGN KEY (name) REFERENCES example_table (name) ON DELETE CASCADE;
```

<br>

## Variables

http://www-db.deis.unibo.it/courses/TW/DOCS/w3schools/sql/sql_datatypes_general.asp.html

strings - char (short), varchar (long), enum (basically factors) <br> 
integers - tinyint, smallint, mediumint, int, bigint (differences in storage) <br>
fixed - decimal(5,3) = 10.523 = numeric <br> 
float - float(5,3) = 10.523, double = longer and more storage <br> 
date / time / date-time / binary / factors, timestamp is exact moment <br> 
blog - binary large objects (.doc, .jpeg, etc) <br> 

<br> 


## Insert

Single entry, match the types!

```
INSERT INTO my_table VALUES (999, NULL, 'John', 'Wick');
```

<br>

many entries

```
INSERT INTO bookings (bookid, facid, memid, starttime, slots) VALUES
(0, 3, 1, '2012-07-03 11:00:00', 2),
(1, 4, 1, '2012-07-03 08:00:00', 2),
(2, 6, 0, '2012-07-03 18:00:00', 2),
(3, 7, 1, '2012-07-03 19:00:00', 2),
(4, 8, 1, '2012-07-03 10:00:00', 1),
(5, 8, 1, '2012-07-03 15:00:00', 1),
(6, 0, 0, '2012-07-04 09:00:00', 3),
(7, 0, 2, '2012-07-04 15:00:00', 3);
```

<br> 

## add / alter column

add new row- 

```
ALTER TABLE example_table
  ADD email VARCHAR(255);
```

<br> 

alter existing row - 

```
ALTER TABLE example_table
  ALTER COLUMN column_name NEW_DATA_TYPE;
```


<br> 
<br> 
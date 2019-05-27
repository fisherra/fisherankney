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

## Drop 

drop an entire object - 

```
DROP TABLE example_table;
```

<br> 

drop a column from the dataset - 
```
ALTER TABLE example_table_2
  DROP COLUMN email;
```

<br> 

## Delete

delete entry from a table, where conditions apply. deleting an entry (row): 

```
DELETE FROM example_table
  WHERE 
    emp_no = 99903;
```

That's it!

<br> 
<br> 

 

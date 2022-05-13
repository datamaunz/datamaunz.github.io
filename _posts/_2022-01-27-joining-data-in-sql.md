--- 
layout: post 
title: Joining data in SQL
subtitle: Notes from Chester Ismay's Course
categories: SQL
tags: [SQL, Joins]
---

Great [resource for joins](https://www.dofactory.com/sql/join)

Most of the content (and all images if not specified differently) are taken from [Joining Data in SQL](https://app.datacamp.com/learn/courses/joining-data-in-postgresql) by Chester Ismay.

## Inner joins

left_table

| id | val |
|---|---|
|1|L1|
|2|L2|
|3|L3|
|4|L4|


right_table

| id | val |
|---|---|
|1|R1|
|4|R2|
|5|R3|
|6|R4|

```SQL
SELECT left_table.id AS L_id, 
       left_table.val AS L_val,
       right_table.val AS R_val
FROM left_table
INNER JOIN right_table
ON left_table.id = right_table.id;
```

| L_id | L_val | R_val |
|---|---|---|
|1|L1|R1|
|4|L4|R2|


## Using


`using`: can be used if column for join has the same name in both tables

```SQL
SELECT left_table.id AS L_id, 
       left_table.val AS L_val,
       right_table.val AS R_val
FROM left_table
INNER JOIN right_table
USING (id)
```

| L_id | L_val | R_val |
|---|---|---|
|1|L1|R1|
|4|L4|R2|

## Self joins



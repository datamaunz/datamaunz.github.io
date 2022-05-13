--- 
layout: post 
title: Introduction to SQL
subtitle: Notes from Nick Carchedi's SQL Course
categories: SQL
tags: [SQL, Intro]
---

Most of the content (and all images if not specified differently) are taken from [Introduction to SQL](https://campus.datacamp.com/courses/introduction-to-sql) by Nick Carchedi.

## SQL

- **SQL** stands for **Structured Query Language** 
- It is a language for interacting with data stored in something called a **relational database**.
- A relational database can be thought of as a collection of tables. 
- A table is just a set of rows (aka `records`) and columns (called `fields`), like a spreadsheet, which represents exactly one type of entity.
- For example, a table might represent employees in a company or purchases made, but not both.

## Selecting columns

Assume we have a table called *people*

| id | name	| birthdate	|deathdate |
|---|---|---|---|
|1|50 Cent|1975-07-06|null|
|2|A. Michael Baldwin|1963-04-04|null|

*... showing 2 out of 8397 rows*

To select the *name* column we can use the following command:

```SQL
SELECT name FROM people;
```

> SELECT and FROM are called keywords and by convention they are written in uppercase (even though SQL is not case sensitive)

**Select multiple columns**

```SQL
SELECT name, birthdate
FROM people;
```

**Select all columns**

```SQL
SELECT *
FROM people;
```

**Limit the number of returned results**

```SQL
SELECT *
FROM people
LIMIT 10;
```

**Select all the unique values from a column**

```SQL
SELECT DISTINCT name
FROM people;
```

**Count the number of rows**

```SQL
SELECT COUNT(*)
FROM people;
```

**Count the number of non-missing values in a column**

```SQL
SELECT COUNT(birthdate)
FROM people;
```

**Count the number of distinct values in a column**

```SQL
SELECT COUNT(DISTINCT birthdate)
FROM people;
```

## Filtering rows

`WHERE` allows you to filter based on both text and numeric values in a table by using the following operators:

- `=` equal
- `<>` not equal
- `<` less than
- `>` greater than
- `<=` less than or equal to
- `>=` greater than or equal to

`WHERE` always comes after the `FROM` statement

Assume we have a tbale called *films* with the following columns:

| id | title| release_year	|country | duration | language | certification | budget |
|---|---|---|---|---|---|---|---|

...Showing 0 out of 4986 rows

<hr>

**Return all rows satisfying a specific condition**

```SQL
SELECT title
FROM films
WHERE title = 'Metropolis';
```

> In PostgreSQL, you must use single quotes with WHERE

```SQL
SELECT title
FROM films
WHERE release_year > 2000;
```

<hr>

**Return all rows satisfying a combination of multiple  conditions**

```SQL
SELECT title
FROM films
WHERE release_year > 1990
AND release_year < 2010;
```

```SQL
SELECT title
FROM films
WHERE release_year = 1990
OR release_year = 2010;
```

```SQL
SELECT title
FROM films
WHERE (release_year = 1994 OR release_year = 1995)
AND (certification = 'PG' OR certification = 'R');
```
<hr>

```SQL
SELECT title
FROM films
WHERE release_year BETWEEN 1994 AND 2000;
```
is equivalent to
```SQL
SELECT title
FROM films
WHERE release_year >= 1994
AND release_year <= 2000;
```
<hr>

```SQL
SELECT title
FROM kids
WHERE release_year IN (1956, 1990, 2000, 2010);
```

<hr>

`NULL` represents a missing or unknown value. Check for NULL values by using the expression `IS NULL`.

**Count the number of missing birth dates in the *people* table**

```SQL
SELECT COUNT(*)
FROM people
WHERE birthdate IS NULL;
```
**Select all rows with no missing values in particular column**

```SQL
SELECT name
FROM people
WHERE birthdate IS NOT NULL;
```
<hr>

- `LIKE` (and `NOT LIKE`) can be used to search for patterns in a column
- `%` is a wildcard which matches zero, one, or many characters in text
- `_` is a wildcard which matches a single character in text

**Select all rows where name in column starts with A**

```SQL
SELECT name
FROM people
WHERE name LIKE 'A%';
```

**Select all rows where name in column do not start with A**

```SQL
SELECT name
FROM people
WHERE name NOT LIKE 'A%';
```

### Aggregate functions

```SQL
SELECT MIN(budget)
FROM films;
```

```SQL
SELECT MAX(budget)
FROM films;
```

```SQL
SELECT SUM(budget)
FROM films;
```

```SQL
SELECT AVG(budget)
FROM films;
```

<hr>

```SQL
SELECT SUM(budget)
FROM films
WHERE release_year >= 2010;
```

<hr>

`+`, `-`, `/`, `*`: symbols for basic arithmetic

```SQL
SELECT (4 * 3);
```
```Output
12
```

> SQL assumes that if you divide an integer by an integer, you want to get an integer back.

```SQL
SELECT (4 / 3);
```
```Output
1
```

```SQL
SELECT (4.0 / 3.0)
```
```Output
1.333
```

<hr>

`AS`: assign a temporary name to something (aka *aliasing*)

```SQL
SELECT MAX(budget) AS max_budget,
       MAX(duration) AS max_duration
FROM films;
```

### Sorting


- `ORDER BY`: sort results in ascending or descending order according to the values of one or more columns.
- `DESC`: sort in descending order (default is ascending order)

> Put `ORDER BY` at the end of your query

**Sort by a column in ascending order**

```SQL
SELECT title
FROM films
ORDER BY release_year;
```

**Sort by a column in descending order**

```SQL
SELECT title
FROM films
ORDER BY release_year DESC;
```

**Sort multiple columns (order of columns matters!)**

```SQL
SELECT birthdate, name
FROM people
ORDER BY birthdate, name;
```

## Grouping

`GROUP BY`: group a result by one or more columns (used with aggregate functions like `COUNT()` or `MAX()`)

> `GROUP BY` always goes after the `FROM` clause

```SQL
SELECT release_year, count(*)
FROM films
GROUP BY release_year
ORDER BY count;
```

## Having

> Aggregate functions can't be used in `WHERE` clauses

```SQL
SELECT release_year
FROM films
GROUP BY release_year
HAVING COUNT(title) > 10;
```

[Joining Data in SQL](https://tea-berlin.github.io/sql/2022/01/27/joining-data-in-sql.html)


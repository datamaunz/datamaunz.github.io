---
layout: post
title: Intro to Relational Databases
subtitle: Notes from Datacamp Course
categories: Databases
tags: [Relational Databases, Intro]
---

Most of the content (and all images if not specified differently) is taken from [Introduction to relational databases in SQL](https://campus.datacamp.com/courses/introduction-to-relational-databases-in-sql) by Timo Grossenbacher.

## Prerequisites

[Introduction to SQL](https://www.datacamp.com/courses/introduction-to-sql)

## Relational Databases

- real-life *entitites* become *tables*
- reduced redundancy
- data integrity by *relationships*

### Inspecting PostgreSQL databases

- `PostgreSQL` is a database management system
- `information_schema`: 
    - a meta-database that holds information about your current database
    - has multiple tables you can query with the known SELECT * FROM syntax

- `information_schema.tables`: information about all tables in your current database

```SQL
 SELECT table_schema, table_name 
 FROM information_schema.tables;
```

`information_schema.columns`: information about all columns in all of the tables in your current database

```SQL
SELECT table_name, column_name, data_type 
FROM information_schema.columns
WHERE table_name = 'pg_config';
```

### Create new tables
#### Syntax

```SQL
CREATE TABLE table_name (
    column_a data_type,
    column_b data_type,
    column_c data_type
    );
```
#### Example

```SQL
CREATE TABLE weather (
    clouds text, 
    temperature numeric, 
    weather_station char(5)
    );
```
### Add a column to table

```SQL
ALTER TABLE table_name
ADD COLUMN column_name data_type;
```

### Rename a column

```SQL
ALTER TABLE table_name
RENAME COLUMN old_name TO new_name;
```

### Drop a column

```SQL
ALTER TABLE table_name
DROP COLUMN column_name;
```

### Insert records
#### General

```SQL
 INSERT INTO table_name (column_a, column_b) 
 VALUES ("value_a", "value_b");
```

#### Migrate (distinct) data

```SQL
INSERT INTO table_name_1
SELECT DISTINCT column_name_1, column_name_2
FROM table_name_2;
```

### Delete a table

```SQL
DROP TABLE table_name;
```

## Constraints

- Constraints
    - give the data structure
    - help with consistency, and thus data quality
- PostgreSQL helps with enforcement

### Attribute constraints

#### Data types

(see [documentation of PostgreSQL](https://www.postgresql.org/docs/10/datatype.html))

- enforced on columns (i.e. attributes)
- define the so-called ***domain*** of a column
- define what operations are possible
- enforce consistent storage of values
- check out [PostgreSQL's data type](https://www.postgresql.org/docs/10/datatype.html) options
- most common types:
    - `text`: character strings of any length
    - `varchar [ (n) ]`: a maximum of `n` characters
    - `char [ (n) ]`: a fixed-length string of `n` characters
    - `boolean`: can only take three states, e.g., `TRUE`, `FALSE` and `NULL` (unknown)
    - `date`, `time` and `timestamp`: various formats for date and time calculations
    - `numeric`: arbitrary precision numbers, e.g. `3.1457`
    - `integer`: whole numbers in the range of `-2147483648` and `2147483647`

#### Casting

```SQL
CREATE TABLE weather ( 
    temperature integer,
    wind_speed text
    );
SELECT temperature * wind_speed AS wind_chill
FROM weather;
```

```OUTPUT
operator does not exist: integer * text
HINT: No operator matches the given name and argument type(s). You might need to add explicit type casts.
```

```SQL
CREATE TABLE weather ( 
    temperature integer,
    wind_speed text
    );
SELECT temperature * CAST(wind_speed AS integer) AS wind_chill
FROM weather;
```
<hr>

```SQL
SELECT CAST(some_column AS integer)
FROM table;
```

#### Alter types (after table creation)

```SQL
ALTER TABLE table_name
ALTER COLUMN column_name
TYPE integer;
```
<hr>

```SQL
ALTER TABLE table_name
ALTER COLUMN column_name
TYPE integer
-- Turns 5.54 into 6, not 5, before type conversion
USING ROUND(column_name);
```

```SQL
ALTER TABLE table_name
ALTER COLUMN column_name
TYPE varchar(x)
-- If you don't want to reserve too much space for a certain varchar column, you can truncate the values before converting its type
USING SUBSTRING(column_name FROM 1 FOR x)
```

#### not-null constraint
##### General

- disallow `NULL` values in a certain column
- must hold true for the current state of the column
- must hold true for any future state of the column

##### Meaning of Null

- unknown
- does not exist
- does not apply
- ...

##### Set null constraint

```SQL
CREATE TABLE table_name (
    unique_id integer not null
    lastname varchar(64) not null,
    home_phone integer,
    office_phone integer
    );
```

```SQL
ALTER TABLE table_name
ALTER COLUMN home_phone
SET NOT NULL
```

##### Remove null constraint

```SQL
ALTER TABLE table_name
ALTER COLUMN lastname
DROP NOT NULL
```

#### unique constraint

##### General

- disallow duplicate values in a column
- must hold true for the current state of the column
- must hold true for any future state of the column

##### Adding unique constraints

```SQL
CREATE TABLE table_name (
    column_name UNIQUE
    );
```

```SQL
ALTER TABLE table_name
-- you have to give a name to the constraint (some_name)
ADD CONSTRAINT some_name UNIQUE(column_name);
```

### Key constraints

#### What is a key?

- Attribute(s) that identify a record uniquely
- As long as atributes can be removed: **superkey**
- if no more attributes can be removed: minimal superkey or **key**
- only one candidate key can be the *chosen* key

#### Primary keys
##### Characteristics
- One primary key per database table, chosen from candidate keys
- Uniquely identifies records, e.g. for referencing in other tables
- Unique and not-null constraints both apply
- Primary keys are time-invariant: choose columns wisely!

##### Identify Unique Keys

```SQL
-- Try out different combinations
SELECT COUNT(DISTINCT(column_a, column_b, column_c)) 
FROM table_name;
```

##### Specifying primary keys

```SQL
CREATE TABLE products (
    product_no integer UNIQUE NOT NULL,
    name text,
    price numeric
    );

-- same input constraints but primary key explicitly specified

CREATE TABLE products (
    product_no integer PRIMARY KEY,
    name text,
    price numeric
    );
```

```SQL
-- designate more than one column as the primary key
CREATE TABLE example (
    a integer,
    b integer,
    c integer,
    PRIMARY KEY (a, c)
    );
```

```SQL
ALTER TABLE table_name
ADD CONSTRAINT some_name PRIMARY KEY (column_name)
```

#### Surrogate keys
##### Characteristics

- Artificial primary key that is not natively part of the table
- Primary keys should be built from as few columns as possible
- Primary keys should never change over time

##### Adding a surrogate key 
###### via serial data type

Let's call the following table **cars**

| make | model | color |
| --- | --- | --- |
|Ford| Mustang | blue |
|Oldsmobile| Cutlass | black |
|Oldsmobile| Delta | silver |
|Mercedes| 190_D | champagne |
|Toyota| Camry | red |
|Jaguar| XJS | blue |

```SQL
ALTER TABLE cars
ADD COLUMN id serial PRIMARY KEY; 
```

| make | model | color | id |
| --- | --- | --- | --- |
|Ford| Mustang | blue | 1 |
|Oldsmobile| Cutlass | black | 2 |
|Oldsmobile| Delta | silver | 3 |
|Mercedes| 190_D | champagne | 4 |
|Toyota| Camry | red | 5 |
|Jaguar| XJS | blue | 6 |

```SQL
INSERT INTO cars
VALUES ('Volkswagen', 'Blitz', 'black');
```

| make | model | color | id |
| --- | --- | --- | --- |
|Ford| Mustang | blue | 1 |
|Oldsmobile| Cutlass | black | 2 |
|Oldsmobile| Delta | silver | 3 |
|Mercedes| 190_D | champagne | 4 |
|Toyota| Camry | red | 5 |
|Jaguar| XJS | blue | 6 |
|Volkswagen| blitz | black | 7 |

```SQL
INSERT INTO cars
VALUES ('Opel', 'Astra', 'green', 1);
```

```OUTPUT
duplicate key value violates unique constraint "id_pkey" DETAIL: Key (id)=(1) already exists.
```

###### via concatenation

```SQL
ALTER TABLE table_name
ADD COLUMN column_c varchar(256);

UPDATE table_name
SET column_c = CONCAT(column_a, column_b); 

ALTER TABLE table_name
ADD CONSTRAINT pk PRIMARY KEY (column_c);
```

### Referential integrity constraints

#### Foreign keys

- A foreign key (FK) points to the primary key (PK) of another table
- Domain of FK must be equal to domain of PK
- FK constraint of referential integrity: Each value of FK must exist in PK of the other table
- FKs are not actual *keys* (because duplicates and null values are allowed)

#### Specifying foreign keys

```SQL

-- table 1
CREATE TABLE manufacturers (
    name varchar(255) PRIMARY KEY);

INSERT INTO manufacturers VALUES ('Ford'), ('VW'), ('GM'); 

-- table 2
CREATE TABLE cars (
    model varchar(255) PRIMARY KEY,
    -- create reference to table 1
    manufacturer_name varchar(255) REFERENCES manufacturers (name));

-- only cars with valid manufactures can now be entered in the table
INSERT INTO cars
VALUES ('Ranger', 'Ford'), ('Beetle', 'VW');
```

```SQL
-- Throws an error! (because manufacturer not available in manufacturers table)
INSERT INTO cars
VALUES ('Tundra', 'Toyota');
```

#### Specifying foreign keys to existing tables

```SQL
ALTER TABLE a
ADD CONSTRAINT a_fkey FOREIGN KEY (b_id) REFERENCES b (id);
```

- Table `a` should now refer to table `b`, via `b_id`, which points to `id`. 
- `a_fkey` is, as usual, a constraint name you can choose on your own.
- naming convention: a foreign key referencing another primary key with name `id` is named `x_id`, where `x` is the **name of the referencing table** in the singular form.

#### Syntax for joins

While foreign keys and primary keys are not strictly necessary for join queries, they greatly help by telling you what to expect.

```SQL
SELECT ...
FROM table_a
JOIN table_b
ON ...
WHERE ...
```

#### N:M-relationships
##### General (Many-to-many relationship)

- Create a table
- Add foreign keys for every connected table
- Add additional attributes

```SQL
CREATE TABLE affiliations (
    professor_id integer REFERENCES professors (id),
    organization_id varchar(256) REFERENCES organizations (id), 
    function varchar(256)
    );
```
- Note: no primary key! (because one professor can play multiple functions)
- Possible PK = {professor_id, organization_id, function}

##### Update columns of a table based on values in another table

```SQL
UPDATE table_a
SET column_to_update = table_b.column_to_update_from
FROM table_b
WHERE condition1 AND condition2 AND ...;
```

#### Referential integrity

- ***A record referencing another table must refer to an existing record in that table***
- Specified between two tables
- Enforced through foreign keys

#### Referential integrity violations

Referential integrity from table A to table B is violated
- if a crecord in table B that is referenced from a record in table A is deleted
- if a crecord in table A referencing a non-existing record from table B is inserted

**Foreign keys prevent violations**

#### Dealing with violations

```SQL
CREATE TABLE a (
    id integer PRIMARY KEY,
    column_a varchar(64),
    -- ...,
    b_id integer REFERENCES b (id) ON DELETE NO ACTION
    );
```

```SQL
CREATE TABLE a (
    id integer PRIMARY KEY,
    column_a varchar(64),
    -- ...,
    b_id integer REFERENCES b (id) ON DELETE CASCADE
    );
```

`ON DELETE`
- `NO ACTION`: Throw an error
- `CASCADE`: Delete all referencing records
- `RESTRICT`: Throw an error
- `SET NULL`: Set the referencing column to NULL
- `SET DEFAULT`: Set the referencing column to its default value

#### Modify existing constraints 

```SQL
-- Identify the correct constraint name
SELECT constraint_name, table_name, constraint_type
FROM information_schema.table_constraints
WHERE constraint_type = 'FOREIGN KEY';
```

```SQL
-- Drop the right foreign key constraint
ALTER TABLE table_name
DROP CONSTRAINT name_of_foreign_key_constraint;
```

```SQL
-- Add a new foreign key constraint which cascades deletion
ALTER TABLE table_name
ADD CONSTRAINT name_of_foreign_key_constraint FOREIGN KEY (other_table_id) REFERENCES other_table (id) ON DELETE CASCADE;
```
--- 
layout: post 
title: Intro to Pyspark
subtitle: Notes from DataCamp Course
categories: Pyspark
tags: [Pyspark, Intro]
---

Most of the content (and all images if not specified differently) is taken from [Introduction to Pyspark](https://campus.datacamp.com/courses/introduction-to-pyspark) on DataCamp.

## What is Spark

### Why?

- Platform for cluster computing
- Spread data and computations over *clusters* with multiple nodes (each node can be thought of as a separate computer)
- As a consequence, data processing and computation are performed *in parallel* over the nodes in the cluster speeding up certain types of programming tasks
- Tradeoff: Speed ⬆ & Complexity ⬆
- Ask yourself:
    - **Is my data too big to work with one single machine?**
    - **Can my calculations be easily parallelized?**

### How?

- Connect to a cluster (usually hosted on a remote machine that is connected to all other nodes; when starting it is easier to run a cluster locally)
- *Master* (computer): splits up the data and computations and sends them to the workers
- *Worker* (computers): runs calculations and computations and sends the results back to the master
- If you're just starting: Run a cluster locally


## Spark in Python

- create a connection by **creating an instance of the `SparkContext` class**
    - specify the attributes of the cluster by passing optional arguments
    - create an object that holds all these attributes with `SparkConf()`
    - see the [documentation](https://spark.apache.org/docs/2.1.0/api/python/pyspark.html) for more details

## RDDs and DataFrames

- `Resilient Distributed Dataset` (RDD): Spark's core data structure
    - a low level object allowing Spark to split data across various nodes in the cluster
- `DataFrame` abstraction built on top of RDDs, since RDDs are notoriously hard to work with directly
    - designed to behave like a SQL table
    - easier to understand than RDDs and optimized for complicated operations

### Create SparkSession

- `SparkContext`: your connection to the cluster
- `SparkSession`: your interface with that connection
- create a `SparkSession` form your `SparkContext`
- `SparkSession.builder.getOrCreate()` returns an existing SparkSession if there is one in the environment; otherwise a new session is being created

```Python
# Import SparkSession from pyspark.sql
from pyspark.sql import SparkSession

# Create my_spark
spark = SparkSession.builder.getOrCreate()
```

### Viewing tables

- `SparkSession.catalog`: lists all the data inside the cluster
- `SparkSession.catalog.listTables()`: returns the names of all tables in your cluster

### Run queries

- `.sql()`: run a query on a `SparkSession`

```Python
# flights is a table available in the SparkSession called spark
query = "FROM flights SELECT * LIMIT 10"

# Get the first 10 rows of flights
flights10 = spark.sql(query)

# Show the results
flights10.show()
```

### Spark to Pandas

- `.toPandas()`: turns Spark DataFrame into `pandas` DataFrame

```Python
# flights is a table available in the SparkSession called spark
query = "SELECT origin, dest, COUNT(*) as N FROM flights GROUP BY origin, dest"

# Run the query
flight_counts = spark.sql(query)

# Convert the results to a pandas DataFrame
pd_counts = flight_counts.toPandas()

# Print the head of pd_counts
print(pd_counts.head())
```

### Pandas to Spark

- `.createDataFrame()`: put a pandas dataframe into a Spark cluster
    - output is stored locally not in the `SparkSession` catalog
    - to access the data in different contexts, save it as a temporary table
- `.createTempView()`: takes output of `.createDataFrame()` as argument and registers it as a table in the catalog
    - only accessible via the `SparkSession` used to create the Spark DataFrame
- `.createOrReplaceTempView()` creates a new temporary table if nothing was there before; otherwise updating the existing table
    - useful to prevent duplicate tables

All at one glance:

![](https://3772604856-files.gitbook.io/~/files/v0/b/gitbook-28427.appspot.com/o/assets%2F-LiBoWdc5sh0EFrazIOe%2F-Lr9h_2hIX89P1llRafl%2F-Lr9l7E7BIFlKuMWjVh3%2Fimage.png?alt=media&token=9846e324-ac7a-4bd8-b6b2-323f8bcf95a3)

```Python
# Create pd_temp
pd_temp = pd.DataFrame(np.random.random(10))

# Create spark_temp from pd_temp
spark_temp = spark.createDataFrame(pd_temp)

# Examine the tables in the catalog
print(spark.catalog.listTables())

# Add spark_temp to the catalog and call it 'temp'
spark_temp.createOrReplaceTempView("temp")

# Examine the tables in the catalog again
print(spark.catalog.listTables())
```

### CSV to Spark

- `.read.csv()`: create Spark DataFrame from csv file

```Python
# name of the SparkSession is spark
file_path = "/usr/local/share/datasets/airports.csv"

# Read in the airports data
airports = spark.read.csv(file_path, header=True)

# Show the data
airports.show()
```

## Creating Columns

- `.withColumn()`: takes two arguments - 1. name of new column, 2. the new column itself (must be an object of class `Column` from your DataFrame using `df.colName`)
- Spark DataFrames are immutable which is why changes require to return a new DataFrame

```Python
df = df.withColumn("newCol", df.oldCol + 1)
```

```Python
# name of the SparkSession is spark containing a table called 'flights'
flights = spark.table('flights')

# Show the head
flights.show()

# Add duration_hrs
flights = flights.withColumn('duration_hrs', flights.air_time / 60)
```

### Filtering Data

- use the `.filter()` method by passing
    - a string that would go behind an SQL WHERE operator, or
    - a column of boolean values
- these two expression yield the same outcome: 

```Python
flights.filter("air_time > 120").show()
flights.filter(flights.air_time > 120).show()
```

### Selecting

- `.select()`: Spark's variant of SQL's `SELECT` (takes one argument per column that is to be selected)
    - you can use string or df.colName syntax
- `.select()` only returns selected columns; `withColumn()` returns all columns of the DataFrame














--- 
layout: post 
title: Big Data Fundamentals with PySpark
subtitle: Notes from Upendra Devisetty's DataCamp Course
categories: PySpark
tags: [Big Data, PySpark]
---

## Intro

- **Big Data** refers to data sets that are too complex for traditional data-processing software

### 3 Vs of Big Data

- **Volume**: Size of the data
- **Variety**: Different sources and formats
- **Velocity**: Speed of the data

### Concepts and Terminology

- **Clustered computing**: Collection of resources of multiple machines
- **Parallel computing**: Simultaneous computation
- **Distributed computing**: Collection of nodes (networked computers) that run in parallel
- **Batch processing**: Breaking the job into small pieces and running them on individual machines
- **Real-time processing**: Immediate processing of data

### Porcessing systems

- **Hadoop/MapReduce**: Scalable and fault tolerant framework written in Java
    - Open source
    - Batch processing

- **Apache Spark**: General purpose and lightning fast cluster computing system
    - Open source
    - Both batch and real-time data processing
    - **Features**
        - Distributed cluster computing framework
        - Efficient in-memory computations for large data sets
        - Lightning fast data processing framework
        - Provide support for Java, Scala, Python, R, SQL
    - **Components**
        - RDD API Apache Spark Core
            - Spark SQL
            - MLlib Machine Learning
            - GraphX
            - Spark Streaming
    - **Modes of Deployment**
        - **Local Mode**: Single machine such as laptop
            - convenient for testing, debugging and demonstration
        - **Cluster Mode**: Set of predefined machines
            - good for production
        - **Workflow**: Local -> Cluster
        - No code change necessary

## PySpark

Spark with Python!

### Overview

- Apache Spark is written in Scala
- PySpark was designed to support Python with Spark
- Similar computation speed and power as Scala
- PySpark APIs are similar to Pandas and Scikit-learn

### Spark shell

- interactive environment for running Spark jobs
- helpful for fast interactive prototyping
- Spark's shell allow interacting with data on disk or in memory
- 3 different Spark shells:
    - Spark-shell for Scala
    - PySpark-shell for Python
    - SparkR for R

### PySpark shell

- PySpark shell is the Python based command line tool
- PySpark shell allows data scientists interface with Spark data structures
- PySpark shell support connecting to a cluster

### Understanding SparkContext

- SparkContext is an entry point into the world of Spark
- An entry point is a way of connecting to Spark cluster
- An entry point is like a key to the house
- PySpark has a default SparkContext called `sc`

### Inspecting SparkContext

```Python
sc.version # retrieve SparkContext version
```

```Python
sc.pythonVer # retrieve Python version of SparkContext
```

```Python
sc.master # URL of the cluster or "local" string to run in local mode of SparkContext
```

### Loading data in PySpark

```Python
rdd = sc.parallelize([1,2,3,4,5])
```

```Python
rdd2 = sc.textFile("test.txt")
```

## Anonymous functions in Python

- Lambda functions are anonymous functions in Python
- return the functions without any names (i.e. *anonymous*)
- Quite efficient with `map()`and `filter()`
- Lambda functions create functions to be called later
- Inline a function definition or to defer execution of some code

### Lambda function syntax

```Python
lambda arguments: expression # the general form of a lambda function
```

```Python
double = lambda x: x * 2 # example of a lambda function
print(double(3))
```

### Lambda function in map()

- `map()`: takes a function and a list; returns a new list which contains items returned by that function for each item

```Python
map(function, list) # general syntax of map()
```

```Python
items = [1,2,3,4]
list(map(lambda x: x + 2 , items)) # example of map()
```

```Python
items = [1,2,3,4]
list(map(lambda x, y: x + y , items, items)) # example of map() with two arguments
```

### Lambda function in filter()

- `filter()`: takes a function and a list; returns a new list for which the function evaluates as true

```Python
filter(function, list) # general syntax of filter()
```

```Python
items = [1,2,3,4]
list(filter(lambda x: (x%2 != 0), items)) # example of filter()
```

```
[1, 3]
```

## PySpark RDD

- `RDD`: Resilient Distributed Datasets
    - **Resilient**: Ability to withstand failures
    - **Distributed**: Spanning across multiple machines
    - **Datasets**: Collection of partitioned data e.g. Arrays, Tables, Tuples etc.

### General Structure

1. Data File on disk
2. Spark driver creates RDD and distributes amount on Nodes
3. Cluster
    - Node 1: RDD Partition 1
    - Node 2: RDD Partition 2
    - Node 3: RDD Partition 3
    - ...
    - Node *n*: RDD Partition *n*

### Creating RDDs

**Options**

- Parallelizing an existing collection of objects an existing collection of objects
- From external datasets (for example):
    - Files in HDFS
    - Objects in Amazon S3 bucket
    - lines in a text file
- From existing RDDs


### Parallelizing an existing collection of objects

```Python
numRDD = sc.parallelize([1,2,3,4]) # parallelize() for creating RDDs from python lists
```

```Python
helloRDD = sc.parallelize("Hello world")
```

```Python
type(helloRDD)
```

```
<class 'pyspark.rdd.PipelinedRDD'>
```

### From external datasets

```Python
fileRDD = sc.textFile("README.md") # textFile() for creating RDDs from external datasets
```

```Python
type(fileRDD)
```

```
<class 'pyspark.rdd.PipelinedRDD'>
```

### Understanding Partitioning in PySpark

- A partition is a logical division of a large distributed data set

```Python
numRDD = sc.parallelize(range(10), minPartitions = 6)
```

```Python
fileRDD = sc.textFile("README.md", minPartitions = 6)
```

- `getNumPartitions()`: find the number of partitions in an RDD

## RDD operations in PySpark

> Spark Operations = Transformations + Actions

- `Transformations`: create new RDDs
- `Actions`: perform computations on the RDDs

### RDD transformations

Transformation follow lazy evaluation:
- `Storage` (-> *reading data from stable storage*)
    - `RDD1` (-> *transformation*)
        - `RDD2` (-> *transformation*)
            - `RDD3` (-> *action*)
                - `result`

- basic RDD transformations: `map()`, `filter()`, `flatMap()`, `union()`

#### map()

- applies a function to all elements in the RDD

```Python
RDD = sc.parallelize([1,2,3,4])
RDD_map = RDD.map(lambda x: x * x)
```

#### filter()

- returns a new RDD with only the elements that pass the condition

```Python
RDD = sc.parallelize([1,2,3,4])
RDD_filter = RDD.filter(lambda x: x > 2)
```

#### flatMap()

- returns multiple values for each element in teh original RDD

```Python
RDD = sc.parallelize(["hello world", "how are you"])
RDD_flatmap = RDD.flatMap(lambda x: x.split(" ")) # returns ["hello", "world", "how", "are", "you"]
```

#### union()

```Python
inputRDD = sc.textFile("logs.txt")
errorRDD = inputRDD.filter(lambda x: "error" in x.split())
warningsRDD = inputRDD.filter(lambda x: "warnings" in x.split())
combinedRDD = errorRDD.union(warningsRDD)
```

### RDD actions

- Operation returning a value after running a computation on the RDD
- Basic RDD actions: `collect()`, `take(N)`, `first()`, `count()`

#### collect()

- return all the elements of the dataset as an array

```Python
RDD_map.collect()
```

```
[1, 4, 9, 16]
```

#### take()

- return an array with the first N elements of the dataset

```Python
RDD_map.take(2)
```

```
[1, 4]
```

#### first()

- print the first element of the RDD

```Python
RDD_map.first()
```

```
[1]
```

#### count()

- return the number of elements in the RDD

```Python
RDD_flatmap.count()
```

```
5
```

## Pair RDDs in PySpark

- Real life datasets are usually key/value pairs
- Each row is a key and maps to one or more values
- Pair RDD: a special data structure to work with this kind of datasets
- `Pair RDD`: Key is the identifier and value is data

### Creating pair RDDs

Two common ways to create pair RDDs

#### From a list of key-value tuples

```Python
my_tuple = [('Sam', 23), ('Mary', 34), ('Peter', 25)]
pairRDD_tuple = sc.parallelize(my_tuple)
```

#### From a regular RDD

```Python
my_list = ['Sam 23', 'Mary 34', 'Peter 25']
regularRDD = sc.parallelize(my_list)
pairRDD_RDD = regularRDD.map(lambda x: (x.split(" ")[0], x.split(" ")[1]))
```

### Transformations on pair RDDs

- All regular transformations work on pair RDDs
- Requires functions that operate on key value pairs rather than on individual elements
- Examples of paired RDD transformations:
    - `reduceByKey(func)`: Combine values with the same key
    - `groupByKey()`: Group values with the same key
    - `sortByKey()`: Return an RDD sorted by the key
    - `join()`: Join two pair RDDs based on their key

#### reduceByKey()

- Transformation that combines values with the same key
- Runs parallel operations for each key in the dataset

```Python
 regularRDD = sc.parallelize([("Messi", 23), ("Ronaldo", 34), ("Neymar", 22), ("Messi", 24)])
pairRDD_reducebykey = regularRDD.reduceByKey(lambda x,y : x + y)
pairRDD_reducebykey.collect()
```

```
[('Neymar', 22), ('Ronaldo', 34), ('Messi', 47)]
```

#### sortByKey()

- Transformation that orders pair RDD by key
- Returns an RDD sorted by key in ascending or descending order

```Python
pairRDD_reducebykey_rev = pairRDD_reducebykey.map(lambda x: (x[1], x[0]))
pairRDD_reducebykey_rev.sortByKey(ascending=False).collect()
```

```
[(47, 'Messi'), (34, 'Ronaldo'), (22, 'Neymar')]
```

#### groupByKey()

- transformation that groups all the values with the same key in the pair RDD

```Python
airports = [("US", "JFK"),("UK", "LHR"),("FR", "CDG"),("US", "SFO")]
regularRDD = sc.parallelize(airports)
pairRDD_group = regularRDD.groupByKey().collect()
for cont, air in pairRDD_group:
    print(cont, list(air))
```

```
FR ['CDG']
US ['JFK', 'SFO']
UK ['LHR']
```

#### join()

- transformation that joins the two paor RDDs based on their key

```Python
RDD1 = sc.parallelize([("Messi", 34), ("Ronaldo", 32), ("Neymar", 24)])
RDD2 = sc.parallelize([("Ronaldo", 80),("Neymar", 120),("Messi", 100)])
RDD1.join(RDD2).collect()
```

```
[('Neymar', (24, 120)), ('Ronaldo', (32, 80)), ('Messi', (34, 100))]
```

## More actions

### Regular RDD

#### reduce()

- `reduce()`: aggregate the elements of a regular RDD
- should be commutative (ie. changing the order of the operands does not change the result) and associative

```Python
x = [1,3,4,6]
RDD = sc.parallelize(x)
RDD.reduce(lambda x, y: x + y)
```

```
14
```

#### saveAsTextFile()

- `saveAsTextFile()`: saves RDD into a text file inside a directory with each partition as a separate file

```Python
RDD.saveAsTextFile("tempFile")
```
- `coalesce()`: can be used to save RDD as a single text file

```Python
RDD.coalesce(1).saveAsTextFile("tempFile")
```

### Pair RDD

#### countByKey()

- `countByKey`: counts the number of elements for each key
- only available for type(K, V)

```Python
rdd = sc.parallelize([("a", 1), ("b", 1), ("a", 1)])
for kee, val in rdd.countByKey().items():
    print(kee, val)
```

```
('a', 2)
('b', 1)
```

#### collectAsMap()

- `collectAsMap`: return the key value pairs in the RDD as a dictionary

```Python
sc.parallelize([(1, 2), (3, 4)]).collectAsMap() {1: 2, 3: 4}
```

```
{1: 2, 3: 4}
```

## PySpark DataFrame

- `PySpark SQL`: Spark library for structured data.
- `PySpark DataFrame`: an immutable distributed collection of data with named columns
    - designed for processing both structured and semi-structured data
- DataFrame API is available in Python, R, Scala, and Java
- DataFrames in PySpark support both SQL queries (`SELECT * FROM table`) or expression methods (`df.select()`)

### SparkSession

- `SparkContext` is the main entry point for creating RDDs
- `SparkSession` provides a single point of entry to interact with Spark DataFrames
    - used to create DataFrame, register DataFrames, execute SQL queries
    - available in PySpark shell as `spark`

### Creating DataFrames

- Two different methods of creating DataFrames in PySpark
    - From existing RDD using Sparksession's `createDataFrame()` method
    - From various data sources (CSV, JSON, TXT) using Sparksession's `read()`method
- `Schema`
    - controls the data and helps DataFrames to optimize queries
    - provides information about column name, type of data in the column, empty values, etc.

#### From RDD

```Python
iphones_RDD = sc.parallelize([ 
    ("XS", 2018, 5.65, 2.79, 6.24), 
    ("XR", 2018, 5.94, 2.98, 6.84), 
    ("X10", 2017, 5.65, 2.79, 6.13), 
    ("8Plus", 2017, 6.23, 3.07, 7.12)
])
names = ['Model', 'Year', 'Height', 'Width', 'Weight']
iphones_df = spark.createDataFrame(iphones_RDD, schema=names) type(iphones_df)
```
```
pyspark.sql.dataframe.DataFrame
```

#### From reading CSV/JSON/TXT

```Python
df_csv = spark.read.csv("people.csv", header=True, inferSchema=True)
df_json = spark.read.json("people.json", header=True, inferSchema=True) df_txt = spark.read.txt("people.txt", header=True, inferSchema=True)
```

### DataFrame Operators in Pyspark

- DataFrame operations: Transformations and Actions
- DataFrame **Tranformations**: `select()`, `filter()`, `groupby()`, `orderby()`, `dropDuplicates()`, and `withColumnRenamed()`
- DataFrame **Actions**: `head()`, `show()`, `count()`, and `describe()`
- General methods for Spark datsets/dataframes: `printSchema()`

#### Transformations

##### select()

- `select()` subsets the columns in the DataFrame

```Python
df_id_age = test.select('Age')
```

##### show()

- `show()` prints first 20 rows in the DataFrame

```Python
df_id_age.show(3) # shows top 3 rows
```

##### filter()

- `filter()` filters out the rows based on a condition

```Python
new_df_age21 = new_df.filter(new_df.Age > 21)
```

##### groupby() and count()

- `groupby()` can be used to group a variable

```Python
test_df_age_group = test_df.groupby('Age')
test_df_age_group.count().show(3)
```

##### orderBy()

- `orderBy()` sorts the DataFrame based on one or more columns

```Python
test_df_age_group.count().orderBy('Age').show(3)
```

##### dropDuplicates()

- `dropDuplicates()` removes the duplicate rows of a DataFrame

```Python
test_df_no_dup = test_df.select('User_ID','Gender', 'Age').dropDuplicates()
test_df_no_dup.count()
```

```
5892
```

##### withColumnRenamed()

- `withColumnRenamed()` renames a column in the DataFrame

```Python
test_df_sex = test_df.withColumnRenamed('Gender','Sex')
test_df_sex.show()
```

```
+-------+---+---+
|User_ID|Sex|Age|
+-------+---+---+
|1000001|  F| 17| 
|1000001|  F| 17| 
|1000001|  F| 17| 
+-------+---+---+
```

#### Actions

##### columns

- `columns` prints the columns of a DataFrame

```Python
test_df.columns
```

##### describe()

- `describe()` computes summary statistics of numerical columns in the DataFrame

```Python
test_df.describe().show()
```

##### printSchema()

- `printSchema()` prints the types of columns in the DataFrame

```Python
test_df.printSchema()
```

## SparkSQL

- `SparkSQL` allows for use of SQL via DataFrame API
- `DataFrame API` provides a programmatic domain-specific language (DSL) for data
- DataFrame transformations and actions are easier to construct programmatically
- The operations on DataFrames can also be done using SQL queries

### SQL queries

- `sql()`: Sparksession method to execute SQL queries
- takes SQL statement as an argument and returns the result as DataFrame

```Python
df.createOrReplaceTempView("table1")
df2 = spark.sql("SELECT field1, field2 FROM table1")
df2.collect()
```

```
[Row(f1=1, f2='row1'), Row(f1=2, f2='row2'), Row(f1=3, f2='row3')]
``` 

### Extract data

```Python
test_df.createOrReplaceTempView("test_table")

query = '''SELECT Product_ID FROM test_table'''

test_product_df = spark.sql(query)
test_product_df.show(5)
```

### Summarizing and grouping data

```Python
test_df.createOrReplaceTempView("test_table")

query = '''SELECT Age, max(Purchase) FROM test_table GROUP BY Age'''

spark.sql(query).show(5)
```

### Filtering columns

```Python
test_df.createOrReplaceTempView("test_table")

query = '''SELECT Age, Purchase, Gender FROM test_table WHERE Purchase > 20000 AND Gender == "F"'''

spark.sql(query).show(5)
```

## Data Visualization in PySpark

- Open source plotting tools to aid visualization in Python: [Matplotlib](https://matplotlib.org/), [Seaborn](https://seaborn.pydata.org/), [Bokeh](https://bokeh.org/), [Plotly](https://plotly.com/), ...
- Plotting graphs using PySpark DataFrames is done using three methods:
    - `pyspark_dist_explore` library,
    - `toPandas()`, and
    - `HandySpark` library

### pyspark_dist_explore

- `pyspark_dist_explore` provides quick insights into DataFrames
- Currenty three functions available - `hist()`, `distplot()`, `pandas_histogram()`

```Python
test_df = spark.read.csv("test.csv", header=True, inferSchema=True)
test_df_age = test_df.select('Age') 
hist(test_df_age, bins=20, color="red")
```

### Plotting with Pandas

```Python
 test_df = spark.read.csv("test.csv", header=True, inferSchema=True) 
 test_df_sample_pandas = test_df.toPandas() 
 test_df_sample_pandas.hist('Age')
```

|Pandas DataFrame|PySpark DataFrame|
|---|---|
|in-memory, single-server based structures|in parallel on multiple nodes|
|[eager evaluation](https://en.wikipedia.org/wiki/Evaluation_strategy#Eager_evaluation)|[lazy evaluation](https://en.wikipedia.org/wiki/Lazy_evaluation)|
|mutable|immutable|
|supports more operations|supports fewer operations|

### HandySpark

- `HandySpark`: package designed to improve PySpark user experience

```Python
test_df = spark.read.csv('test.csv', header=True, inferSchema=True) 
hdf = test_df.toHandy()
hdf.cols["Age"].hist()
```















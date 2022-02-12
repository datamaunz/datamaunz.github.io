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
- For starters: Run the 


## Spark in Python

- create a connection by **creating an instance of the `SparkContext` class**
    - specify the attributes of the cluster by passing optional arguments
    - create an object that holds all these attributes with `SparkConf()`
    - see the [documentation](https://spark.apache.org/docs/2.1.0/api/python/pyspark.html) for more details




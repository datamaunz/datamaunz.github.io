--- 
layout: post 
title: Intro to Airflow
subtitle: Notes from Mike Metzger's Course
categories: Airflow
tags: [Airflow, Intro]
---

Most of the content (and all images if not specified differently) is taken from [Introduction to Airflow](https://campus.datacamp.com/courses/introduction-to-airflow-in-python) by Mike Metzger.

## General
### What are DAGs

`DAG`: Directed Acyclic Graph

- ***Directed***: inherent flow representing dependencies between components
- ***Acyclic***: does not loop/cycle/repeat
- ***Graph***: the set of components

Within Airflow, DAGs

- are written in Python (but can use components written in other languages)
- are made up of components (typically *tasks*) to be executed, such as operators, sensors, etc.
- contain dependencies defined explicitly or implicitly

### Defining a DAG

```Python
etl_dag = DAG(
    dag_id='etl_pipeline',
    default_args={'start_date':'2022-02-01'}
)
```

```Python
from airflow.models import DAG

from datetime import datetime
default_arguments = {
    'owner': 'TEA',
    'email': 'berlin@techexpertacademy.com',
    'start_date': datetime(2022,2,1),
}
etl_dag = DAG( 'etl_workflow', default_args=default_arguments )
```

### Running a workflow

```Bash
airflow run <dag_id> <task_id> <start_date>
```

```
airflow run example-etl download-file 2022-02-01
```

Learn about available sub-commands

```Bash
airflow
```

```Bash
airflow -h
```

### Command line vs Python

Use the command line tool to:
- start Airflow process
- manually run DAGs / Tasks
- get logging information from Airflow

Use Python to:
- create a DAG
- edit the individual properties of a DAG

### Web UI vs command line

```Bash
airflow webserver -p 9090
```

In most cases:

- equally powerful depending on needs
- Web UI is easier
- Command line tool may be easier to access depending on settings

## Operators
### General

- represent a single task in a workflow
- run independently (usually)
- generally do not share information
- various operators to perform different tasks

```Python
DummyOperator(task_id='example', dag=dag)
```

### BashOperator
#### Syntax

```Python
BashOperator(
    task_id='bash_example',
    bash_command='echo "Example!"',
    dag=ml_dag
    )
```

```Python
BashOperator(
    task_id='bash_script_example',
    bash_command='runcleanup.sh',
    dag=ml_dag
    )
```
- Executes a given Bash command or script.
- Runs the command in a temporary directory.
- Can specify environment variables for the command.

#### Examples

```Python
from airflow.operators.bash_operator import BashOperator
example_task_1 = BashOperator(
    task_id='bash_ex',
    bash_command='echo 1',
    dag=dag
    )
```

```Python
example_task_2 = BashOperator(
    task_id='clean_addresses',
    bash_command='cat addresses.txt | awk "NF==10" > cleaned.txt',
    dag=dag
    )
```

### Operator gotchas

- not guaranteed to run in the same location / environment
- may require extensive use of environment variables
- can be difficult to run tasks with elevated privileges


## Tasks
### General

- instances of operators
- usually assigned to a variable in Python

```Python
example_task = BashOperator(
    task_id='bash_example',
    bash_command='echo "Example!"',
    dag=dag)
```
- referred to by the task_id within the Airflow tools

### Task dependencies

- define a given order of task completion
- are not required for a given workflow ´, but usually present in most
- are referred to as *upstream* or *downstream* tasks
- in Airflow 1.8 and later, defined by using the *bitshift* operator
    - `>>`: upstream operator (means *before*)
    - `<<`: downstream operator (means *after*)
```Python
# Define the tasks

task1 = BashOperator(task_id='first_task', bash_command='echo 1',
dag=example_dag)

task2 = BashOperator(task_id='second_task', bash_command='echo 2',
dag=example_dag)
# Set first_task to run before second_task
task1 >> task2 # or task2 << task1
```

### Multiple dependencies

#### Chained dependencies

```Python
task1 >> task2 >> task3 >> task4
```

#### Mixed dependencies

```Python
task1 >> task2 << task3
```

or

```Python
task1 >> task2
task3 >> task2
```

### PythonOperator
#### Syntax

- executes a Python function / callable
- operates similarly to the BashOperator, with more options
- can pass in arguments to the Python code

```Python
from airflow.operators.python_operator import PythonOperator
def printme():
    print('This goes in the logs!')

python_task = PythonOperator(
    task_id='simple_print', 
    python_callable=printme, 
    dag=example_dag
    )
```

#### Arguments

- supports arguments to tasks
    - positional
    - keyword   
- use the `op_kwargs` dictionary

#### op_kwargs example

```Python
import time
def sleep(length_of_time):
    time.sleep(length_of_time)
    
sleep_task = PythonOperator(
    task_id='sleep',
    python_callable=sleep, 
    op_kwargs={'length_of_time': 5},
    dag=example_dag
)
```

### EmailOperator

- found in the `airflow.operators` library
- sends an email
- can contain typical components
    - html content
    - attachments
- does require the Airflow system to be configured with email server details

```Python
from airflow.operators.email_operator import EmailOperator

email_task = EmailOperator(
    task_id='email_sales_report',
    to='sales_manager@example.com',
    subject='Automated Sales Report',
    html_content='Attached is the latest sales report',
    files='latest_sales.xlsx',
    dag=example_dag )
```

## Airflow scheduling
### DAG Runs

- a specific instance of a workflow at a point in time
- can be run manually or via `schedule_interval`
- maintin state for each workflow and the tasks within
    - `running`
    - `failed`
    - `success`

### Schedule details

When scheduling a DAG, there are several attirbutes of note:

- `start_date` - The date / time to initially schedule the DAG run
- `end_date` - Optional attribute for when to stop running new DAG instances
- `max_tries` - Optional attribute for how many attempts to make
- `schedule_interval` - How often to run

### Schedule interval

`schedule_interval` represents:

- how often to schedule the DAG
- between the `start_date` and `end_date`
- can be defined via `cron` style syntax or via built-in presets

### cron syntax
#### General

![cron job format](https://ostechnix.com/wp-content/uploads/2018/05/cron-job-format-1.png)

- is pulled from the Unix cron format
- conssits of 5 fields separated by a space
- an asterisk `*` represents running for every interval (ie, every minute, every day, etc)
- can be comma separated values in fields for a list of values

#### cron examples

```Bash
0 12 * * *          # Run daily at noon
```

```Bash
* * 25 2 *          # Run once per minute on February 25
```

```Bash
0,15,30,45 * * * *          # Run every 15 minutes
```

#### Airflow scheduler presets
##### Standard presets

|Preset|cron equivalent|
|---|---|
|@hourly|`0 * * * *`|
|@daily|`0 0 * * *`|
|@weekly|`0 0 * * 0`|
|@monthly|`0 0 1 * *`|
|@yearly|`0 0 1 1 *`|

##### Special presets

Airflow has two special `schedule_interval` presets:
- `None` - Don't schedule ever, used for manually triggered DAGs
- `@once` - Schedule only once

#### schedule_interval inssues

When scheduling a DAG, Airflow will:
- use the `start_date` as the earliest possible value
schedule the task at `start_date` + `schedule_interval`

```Python
# the earliest starting time to run the DAG is on February 26th, 2020
'start_date': datatime(2020,2,25),
'schedule_interval': @daily
```

## Sensors
### General

- an operator that waits for a certain condition to be true
    - creation of a file
    - upload of a database record
    - certain response from a web request
- can define how often to check for the condition to be true
- are assigned to tasks
- derived from `airflow_sensors.base_sensor_operator`
- Sensor arguments:
    - `mode='poke'` - default, run repeatedly
    - `mode='reschedule'` - give up task slot and try again later
- `poke_interval` - how often to wait between checks
- `timeout` - how long to wait before failing task
- also includes normal operator attributes

### File sensor

- part of the `airflow.contrib.sensors` library
- checks for the existence of a file at a certain location
- can also check if any files exist within a directory

```Python
from airflow.contrib.sensors.file_sensor import FileSensor

file_sensor_task = FileSensor(
    task_id='file_sense', 
    filepath='salesdata.csv',
    poke_interval=300, 
    dag=sales_report_dag)

init_sales_cleanup >> file_sensor_task >> generate_report
```

### More sensors

- `ExternalTaskSensor` - wait for a task in another DAG to complete
- `HttpSensor` - request a web URL and check for content
- `SqlSensor` - runs an SQL query to check for content
- many others in `airflow.sensors` and `airflow.contrib.sensors`

### Use sensors ...

- if uncertain when it will be true
- if failure not immediately desired
- if you want to add task repetition without loops

## Executors
### Types

- executors run tasks
- different executors handle running the tasks differently
- example executors:
    - `SequentialExecutor`
    - `LocalExecutor`
    - `CeleryExecutor`

#### SequentialExecutor

- the default Airflow executor
- runs one task at a time
- useful for debugging
- while functional, not really recommended for production

#### LocalExecutor

- runs on a single system
- treats tasks as processes
- *parallelism* defined by the user
- can utilize all resources of a given host system

#### CeleryExecutor

- uses a Celery backend as task manager
- multiple worker systems can be defined
- is significantly more difficult to setup & configure
- extremely powerful method for organizations with extensive workflows

### Determine your executor
#### airflow.cfg

- via the `airflow.cfg` file
- look for the `exectuor=` line

```Bash
repl:~$ cat airflow/airflow.cfg | grep "executor = "
```

```Output
executor = SequentialExecutor
```

#### list_dags

- via the first line of `airflow list_dags`

```Bash
repl:~$ airflow list_dags
```

```Output
Info - Using SequentialExecutor
```

## Debugging in Airflow

### Typical Issues
#### Overview

- DAG won't run on schedule
- DAG won't load
- Syntax errors

#### DAG won't run on schedule

- at least one `schedule_interval` hasn't passed
    - modify the attributes to meet your requirements
- not enough tasks free within the executor to run
    - change executor type
    - add system resources
    - add more systems
    - change DAG scheduling

#### DAG won't load



#### Syntax errors
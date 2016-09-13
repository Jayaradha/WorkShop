<img src="http://spark.apache.org/images/spark-logo-trademark.png">

# Apache Spark

## Spark Intro

## Matei Zaharia

<img style="width:50%" src="https://qph.ec.quoracdn.net/main-thumb-8644-200-idoxbttmjhqhdlxtlvgodiymfpvvoidm.jpeg">

What is Spark?

- Spark is a framework for distributed processing.

- It is a streamlined alternative to Map-Reduce.

- Spark applications can be written in Python, Scala, or Java.

## Why Spark

Why learn Spark?

- Spark enables you to analyze petabytes of data.

- Spark is signficantly faster than MapReduce.

- Paradoxically, Spark's API is simpler than the MapReduce API.

## Goals

By the end of this workshop, you will be able to:

- Create RDDs to distribute data across a cluster

- Use the Spark shell to compose and execute Spark commands

- Use Spark to analyze apache access logs.


## Essense of Spark

What is the basic idea of Spark?

- Spark takes the Map-Reduce paradigm and changes it in some critical
  ways.

- Instead of writing single Map-Reduce jobs a Spark job consists of a
  series of map and reduce functions. 
  
- However, the intermediate data is kept in memory instead of being
  written to disk or written to HDFS.



# Spark Fundamentals

## Spark Execution

<img src="https://spark.apache.org/docs/1.1.1/img/cluster-overview.png">


## Spark Terminology

Term     |Meaning
----     |-------
Driver   |Process that contains the Spark Context
Executor |Process that executes one or more Spark tasks
Master   |Process which manages applications across the cluster
-        |E.g. Spark Master
Worker   |Process which manages executors on a particular worker node
-        |E.g. Spark Worker


## Spark Terminology

Term                   |Meaning
----                   |-------
RDD                    |*Resilient Distributed Dataset* or a distributed sequence of records
Spark Job              |Sequence of transformations on data with a final action
Spark Application      |Sequence of Spark jobs and other code
Transformation         |Spark operation that produces an RDD
Action                 |Spark operation that produces a local object


- A Spark job consists of a series of transformations followed by an
  action.

- It pushes the data to the cluster, all computation happens on the
  *executors*, then the result is sent back to the driver.

## Pop Quiz

<details><summary>
what is RDD? 
`sc.parallelize(Range(0,10)).filter(x => x % 2 == 0).collect`
</summary>

1. Resilient Distributed Dataset

</details>

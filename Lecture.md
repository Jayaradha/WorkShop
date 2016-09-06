<img src="http://spark.apache.org/images/spark-logo-trademark.png">

# Apache Spark

## Spark Intro

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

By the end of this lecture, you will be able to:

- Create RDDs to distribute data across a cluster

- Use the Spark shell to compose and execute Spark commands

- Use Spark to analyze stock market data

## Spark Version History

Date               |Version        |Changes
----               |-------        |-------
May 30, 2014       |Spark 1.0.0    |APIs stabilized 
September 11, 2014 |Spark 1.1.0    |New functions in MLlib, Spark SQL
December 18, 2014  |Spark 1.2.0    |Python Streaming API and better streaming fault tolerance
March 13, 2015     |Spark 1.3.0    |DataFrame API, Kafka integration in Streaming
April 17, 2015     |Spark 1.3.1    |Bug fixes, minor changes

## Matei Zaharia

<img style="width:50%" src="https://cs.stanford.edu/~matei/matei5.jpg">

## Essense of Spark

What is the basic idea of Spark?

- Spark takes the Map-Reduce paradigm and changes it in some critical
  ways.

- Instead of writing single Map-Reduce jobs a Spark job consists of a
  series of map and reduce functions. 
  
- However, the intermediate data is kept in memory instead of being
  written to disk or written to HDFS.

## Pop Quiz

<details><summary>
Since Spark keeps intermediate data in memory to get speed, what
does it make us give up? Where's the catch?
</summary>

1. Spark does a trade-off between memory and performance.

2. While Spark apps are faster, they also consume more memory.

3. Spark outshines Map-Reduce in iterative algorithms where the
   overhead of saving the results of each step to HDFS slows down
   Map-Reduce.

4. For non-iterative algorithms Spark is comparable to Map-Reduce.
</details>

## Spark Logging

How can I make Spark logging less verbose?

- By default Spark logs messages at the `INFO` level.

- Here are the steps to make it only print out warnings and errors.

```sh
cd $SPARK_HOME/conf
cp log4j.properties.template log4j.properties
```

- Edit `log4j.properties`.

- Replace `rootCategory=INFO` with `rootCategory=WARN`.

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

## Spark Job

Flip a coin 100 times. What fraction of the time do you get heads?

- Initialize Spark.

    ```sh
    spark-shell
    ```

- Import random.

    ```scala
    import scala.util.Random
    Random.nextInt(2)
    val flips = 1000000
    val heads = sc.parallelize(0 until flips).
      map(x => Random.nextDouble).
      filter(x => x <= 0.50).
      count
    val ratio = heads.toDouble / flips.toDouble
    println(heads)
    println(ratio)
    ```

## Notes

- `sc.parallelize` creates an RDD.

- `map` and `filter` are *transformations*.

- They create new RDDs from existing RDDs.

- `count` is an *action* and brings the data from the RDDs back to the
  driver.

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
In this Spark job what is the transformation is what is the action? 
`sc.parallelize(Range(0,10)).filter(x => x % 2 == 0).collect`
</summary>

1. `filter` is the transformation.

2. `collect` is the action.
</details>

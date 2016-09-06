## Part 0: Spark Installation

Follow these steps for installing Spark on your laptop.

1. Go to this [link](http://spark.apache.org/downloads.html). 

2. *Choose a Spark release: 1.5.2*

3. *Choose a package type: Pre-built for Hadoop 2.6 and later*. 

4. Make sure you are downloading the binary version, not the source
   version.

5. Download the tgz package.

6. Do **not** download the latest version. It has bugs we will talk about.

7. Make sure you are downloading the binary version, not the source
   version.

8. Unzip the file and place it at your home directory

9. Include the following lines in your `~/.bash_profile` file on Mac
   (without the brackets).

   ```
   export SPARK_HOME=[FULL-PATH-TO-SPARK-FOLDER]
   export PATH=[FULL-PATH-TO-SPARK-FOLDER]/bin:$PATH
   ```

10. Open a new terminal window.

11. Start the Spark shell using `spark-shell`.

12. If Spark throws errors about Java you might need to download the
    newest version of the
    [JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
 
    
## Part 1: RDD and Spark Basics

Lets get familiar with the basics of Spark. We will be using Spark in
local mode. 

1. Spark keeps your data in **Resilient Distributed Datasets (RDDs)**.
   **An RDD is a collection of data partitioned across machines**.
   Each group of records that is processed by a single thread (*task*) on a
   particular machine on a single machine is called a *partition*.

   Using RDDs Spark can process your data in parallel across
   the cluster. 
   
   You can create an RDD from a list, from a file or from an existing
   RDD.
   
   Lets create an RDD from a list.
   
   ```scala
   val listRdd = sc.parallelize(1 to 3)
   ```

3. RDDs are lazy so they do not load the data from disk unless it is
   needed. Each RDD knows what it has to do when it is asked to
   produce data. In addition it also has a pointer to its parent RDD
   or a pointer to a file or a pointer to an in-memory list.

   When you use `take()` or `first()` to inspect an RDD does it load
   the entire file or just the partitions it needs to produce the
   results? Exactly. It just loads the partitions it needs.
 
   ```scala
   fileRdd.first   // Views the first entry
   fileRdd.take(2) // Views the first two entries
   ```
    
4. If you want to get all the data from the partitions to be sent back
   to the driver you can do that using `collect()`. However, if your
   dataset is large this will kill the driver. Only do this when you
   are developing with a small test dataset.
   
   ```scala
   fileRdd.collect
   listRdd.collect
   ```

## Part 2: Transformations and Actions

Use
<http://real-chart.finance.yahoo.com/table.csv?s=CSCO&g=d&ignore=.csv>
to download the most recent stock prices of CSCO, and save it to
`csco.csv`.

Here are some useful functions for doing this.

```scala
// Grab URL contents
def getUrl(url:String):String = 
  scala.io.Source.fromURL(url).mkString

// Write file
def fileWrite(path:String,contents:String) = {
  import java.io.{PrintWriter,File}
  val writer = new PrintWriter(new File(path))
  writer.write(contents)
  writer.close
}

val symbol = "CSCO"
val baseUrl = "http://real-chart.finance.yahoo.com"
val url = s"${baseUrl}/table.csv?s=${symbol}&g=d&ignore=.csv"
val csv = getUrl(url)
fileWrite(s"${symbol}.csv", csv)
```

Next, use Spark to answer the following questions. Note, you already
have the file downloaded into `CSCOA.csv`. So your code should not
download it repeatedly. Just use the local copy that you downloaded.

Q: How many records are there in this CSV?

Q: Find the min, max Stockprice.

Q: Find the dates of the 3 highest adjusted close prices.

Q: Find the date of the 3 lowest adjusted close prices.

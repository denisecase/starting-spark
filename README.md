# setup-spark

> Set up Spark on Windows

## Set up Tools

- [Basic Setup for Big Data](https://github.com/denisecase/basic-setup-for-bigdata)

Questions? See [Big Data Developers](https://github.com/denisecase/big-data-developers)

## Install Spark

Spark is written in Scala (a new language for the JVM), but you can interact with it using Scala - or Python. 

1. Read: <https://spark.apache.org/>
1. Download: <https://spark.apache.org/downloads.html>
1. Extract to C:\ using 7-zip (extract to the recommended folder  name), then extract the tar file and verify your path.
1. Prereqs: We already have JDK 8, winutils.exe, Hadoop (and HDFS), and we've learned how to set system environment variables for HADOOP and JAVA. 
1. Set System Environment Variables:
    - SPARK_HOME = C:\<your spark path> (e.g., C:\spark-3.0.1-bin-hadoop2.7)
    - Path - add %SPARK_HOME%\bin
1. In PS as Admin, run spark-shell to launch Spark with Scala (you should get a scala prompt)
1. In a browser, open <http://localhost:4040/> to see the Spark shell web UI
1. Exit spark shell with CTRL-D (for "done")
1. In PS as Admin, run pyspark to launch Spark with Python.  You should get a Python prompt >>>.
1. Quit a Python window with the quit() function. 

## Code

Create a new RDD from an existing file. Use common textFile functions.

Implement word count. 

- map - one in to one out
- reduce - all in, one out
- filter - some in to some or less or zero out
- flatMap - one in to many out
- reduceByKey - many in, one out 

```scala
val textFile = sc.textFile("README.md")
textFile.count()
textFile.first()
val linesWithSpark = textFile.filter(line => line.contains("Spark"))
val ct = textFile.filter(line => line.contains("Spark")).count()
```

Find the line with the most words. All data in -> one out. 

1. First, map each line to words and get the size. 
1. Then, reduce all sizes to one max value. 

Which functions should we use?  Which version do you prefer?

```scala
textFile.map(line => line.split(" ").size).reduce((a, b) => if (a > b) a else b)
textFile.map(line => line.split(" ").size).reduce((a, b) => Math.max(a, b))

val maxWords = textFile.map(line => line.split(" ").size).reduce((a, b) => if (a > b) a else b)
```

## MapReduce in Spark

1. First, flatMap each line to words. 
1. Then, map each word to a count (one). 
1. Then, reduceByKey to aggregate a total for each key. 
1. After transformations, use an action (e.g. collect) to collect the results to our shell. 

Can you modify to get max by key? 

Can you modify to get max by key? 

```scala
val wordCounts = textFile.flatMap(line => line.split(" ")).map(word => (word, 1)).reduceByKey((a, b) => a + b)
wordCounts.collect()
```

## Terms

- Resilient Distributed Dataset (RDD) - a distributed collection of items
- RDD actions (return values)
- RDD transformations (return pointers to new RDD)
- RDD transformations list ([link](https://spark.apache.org/docs/latest/rdd-programming-guide.html#transformations))


## Resources

- Try Spark Quick Start at <https://spark.apache.org/docs/2.1.0/quick-start.html>
- Try Spark examples at <https://spark.apache.org/examples.html>
- Try Spark online with Databricks at <https://databricks.com/>


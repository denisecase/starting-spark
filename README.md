# setup-spark

> Set up Spark on Windows

## Suggested: Set up Tools

- [Basic Setup for Big Data](https://github.com/denisecase/basic-setup-for-bigdata)

## Install Prerequisities (included above)

- JDK
- Python (Anaconda)

```PowerShell
choco install openjdk -y
choco install 7zip -y
choco install anaconda3 --params="/AddToPath:1" -y
```

Verify:

```PowerShell
java --version
python --version
```

## Install Spark

Spark is written in Scala (a new language for the JVM), but you can interact with it using Scala - or Python. 

1. Read: <https://spark.apache.org/>
2. Download Spark: <https://spark.apache.org/downloads.html> (e.g., to Downloads folder).
3. Use 7zip to extract, extract again. 
4. Move so you have C:\spark-3.1.1-bin-hadoop2.7\bin
5. Download winutils from https://github.com/cdarlint/winutils/tree/master/hadoop-2.7.7/bin into spark bin folder.
6. Optional: Create C:\tmp\hive and in your new bin folder, open Command Window as Admin and run winutils.exe chmod -R 777 C:\tmp\hive
7. Set System Environment Variables:
    - SPARK_HOME = C:\spark-3.1.1-bin-hadoop2.7
    - HADOOP_HOME = C:\spark-3.1.1-bin-hadoop2.7
    - Path - add %SPARK_HOME%\bin

## Verify Spark using Scala

1. In PowerShell as Admin, run ```spark-shell``` to launch Spark with Scala (you should get a scala prompt)
2. In a browser, open <http://localhost:4040/> to see the Spark shell web UI
3. Exit spark shell with CTRL-D (for "done")

## Verify Spark using Python

1. In PS as Admin, run ```pyspark``` to launch Spark with Python.  You should get a Python prompt >>>.
2. Quit a Python window with the quit() function. 

## Warnings

If you see a WARN about trying to compute pageszie, just hit ENTER. This command works in Linux, but not in Windows. 

---

## Experiment with Spark & Python

- [First Steps With PySpark and Big Data Processing
by Luke Lee](https://realpython.com/pyspark-intro/)

---

## Experiment with Spark & Scala

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

Find the most words in a single line. All data in -> one out. 

1. First, map each line to words and get the size. 
1. Then, reduce all sizes to one max value. 

Which functions should we use?  Which version do you prefer?

```scala
textFile.map(line => line.split(" ").size).reduce((a, b) => if (a > b) a else b)
textFile.map(line => line.split(" ").size).reduce((a, b) => Math.max(a, b))

val maxWords = textFile.map(line => line.split(" ").size).reduce((a, b) => if (a > b) a else b)
```

### MapReduce in Spark & Scala

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

---

## If needed, install PySpark (Anaconda)

- /bin includes pyspark. If you need another, follow instructions at https://anaconda.org/conda-forge/pyspark. Open Anaconda prompt and run:

```Anaconda
conda install -c conda-forge pyspark
```

## Terms

- Resilient Distributed Dataset (RDD) - a distributed collection of items
- RDD actions (return values)
- RDD transformations (return pointers to new RDD)
- RDD transformations list ([link](https://spark.apache.org/docs/latest/rdd-programming-guide.html#transformations))

- Application - custome driver program + executors
- SparkSession - access to Spark (interactive or created in an app)
- Job - parallel computation with tasks spawned by Spark action (e.g., save(), collect())
- Stage - set of tasks
- Task - unit of work sent to Spark executor

- Cluster manager (built-in, YARN, Mesos, or Kubernetes)
- Spark executor (usually one per worker node)
- Deployment modes (local, standalone, YARN, etc.)


## Resources

- Try Spark Quick Start at <https://spark.apache.org/docs/2.1.0/quick-start.html>
- Try Spark examples at <https://spark.apache.org/examples.html>
- Try Spark online with Databricks at <https://databricks.com/>
- Free Spark Databricks Community Edition at <https://community.cloud.databricks.com/login.html>
- Databricks Datasets at <https://docs.databricks.com/data/databricks-datasets.html>
- "/databricks-datasets/Rdatasets/data-001/csv/ggplot2/diamonds.csv"
- [Learning Spark v2 (includes Spark 3)](https://github.com/databricks/LearningSparkV2)

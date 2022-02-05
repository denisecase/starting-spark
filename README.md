# Starting Spark

> Getting started with Spark big data analytics engine
> 

## Spark in the Cloud

- Used to recommend [Free Databricks Community Edition](https://community.cloud.databricks.com/login.html). 
- Now it looks like they require a credit card after 14 days. 
- Looking for a truly free way to experiment for students. 

---

## Preferred: Local Installation on Windows

### First, set up Windows machine tools

- [Basic Setup for Big Data](https://github.com/denisecase/basic-setup-for-bigdata)

### Verify Prerequisities are installed (some included above)

- JDK 8 or better
- Python (includied with Miniconda or Anaconda) 

```PowerShell
choco install 7zip.install -y
choco install openjdk -y
choco install miniconda3 --params="/AddToPath:1" -y
```

Verify:

```PowerShell
java --version
python --version
```

### Install Spark locally

Spark is written in Scala (a new language for the JVM), but you can interact with it using Scala - or Python. 

1. Read: <https://spark.apache.org/>
2. Download Spark: <https://spark.apache.org/downloads.html> (e.g., to Downloads folder).
3. Use 7zip to extract, extract again. 
4. Move so you have C:\spark-3.2.1-bin-hadoop3.2\bin
5. Download the correct version of winutils for the associated Spark from <https://github.com/cdarlint/winutils/> into the spark bin folder. 
6. Recommended: Star the repo to say thanks for providing this helpful service. 
7. Set System Environment Variables - be sure to match the version you actually download.
    - SPARK_HOME = C:\spark-3.2.1-bin-hadoop3.2
    - HADOOP_HOME =C:\spark-3.2.1-bin-hadoop3.2
    - Path - add %SPARK_HOME%\bin

### Verify Spark using Scala

1. In PowerShell as Admin, run ```spark-shell``` to launch Spark with Scala (you should get a scala prompt). Be patient - it may take a while. 
2. In a browser, open <http://localhost:4040/> to see the Spark shell web UI
3. Exit spark shell with CTRL-D (for "done")

### Verify PySpark (install if needed)

1. The new /bin includes PySpark. 
2. In PS as Admin, run ```pyspark``` to launch Spark with Python.  You should get a Python prompt >>>.
3. Quit a Python window with the quit() function. 
4. If you don't see a version after the command above, follow instructions at https://anaconda.org/conda-forge/pyspark. Open Anaconda prompt and run:

```Anaconda
conda install -c conda-forge pyspark
```

### Warnings

If you see a WARN about trying to compute pageszie, just hit ENTER. This command works in Linux, but not in Windows. 

---

## Experiment with Spark & Python

- [First Steps With PySpark and Big Data Processing
by Luke Lee](https://realpython.com/pyspark-intro/)

---

## Experiment with Spark & Java

Build custom apps using Java. 

See <https://github.com/denisecase/spark-maven-java-challenge>

```PowerShell
java -cp target/spark-challenge-1.0.0-jar-with-dependencies.jar edu.nwmissouri.isl.App "data.txt"
```

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
- [Intro to SparkNLP](https://towardsdatascience.com/introduction-to-spark-nlp-foundations-and-basic-components-part-i-c83b7629ed59)

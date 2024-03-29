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

- JDK 11 - 17 may not work?
- Python (includied with Miniconda or Anaconda) 

```PowerShell
choco install 7zip.install -y
choco install openjdk11 -y
choco install miniconda3 --params="/AddToPath:1" -y
refreshenv
```

Verify:

```PowerShell
java --version
python --version
```

Important: Make sure there is only one Java in your path - both system and user path. 

### Install Spark locally

Spark is written in Scala (a new language for the JVM), but you can interact with it using Scala - or Python. 

1. Read: <https://spark.apache.org/>
2. View Spark Downloads: <https://spark.apache.org/downloads.html> (e.g., to Downloads folder). 
3. NEW: REVERT BACK TO "3.1.2 (Jun 01 2021)" in the dropdown box before downloading. 
4. Use 7zip to extract, extract again. 
5. Move so you have C:\spark-3.1.2-bin-hadoop3.2\bin - MUST BE THE EARLIER 3.1.2 (not 3.2.1).
6. View the different versions of winutils for Spark from <https://github.com/cdarlint/winutils/> and download into your spark bin folder. 
7. UPDATE: Download 3.2.0 winutils.exe from https://github.com/cdarlint/winutils/blob/master/hadoop-3.2.0/bin/winutils.exe into spark bin
8. UPDATE: Download 3.2.0 hadoop.dll from https://github.com/cdarlint/winutils/blob/master/hadoop-3.2.0/bin/hadoop.dll into spark bin
9. Recommended: Star the repo to say thanks for providing this helpful service. 
10. Set System Environment Variables - be sure to match the version you actually download.
    - JAVA_HOME = C:\Program Files\OpenJDK\openjdk-11.0.13_8
    - HADOOP_HOME = C:\spark-3.1.2-bin-hadoop3.2
    - SPARK_HOME =  C:\spark-3.1.2-bin-hadoop3.2
    - Path - add %SPARK_HOME%\bin
    - Path - add %HADOOP_HOME%\bin
    - Path - verify there is exactly one path to java bin in both user and system paths. 

### Verify Spark using Scala

1. In PowerShell as Admin, run ```spark-shell``` to launch Spark with Scala (you should get a scala prompt). Be patient - it may take a while. 
2. In a browser, open <http://localhost:4040/> to see the Spark shell web UI
3. Exit spark shell with CTRL-D (for "done")

### Verify PySpark (install if needed)

1. The new /bin includes PySpark. 
2. In PS as Admin, run ```pyspark``` to launch Spark with Python.  You should get a Python prompt >>>.
3. Quit a Python window with the quit() function. 
4. SHOULD NOT BE NEEDED: If you DON'T see a version after the command above, follow instructions at https://anaconda.org/conda-forge/pyspark. Open Anaconda prompt and run: `conda install -c conda-forge pyspark`

-----

### Run a PySpark Script

Try to run an example, open PS as Admin in the following location and run this command. 

C:\spark-3.1.2-bin-hadoop3.2>  `bin/spark-submit examples/src/main/python/wordcount.py README.md`

Required: 

Enter miniconda directory and create a copy of python.exe renamed as python3.exe.  Spark requires the python3 command name.

Solution provided by:

- Marci DeVaughan
- Junior – Computer Science
- Campus Technology Support Assistant
- President – Northwest Gaming
- She/Her

Required: Copy log4j template to properties

- Copy conf/log4j.properties.template to conf/log4j.properties

Option: Manage App Execution Aliases

- Start / type “Manage App Execution Aliases” / Manage “Python” options

-----

## Spark Scala Examples

Read the example code. What examples are avaiable? What arguments are needed?

```
.\bin\run-example SparkPi
.\bin\run-example JavaWordCount
.\bin\run-example JavaWordCount README.md
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

## Python: Conda Environments (Do not recommend pip as we do not include instructions for pip virtual environments)

- [pip vs conda](https://pythonspeed.com/articles/conda-vs-pip/)
- [SO pip / conda](https://stackoverflow.com/questions/20994716/what-is-the-difference-between-pip-and-conda?rq=1)
- [MManage your Python Virtual Environment with Conda](https://towardsdatascience.com/manage-your-python-virtual-environment-with-conda)
- When we install Python with Anaconda or Miniconda, it automatically creates the Python 'base' environment. 
- We can create additional conda environments as needed. 
- The current environment is used anytime we invoke Python, until we explicitly change the enviroment. 
- Conda and pip handle environments differently. Understand their use before mixing. 
- Conda may show a warning, but if installation instructions were followed, there will not be an error. 
- Optional: To explore more about what conda does for us and where the information is stored, try the following. 
- Optional: Explore Conda Environments with `conda info` for current or `conda env list` to see all. 
- Optional: Review conda env information at (change to your username):

```
C:\Users\dcase\miniconda3\envs
C:\Users\dcase\.conda\envs
C:\Users\dcase\AppData\Local\conda\conda\envs
```

## A Few Terms

- Resilient Distributed Dataset (RDD) - a distributed collection of items
- DataFrame, DataSet - abstractions on top of RDD
- Operations:
   - actions (required to emit values)
   - transformations (return pointers to new RDD)
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

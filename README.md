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






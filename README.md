# setup-spark

> Set up Spark and related tools on Windows

## Set up Tools

- [Set up Basic Tools for Windows Development](https://github.com/denisecase/basic-tools-for-webdev)

## Choco Installs

Open Powershell as Administrator and run the following commands to install a variety of big data tools on Windows. 

```Powershell

choco install jdk8 -y
choco install solr -y
choco install hadoop -y
choco install sbt -y
choco install python -y
choco install flink -y

```

Periodically, you may need the following command to update the environment.

```Powershell
refreshenv
```

Update your packages as needed.

```Powershell
choco upgrade all -y
```

## Install Spark

- [Download Apache Spark](https://spark.apache.org/downloads.html)
- Download the most recent version. Verify integrity. Use 7-zip to expand (including the docs)
- Move the spark_* folder to c:\

## Install Kafka (includes simple Zookeeper)

- [Kafka Quickstart](https://kafka.apache.org/quickstart)
- Download the most recent version. Verify integrity. Use 7-zip to expand (including the docs)
- Move the kafka_* folder to c:\

## Set/Verify Environment Variables

- Set and verify environment variables
- Update and verify path





# NLP Details

## References

- PySpark reference at https://spark.apache.org/docs/2.1.0/api/python/pyspark.html
- Python RE reference at https://docs.python.org/3/library/re.html
- re.sub(pattern, repl, string, count=0, flags=0)

## Processes

- Ingesting (to strings)
- Cleaning 
- Processing 
  - flatMap each line to many tokens (messy words)
  - map each messy token to clean word tokens with RE
  - fliter each clean word token to word not in stopwords (meaningful words)
  - map to tuples (IKVPairs)
  - reduceByKey to get counts
- Charting

## Ingesting 

- by url
- with triple quotes

by URL example
```Python
# fetch by URL
import urllib.request
stringInURL = "https://raw.githubusercontent.com/denisecase/starting-spark/master/data/nod.txt"
stringTmp = "/tmp/nod.txt"
stringDbfs = "/data/nod.txt"
filePrefix = "file:"
dbfsPrefix = "dbfs:"
urllib.request.urlretrieve(stringInURL, stringTmp)

# move into Databricks filesystem - requires prefixes
dbutils.fs.mv(filePrefix + stringTmp, dbfsPrefix + stringDbfs)

# create initial RDD using Spark Context
inRDD = sc.textFile(dbfsPrefix + stringDbfs)
```

Triple quotes example
```Python
# data from triple quotes
poem = """
Wynken, Blynken, and Nod - BY EUGENE FIELD
Wynken, Blynken, and Nod one night
    Sailed off in a wooden shoe--
Sailed on a river of crystal light,
    Into a sea of dew.
"""

# convert to list of strings (looks like array)
listIn = [poem]

# create initial RDD of strings using PySpark Spark Context
inRDD = sc.parallelize(listIn)
```

## Cleaning

- NLP Stopword Removal (filter not in stopwords)
- Remove non-letters (map re.sub)

Stopword example
```Python
# Fetch stopwords
from pyspark.ml.feature import StopWordsRemover
remover = StopWordsRemover()
stopwords = remover.getStopWords()
print(stopwords)

# Filter out stopwords
nonStopRDD = allWordsRDD.filter(lambda word: word not in stopwords)
```

Non-letters example
```Python
#remove non-letters with RegEx
import re
cleanRDD = allTokensRDD.map(lambda w: re.sub(r'','',w))
```


## Processing

- flat map each line of text to just a word (one to many) 
    - lowercase
    - strip off whitespace
    - split it by our delimiter (, tab, space)
- map
- filter
- reduce

Don't forget to collect results back into regular Python.

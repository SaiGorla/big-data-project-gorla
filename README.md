# Big-Data-Project-Gorla

## Author :: Sai Rohith Gorla

## About : 
- The Big data current project is about the processing text using Databricks Community Edition and PySpark.Here we process text data with Spark and Python. We are going to implement word count (find the top ~10 most widely used words) in the text data using the text from the URL. In this process, we include key steps. There are to Force all words to lowercase, Remove all non-letter characters from each word token and then filter out common stop words.

## Tools and Skills Used :

- Databricks Cloud Environment
- Spark Processing Engine
- PySpark API

## Text Resource :
- https://www.gutenberg.org/files/1342/1342-0.txt

## Commands :

### Data Gathering :
- First, We will use the library called "urllib.request" to pull the data into the notebook from the URL we have mentioned. Then, once the book has been brought in, we will save the text file to /tmp/ and name it Rohith.txt. Now, using the "dbutils.fs.mv" method, we will move our saved text file from the tmp folder to the dbfs internal folder and then the path you want to save(data).Our file will be saved in the data folder. The Final thing is we need to tranfer our file into spark. As a result, we'll be converting our data into an RDD(rohithRDD).

```
import urllib.request
urllib.request.urlretrieve("https://www.gutenberg.org/files/1342/1342-0.txt" , "/tmp/Rohith.txt")
```
```
dbutils.fs.mv("file:/tmp/Rohith.txt","dbfs:/data/Rohith.txt")
```
```
rohithRDD = sc.textFile("dbfs:/data/Rohith.txt")
```

### Data Cleaning :
- The text is currently in book form with capitalization, punctuation, sentences, and stopwords. Stopwords are just words that make a sentence flow better but don't add anything to the sentence. For example "a", To get the word count the first step is to flatmap and get rid of capitalization and spaces. Flatmapping is just breaking up the sentences into words. The next step is to move all the punctuation, this can be done by using the regular expression,by importing the re. The last step is to remove the stopwords by using the import the library StopWordsRemover from pyspark. Then , now we will filter out the words. now, remove all the empty sets using filter keyword.

```
wordRDD=rohithRDD.flatMap(lambda line : line.lower().strip().split(" "))
```
```
import re
tokenCleanerRDD = wordRDD.map(lambda w: re.sub(r'[^a-zA-Z]','',w))
```
```
from pyspark.ml.feature import StopWordsRemover
remover =StopWordsRemover()
stopwords = remover.getStopWords()
cleanedwordRDD=tokenCleanerRDD.filter(lambda w: w not in stopwords)
```
```
cleanedwordRDD=tokenCleanerRDD.filter(lambda w: w not in stopwords)
```
### Data Processing :
- For Data Processing, the first step is to Map words to key-value pairs.Here, we need to change the words into the form(word,1), then we count the occurancy of the word.Next step is by using the ReduceByKey, we need to remove the duplicate words occured in the text and add the first count words.The last step is to return to the python from the spark using Collect( ) function.

```
IKVPairsRDD= cleanedwordRDD.map(lambda word: (word,1))
```
```
wordsCountRDD = IKVPairsRDD.reduceByKey(lambda acc, value: acc+value)
```
```
results = wordsCountRDD.collect()
```

### Charting :
- The final processing is to display the final output data and then  visualize our performance using MatPlotLib, and Seaborn.Viewing a list of words is fine but it is better to graph data. To create a graph we will use the library mathplotlib. Here is a helpful stack overflow on how to graph a list of tuple using one side of the x axis and the other side for y axis.

```
results.sort(key=lambda x:x[1])
results.reverse()
print(results[:12])
```
```
mostCommon=results[1:14]
word,count = zip(*mostCommon)
import matplotlib.pyplot as plt
fig = plt.figure()
plt.stackplot(word,count, color='crimson')
plt.xlabel("No of times used")
plt.ylabel("Most repeated words")
plt.title("Most used words in the File")
plt.show()
```
![](https://github.com/SaiGorla/big-data-project-gorla/blob/main/Data%20Visualized.PNG)


## References :
- [MatplotLib](https://matplotlib.org/stable/tutorials/introductory/sample_plots.html)

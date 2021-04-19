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



## References :
- [MatplotLib](https://matplotlib.org/stable/tutorials/introductory/sample_plots.html)

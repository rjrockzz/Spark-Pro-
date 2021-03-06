**_Machine Learning in Spark_**

Machine learning algorithms in MLlib require that data is represented as numerical values. 

* **Preparing dataset**

```scala
import org.apache.spark.sql.functions.date_format

val preppedDataFrame = staticDataFrame
                      .na.fill(0) //Fill/Replace Null values with 0
                      .withColumn("day_of_week", date_format($"InvoiceDate", "EEEE"))  
                      .coalesce(5)
```
preppedDataFrame: org.apache.spark.sql.Dataset[org.apache.spark.sql.Row] = [InvoiceNo: string, StockCode: string ... 7 more fields]

```
scala> preppedDataFrame.show(5)
+---------+---------+--------------------+--------+-------------------+---------+----------+--------------+-----------+
|InvoiceNo|StockCode|         Description|Quantity|        InvoiceDate|UnitPrice|CustomerID|       Country|day_of_week|
+---------+---------+--------------------+--------+-------------------+---------+----------+--------------+-----------+
|   580538|    23084|  RABBIT NIGHT LIGHT|      48|2011-12-05 08:38:00|     1.79|   14075.0|United Kingdom|     Monday|
|   580538|    23077| DOUGHNUT LIP GLOSS |      20|2011-12-05 08:38:00|     1.25|   14075.0|United Kingdom|     Monday|
|   580538|    22906|12 MESSAGE CARDS ...|      24|2011-12-05 08:38:00|     1.65|   14075.0|United Kingdom|     Monday|
|   580538|    21914|BLUE HARMONICA IN...|      24|2011-12-05 08:38:00|     1.25|   14075.0|United Kingdom|     Monday|
|   580538|    22467|   GUMBALL COAT RACK|       6|2011-12-05 08:38:00|     2.55|   14075.0|United Kingdom|     Monday|
+---------+---------+--------------------+--------+-------------------+---------+----------+--------------+-----------+
only showing top 5 rows
```
* **Splitting into testing and training datasets**

```scala
val train = preppedDataFrame.where("InvoiceDate < '2011-07-01'")

val test = preppedDataFrame.where("InvoiceDate > '2011-07-01'")
```
* **Turn our days of weeks into corresponding numerical values. For example, Spark might represent Saturday as 6, and Monday as 1.**
```scala
import org.apache.spark.ml.feature.StringIndexer

val indexer = new StringIndexer()

indexer.setInputCol("day_of_week").setOutputCol("day_of_week_index")
```

* **[OneHotEncoder](https://stackoverflow.com/questions/42295001/how-to-interpret-results-of-spark-onehotencoder)**
```scala
import org.apache.spark.ml.feature.OneHotEncoder

val encoder = new OneHotEncoder()

encoder.setInputCol("day_of_week_index").setOutputCol("day_of_week_encoded")
```
* **Each of these OneHotEncoder values will result in a set of columns that we will “assemble” into a vector. All machine learning algorithms in Spark take as input a Vector type, which must be a set of numerical value**

```scala
import org.apache.spark.ml.feature.VectorAssembler

val Vector = new VectorAssembler()

Vector.setInputCols(Array("UnitPrice","Quantity","day_of_week_encoded")).setOutputCol("features")
```
* **We’ll set this up into a pipeline so that any future data we need to transform can go through the exact same process**

```scala
scala> import org.apache.spark.ml.Pipeline
import org.apache.spark.ml.Pipeline

scala> val transPipeline = new Pipeline()
transPipeline: org.apache.spark.ml.Pipeline = pipeline_cdd9f7c4b2c3

scala> transPipeline.setStages(Array(indexer, encoder, Vector))
res20: transPipeline.type = pipeline_cdd9f7c4b2c3
```
* **Preparing for training is a two-step process:**

  * We first need to fit our transformers to this dataset.StringIndexer needs to know how many unique values there are to be indexed.       After those exist, encoding is easy but Spark must look at all the distinct values in the column to be indexed in order to store         those values later on:
  
  ```scala
  scala> val fitPipe = transPipeline.fit(train)
  fitPipe: org.apache.spark.ml.PipelineModel = pipeline_cdd9f7c4b2c3
  ```
  
  * After we fit the training data, we are ready to take that fitted pipeline and use it to transform all of our data in a consistent       and repeatable way:
    
   ```scala
   scala> val transTrain = fitPipe.transform(train)
   transTrain: org.apache.spark.sql.DataFrame = [InvoiceNo: string, StockCode: string ... 10 more fields]
   ```

* **Finally Training the model!**

```scala

scala> import org.apache.spark.ml.clustering.KMeans
import org.apache.spark.ml.clustering.KMeans

scala> val kmeans = new KMeans()
kmeans: org.apache.spark.ml.clustering.KMeans = kmeans_e3554ea52dd0

scala> kmeans.setK(20).setSeed(1L)
res22: kmeans.type = kmeans_e3554ea52dd0

scala> val kmModel = kmeans.fit(transTrain)

scala> kmModel.computeCost(transTrain)
warning: there was one deprecation warning; re-run with -deprecation for details
res23: Double = 8.455373996537484E7

scala> val transTest = fitPipe.transform(test)
transTest: org.apache.spark.sql.DataFrame = [InvoiceNo: string, StockCode: string ... 10 more fields]

scala> kmModel.computeCost(transTest)
warning: there was one deprecation warning; re-run with -deprecation for details
res24: Double = 5.175070947222117E8
```

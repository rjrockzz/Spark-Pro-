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


**Structured Streaming**
<br>[Dataset](https://github.com/rjrockzz/Spark-The-Definitive-Guide/tree/master/data/retail-data/by-day)
<br>
*_[Window Function in Spark](https://databricks.com/blog/2015/07/15/introducing-window-functions-in-spark-sql.html)_*

<br>* **Loading several CSVs at once**
```scala
 val staticDataFrame = spark.read.format("csv")
                       .option("header","true")
                       .option("inferSchema","true")
                       .load("C://Users//RJ//Desktop//scalala//data//retail-data//by-day//*.csv")
```
staticDataFrame: org.apache.spark.sql.DataFrame = [InvoiceNo: string, StockCode: string ... 6 more fields]

* **Creating a view**
```scala
staticDataFrame.createOrReplaceTempView("retail")
```
* **Creating static schema from the given dataset.**
```scala
scala> val staticSchema = staticDataFrame.schema
```
staticSchema: org.apache.spark.sql.types.StructType = StructType(StructField(InvoiceNo,StringType,true), StructField(StockCode,StringType,true), StructField(Description,StringType,true), StructField(Quantity,IntegerType,true), StructField(InvoiceDate,TimestampType,true), StructField(UnitPrice,DoubleType,true), StructField(CustomerID,DoubleType,true), StructField(Country,StringType,true))

* **Grouping and Aggregating our time-series data**
```scala
staticDataFrame
.selectExpr("CustomerId","(UnitPrice * Quantity) as total_cost","InvoiceDate")
.groupBy(col("CustomerId"),window(col("InvoiceDate"),"1 day"))
.sum("total_cost")
.show(5)
```
```
+----------+--------------------+-----------------+
|CustomerId|              window|  sum(total_cost)|
+----------+--------------------+-----------------+
|   16057.0|[2011-12-05 05:30...|            -37.6|
|   14126.0|[2011-11-29 05:30...|643.6300000000001|
|   13500.0|[2011-11-16 05:30...|497.9700000000001|
|   17160.0|[2011-11-08 05:30...|516.8499999999999|
|   15608.0|[2011-11-11 05:30...|            122.4|
+----------+--------------------+-----------------+
only showing top 5 rows
```
* **Streaming at Business!**
```scala
val streaming = spark.readStream.schema(staticSchema)
.option("maxFilesPerTrigger",1)
.format("csv")
.option("header","true")
.load("C://Users//RJ//Desktop//scalala//data//retail-data//by-day//*.csv")
```
streaming: org.apache.spark.sql.DataFrame = [InvoiceNo: string, StockCode: string ... 6 more fields]

* **Check Streaming status**
```scala 
streaming.isStreaming
```
res3: Boolean = true

* **Previous DataFrame manipulation logic for summation in the process.**
```scala
val purchasePerHour = streaming
.selectExpr("CustomerId","(UnitPrice * Quantity) as total_cost", "InvoiceDate")
.groupBy($"CustomerId",window($"InvoiceDate","1 day"))
.sum("total_cost")
```
purchasePerHour: org.apache.spark.sql.DataFrame = [CustomerId: double, window: struct<start: timestamp, end: timestamp> ... 1 more field]

* **In-Memory Concept**

Streaming actions are a bit different from our conventional static action because we’re going to be populating data somewhere instead of just calling something like count (which doesn’t make any sense on a stream anyways). The action we will use will output to an in-memory table that we will update after each trigger. In this case, each trigger is based on an individual file (the read option that we set). Spark will mutate the data in the in-memory table such that we will always have the highest value as specified in our previous aggregation
```scala
purchasePerHour.writeStream
.format("memory") //Storing in-memory table
.queryName("customer") //name of in-memory table
.outputMode("complete") //all counts in table
.start()
```

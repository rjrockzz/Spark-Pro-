**Structured Streaming**
<br>[dataset](https://github.com/rjrockzz/Spark-The-Definitive-Guide/tree/master/data/retail-data/by-day)

* **Loading several CSVs at once**
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

* **Grouping and Aggregating out time-series data**
```scala
staticDataFrame
.selectExpr("CustomerId","(UnitPrice * Quantity) as total_cost","InvoiceDate")
.groupBy(col("CustomerId"),window(col("InvoiceDate"),"1 day"))
.sum("total_cost")
.show(5)
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


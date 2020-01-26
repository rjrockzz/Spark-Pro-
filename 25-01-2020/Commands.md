**Structured Streaming**
<br>[dataset](https://github.com/rjrockzz/Spark-The-Definitive-Guide/tree/master/data/retail-data/by-day)

* **Loading several CSVs at once**

 val staticDataFrame = spark.read.format("csv").option("header","true").option("inferSchema","true").load("C://Use
rs//RJ//Desktop//scalala//data//retail-data//by-day//*.csv")

staticDataFrame: org.apache.spark.sql.DataFrame = [InvoiceNo: string, StockCode: string ... 6 more fields]

* **Creating a view**

staticDataFrame.createOrReplaceTempView("retail")

* **Creating static schema from the given dataset.**

scala> val staticSchema = staticDataFrame.schema

staticSchema: org.apache.spark.sql.types.StructType = StructType(StructField(InvoiceNo,StringType,true), StructField(StockCode,StringType,true), StructField(Description,StringType,true), StructField(Quantity,IntegerType,true), StructField(InvoiceDate,TimestampType,true), StructField(UnitPrice,DoubleType,true), StructField(CustomerID,DoubleType,true), StructField(Country,StringType,true))


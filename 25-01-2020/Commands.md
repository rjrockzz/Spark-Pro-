**Structured Streaming**
[dataset](https://github.com/rjrockzz/Spark-The-Definitive-Guide/tree/master/data/retail-data/by-day)

* **Loading several CSVs at once**

 val staticDataFrame = spark.read.format("csv").option("header","true").option("inferSchema","true").load("C://Use
rs//RJ//Desktop//scalala//data//retail-data//by-day//*.csv")

staticDataFrame: org.apache.spark.sql.DataFrame = [InvoiceNo: string, StockCode: string ... 6 more fields]

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

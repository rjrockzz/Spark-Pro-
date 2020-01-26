**_Machine Learning in Spark_**

Machine learning algorithms in MLlib require that data is represented as numerical values. 

* **Preparing dataset**

```spark
import org.apache.spark.sql.functions.date_format

val preppedDataFrame = staticDataFrame
                      .na.fill(0)
                      .withColumn("day_of_week", date_format($"InvoiceDate", "EEEE"))  
                      .coalesce(5)
                      ```

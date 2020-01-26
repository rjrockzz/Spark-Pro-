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

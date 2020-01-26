* **SQL Queries**
```scala
val maxSQL = spark.sql("""SELECT DEST_COUNTRY_NAME, sum(count) as destination_total 
             FROM Deeps 
             GROUP BY DEST_COUNTRY_NAME 
             ORDER BY sum(count) 
             DESC LIMIT 5""")
```
* **Query Results**
```scala
maxSQL.show()
```
* **Queries - *_Dataframe Syntax_***
```scala
flight.groupBy("DEST_COUNTRY_NAME")
.sum("count")
.withColumnRenamed("sum(count)","dest_total")
.sort(desc("dest_total"))
.limit(5)
.show()
```

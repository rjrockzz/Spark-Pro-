* **Loading CSV data in spark**
```scala
val flight = spark.read.option("inferSchema","true").option("header","true").csv("C://Users//RJ//Desktop//data.csv")
```
* **Take out elements**
```scala
flight.take(4).explain()
```
* **Dataframe Conversion into a Table**
```scala
flight.createOrReplaceTempView("Views")
```
* **SQL Queries**
```scala
val sqlcmd = spark.sql("""SELECT DEST_COUNTRY_NAME, count(1) FROM Deeps GROUP BY DEST_COUNTRY_NAME""") 
```
* **SQL Aggregation**
```scala
val dfu = flight.groupBy("DEST_COUNTRY_NAME").count() 
```

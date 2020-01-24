* **Loading CSV data in spark**

val flight = spark.read.option("inferSchema","true").option("header","true").csv("C://Users//RJ//Desktop//data.csv")

* **Take out elements**

flight.take(4).explain()

* **Dataframe Conversion into a Table**

flight.createOrReplaceTempView("Views")

* **SQL Queries**

val sqlcmd = spark.sql("""SELECT DEST_COUNTRY_NAME, count(1) FROM Deeps GROUP BY DEST_COUNTRY_NAME""") 

* **SQL Aggregation**

val dfu = flight.groupBy("DEST_COUNTRY_NAME").count() 

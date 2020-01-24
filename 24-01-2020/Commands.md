* **SQL Queries**

val maxSQL = spark.sql("""SELECT DEST_COUNTRY_NAME, sum(count) as destination_total FROM Deeps GROUP BY DEST_COUNTRY_NAME ORDER BY sum(count) DESC LIMIT 5""")

* **Query Results**

maxSQL.show()

* **Queries - *_Dataframe Syntax_***

flight.groupBy("DEST_COUNTRY_NAME").sum("count").withColumnRenamed("sum(count)","dest_total").sort(desc("dest_tot
al")).limit(5).show()

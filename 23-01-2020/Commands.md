* **Loading csv data in spark**

val flight = spark.read.option("inferSchema","true").option("header","true").csv("C://Users//RJ//Desktop//data.csv")

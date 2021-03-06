**Dataframes**: *_Case_* classes in **scala** and *_JavaBean Pattern_* in **java.**

* **Case Class Scala**
```scala
case class Flight(DEST_COUNTRY_NAME: String,
     ORIGIN_COUNTRY_NAME: String,
     count: BigInt)
```     
     defined class Flight
     
* **Read as Parquet File**
```scala
val flightsDF = spark.read.parquet("C://Users//RJ//Desktop//flight.parquet")
```
flightsDF: org.apache.spark.sql.DataFrame = [DEST_COUNTRY_NAME: string, ORIGIN_COUNTRY_NAME: string ... 1 more field]

* **Converting into Dataset File**
```scala
val flights = flightsDF.as[Flight]
```
flights: org.apache.spark.sql.Dataset[Flight] = [DEST_COUNTRY_NAME: string, ORIGIN_COUNTRY_NAME: string ... 1 more field]

* **Manipulations on Dataset file**
```scala
scala> flights
       .filter(flight_row => flight_row.ORIGIN_COUNTRY_NAME != "Canada")
       .map(flight_row => flight_row)
       .take(5)
```
res0: Array[Flight] = Array(Flight(United States,Romania,1), Flight(United States,Ireland,264), Flight(United States,India,69), Flight(Egypt,United States,24), Flight(Equatorial Guinea,United States,1))

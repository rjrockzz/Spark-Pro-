**Dataframes**: *_Case_* classes in **scala** and *_JavaBean Pattern_* in **java.**

* **Case Class Scala**

case class Flight(DEST_COUNTRY_NAME: String,
     | ORIGIN_COUNTRY_NAME: String,
     | count: BigInt)
     
* **Read as Parquet File**

val flightsDF = spark.read.parquet("C://Users//RJ//Desktop//flight.parquet")

* **Converting into Dataset File**

val flights = flightsDF.as[Flight]

* **Manipulations on Dataset file**

scala> flights.filter(flight_row => flight_row.ORIGIN_COUNTRY_NAME != "Canada").map(flight_row => flight_row).take(5)

res0: Array[Flight] = Array(Flight(United States,Romania,1), Flight(United States,Ireland,264), Flight(United States,India,69), Flight(Egypt,United States,24), Flight(Equatorial Guinea,United States,1))

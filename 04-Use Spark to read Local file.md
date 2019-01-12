## Use Spark to read local file data

```scala
import org.apache.spark.sql.{DataFrame, SparkSession}
val spark = SparkSession.builder().appName("ReadLocalFiles")
                      .master("local").getOrCreate()

val df: DataFrame = spark.read.option("header", "true").option("inferSchema", "true")
                      .format("csv").load("joined100.csv")
println("Printing the schema of the data:")
df.printSchema()
println("Number of Columns:"+df.columns.toSeq.size)
println("Number of Datapoints:"+df.count())
println("Printing first 20 rows:")
df.show()
```


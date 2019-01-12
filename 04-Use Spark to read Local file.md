## Use Spark to read local file data

```scala
import org.apache.spark.sql.{DataFrame, SparkSession}
val spark = SparkSession.builder().appName("ReadLocalFiles")
                      .master("local").getOrCreate()

val df: DataFrame = spark.read.option("header", "true").option("inferSchema", "true")
                      .format("csv").load("data/joined100.csv")
```


###Linear Regression with Scala / Spark

Use Spark's ML-lib to utilize in-built machine learning capabilities of Spark.

1. First step is to include ML-lib as a dependency in the pom.xml file. Use the following piece of code to include.

```xml
<dependency>
    <groupId>org.apache.spark</groupId>
    <artifactId>spark-mllib_${scala.compat.version}</artifactId>
    <version>${spark.version}</version>
</dependency>
```

2. Next step is to create a Scala object file under src/main/scala directory. To do so, go to 'scala' folder, right click to access 'New' menu and select 'Scala Class' option to reach the dialog box as shown below.

![09-CreateScalaObject](./img/09-CreateScalaObject.PNG)

3. Give it a meaningful name, select 'Object' as Kind and click OK. You will see a blank LinearRegressionExample.scala file with an Object created named LinearRegressionExample.

4. Create another directory under your project structure named 'data' and download any desired dataset in that directory. Here USA_Housing.csv dataset has been used for illustration purpose.

5. Next step is to import the following libraries by writing the following piece of code at the very beginning. 

   ```scala
   import org.apache.spark.sql.{DataFrame, SparkSession}
   import org.apache.spark.ml.regression.LinearRegression
   import org.apache.spark.ml.evaluation.RegressionEvaluator
   import org.apache.log4j._
   ```

   SparkSession aids in creating a SparkSession. DataFrame library utilized Spark DataFrame API which is a named column abstraction on top of Spark RDD.

   LinearRegression library is the main library for this example. RegressionEvaluator helps in evaluating how good the predictions are against the actual values using a pre-defined metrics such as 'mse', 'r2' or 'rmse'.

   log4j library helps in suppressing unwanted warning and information from getting printed onto the console.

6. Next step is to load the dataset. Use the following lines of code for creating a spark session and importing dataset.

   ```scala
   Logger.getLogger("org").setLevel(Level.ERROR)
   val spark = SparkSession.builder().appName("LinearRegressionAlgorithm")
     						.master("local").getOrCreate()
   val df: DataFrame = spark.read.option("header", "true").option("inferSchema", "true")
    						.format("csv").load("data/USA_Housing.csv")
   import df.sqlContext.implicits._
   ```

7. Check out the datatype and structure of data before proceeding with modelling. 

   ```scala 
   df.printSchema()
   df.show()
   println("Number of Columns:"+df.columns.toSeq.size)
   println("Number of Datapoints:"+df.count())
   ```

8. If any, null rows can be removed using na.drop() method available with Spark Data Frames. There are 2 arguements:

   "any": remove rows if any of the column is null

   "all": remove rows if all the columns are null

   ```scala 
   //Remove missing value or NULL rows
   val removeNullDF = df.na.drop("any")
   ```

9. 
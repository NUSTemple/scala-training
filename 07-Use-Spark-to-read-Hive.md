# Use Spark to read Hive data

### Ingest data into Hive based on CSV
- [load-csv-file-into-hive-orc-table](https://bigdataprogrammers.com/load-csv-file-into-hive-orc-table/) 

- [data source](https://www.stats.govt.nz/assets/Uploads/Overseas-trade-indexes-prices-and-volumes/Overseas-trade-indexes-prices-and-volumes-December-2018-quarter-provisional/Download-data/overseas-trade-indexes-december-2018-quarter-provisional-csv.csv)

```hiveql
create schema if not exists overseas_trade_indexes;

CREATE EXTERNAL TABLE IF NOT EXISTS overseas_trade_indexes.csv_table
(
Series_reference STRING,
Period STRING,
Data_value DOUBLE,
STATUS STRING,
UNITS STRING,
MAGNTUDE STRING,
Subject STRING,
Group_id STRING,
Series_title_1 STRING,
Series_title_2 STRING,
Series_title_3 STRING,
Series_title_4 STRING,
Series_title_5 STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS TEXTFILE
LOCATION '/user/pengtan/sample_data/overseas_trade_indexes';

CREATE TABLE overseas_trade_indexes.orc_table
(
Series_reference STRING,
Period STRING,
Data_value DOUBLE,
STATUS STRING,
UNITS STRING,
MAGNTUDE STRING,
Subject STRING,
Group_id STRING,
Series_title_1 STRING,
Series_title_2 STRING,
Series_title_3 STRING,
Series_title_4 STRING,
Series_title_5 STRING
)
STORED AS ORC;


INSERT INTO TABLE overseas_trade_indexes.orc_table SELECT * FROM overseas_trade_indexes.csv_table
WHERE csv_table.Series_reference <> 'Series_reference';
```
######################################################################################################

- [data source](https://data.consumerfinance.gov/api/views/s6ew-h6mp/rows.csv?accessType=DOWNLOAD)

```hiveql
CREATE EXTERNAL TABLE IF NOT EXISTS consumer_complaints.csv_table
(
date_received string,
product string,
sub_product string,
issue string,
sub_issue string,
consumer_complaint_narrative string,
company_public_response string,
company string,
state string,
zip_code string,
tags string,
consumer_consent_provided string,
submitted_via string,
date_sent_to_company string,
company_response_to_consumer string,
timely_response string,
consumer_disputed string,
complaint_id string
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS TEXTFILE
LOCATION '/user/pengtan/sample_data/consumer_complaints';

```
```hiveql

CREATE TABLE consumer_complaints.orc_table
(
date_received string,
product string,
sub_product string,
issue string,
sub_issue string,
consumer_complaint_narrative string,
company_public_response string,
company string,
state string,
zip_code string,
tags string,
consumer_consent_provided string,
submitted_via string,
date_sent_to_company string,
company_response_to_consumer string,
timely_response string,
consumer_disputed string,
complaint_id string
)
STORED AS ORC;

 
 INSERT INTO TABLE consumer_complaints.orc_table SELECT * FROM consumer_complaints.csv_table
WHERE csv_table.date_received <> 'Date received';

```

###

```bash
spark-shell --master yarn --jars \
/usr/hdp/current/hive_warehouse_connector/hive-warehouse-connector-assembly-1.0.0.3.1.0.0-78.jar
```
or 

```bash
spark-shell --master yarn \
--jars /usr/hdp/3.0.1.0-183/hive_warehouse_connector/hive-warehouse-connector-assembly-1.0.0.3.0.1.0-183.jar \
--conf spark.security.credentials.hiveserver2.enabled=false
--conf spark.hadoop.hive.llap.daemon.service.hosts='<LLAP_APP_NAME>'
--conf spark.sql.hive.hiveserver2.jdbc.url='jdbc:hive2://<ZOOKEEPER_QUORUM>;serviceDiscoveryMode=zookeeper;zookeeperNamespace=hiveserver2-interactive'
--conf spark.datasource.hive.warehouse.load.staging.dir='<STAGING_DIR>'
--conf spark.datasource.hive.warehouse.metastoreUri='<METASTORE_URI>'
--conf spark.hadoop.hive.zookeeper.quorum='<ZOOKEEPER_QUORUM>'
```
[How to fill in above parameter](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/integrating-hive/content/hive_configure_a_spark_hive_connection.html)


###

```scala
import com.hortonworks.hwc.HiveWarehouseSession
val hive = HiveWarehouseSession.session(spark).build()
hive.showDatabases.show();
hive.setDatabase("consumer_complaints");
val df = hive.executeQuery("SELECT * FROM csv_table limit 10");
df.show()
```
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


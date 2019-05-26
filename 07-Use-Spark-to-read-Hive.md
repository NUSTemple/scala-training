# Use Spark to read Hive data

### Ingest data into Hive based on CSV
- [load-csv-file-into-hive-orc-table](https://bigdataprogrammers.com/load-csv-file-into-hive-orc-table/) 

```bash
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
```


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


###
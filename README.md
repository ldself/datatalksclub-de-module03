# datatalksclub-de-module03
DataTalksClub module 3

### Create external table
CREATE OR REPLACE EXTERNAL TABLE
  `dtc-de-course-484604`.`ny_taxi_ds_lds`.`yellow_tripdata_2024_01-06_parquet_ext` OPTIONS ( format = 'PARQUET',
    uris = [ 'gs://ny_taxi_bucket_lds/yellow_tripdata_2024-01.parquet',
    'gs://ny_taxi_bucket_lds/yellow_tripdata_2024-02.parquet',
    'gs://ny_taxi_bucket_lds/yellow_tripdata_2024-03.parquet',
    'gs://ny_taxi_bucket_lds/yellow_tripdata_2024-04.parquet',
    'gs://ny_taxi_bucket_lds/yellow_tripdata_2024-05.parquet',
    'gs://ny_taxi_bucket_lds/yellow_tripdata_2024-06.parquet']);

### Create materialized table
CREATE OR REPLACE TABLE `dtc-de-course-484604.ny_taxi_ds_lds.yellow_tripdata_2024_01-06_parquet_non_partitioned` AS
SELECT *
 FROM `dtc-de-course-484604.ny_taxi_ds_lds.yellow_tripdata_2024_01-06_parquet_ext`

### Question 1
What is count of records for the 2024 Yellow Taxi Data?
SELECT COUNT(*) FROM
  `dtc-de-course-484604`.`ny_taxi_ds_lds`.`yellow_tripdata_2024_01-06_parquet_ext`
20332093

### Question 2
Write a query to count the distinct number of PULocationIDs for the entire dataset on both the tables.
What is the estimated amount of data that will be read when this query is executed on the External Table and the Table?
SELECT COUNT(DISTINCT PULocationID) FROM
  `dtc-de-course-484604`.`ny_taxi_ds_lds`.`yellow_tripdata_2024_01-06_parquet_ext`
SELECT COUNT(DISTINCT PULocationID) FROM
  `dtc-de-course-484604`.`ny_taxi_ds_lds`.`yellow_tripdata_2024_01-06_parquet_non_partitioned`
0MB, 155.112MB

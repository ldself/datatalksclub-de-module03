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

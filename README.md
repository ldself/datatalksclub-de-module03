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

### Question 3
Write a query to retrieve the PULocationID from the table (not the external table) in BigQuery. Now write a query to retrieve the PULocationID and DOLocationID on the same table. Why are the estimated number of Bytes different?
SELECT PULocationID FROM
  `dtc-de-course-484604`.`ny_taxi_ds_lds`.`yellow_tripdata_2024_01-06_parquet_non_partitioned`
SELECT PULocationID, DOLocationID FROM
  `dtc-de-course-484604`.`ny_taxi_ds_lds`.`yellow_tripdata_2024_01-06_parquet_non_partitioned`
155.12MB, 310.24MB - It's reading two columns from a columnar database

### Question 4
How many records have a fare_amount of 0?
SELECT COUNT(*) FROM
  `dtc-de-course-484604`.`ny_taxi_ds_lds`.`yellow_tripdata_2024_01-06_parquet_ext`
WHERE fare_amount = 0
8333

### Question 5
What is the best strategy to make an optimized table in Big Query if your query will always filter based on tpep_dropoff_datetime and order the results by VendorID (Create a new table with this strategy)
Partition by tpep_dropoff_datetime and Cluster on VendorID

### Question 6
Write a query to retrieve the distinct VendorIDs between tpep_dropoff_datetime 2024-03-01 and 2024-03-15 (inclusive)

Use the materialized table you created earlier in your from clause and note the estimated bytes. Now change the table in the from clause to the partitioned table you created for question 5 and note the estimated bytes processed. What are these values?
310.24, 0  (Closest = 310.24, 26.84)

### Question 7
Where is the data stored in the External Table you created?
GCP Bucket

### Question 8
It is best practice in Big Query to always cluster your data:
False




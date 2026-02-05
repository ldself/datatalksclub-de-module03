# DataTalksClub DE Module 03

Data Engineering Zoomcamp - Module 3: Data Warehouse (BigQuery)

## Setup

### Create External Table

```sql
CREATE OR REPLACE EXTERNAL TABLE
  `dtc-de-course-484604`.`ny_taxi_ds_lds`.`yellow_tripdata_2024_01-06_parquet_ext`
OPTIONS (
  format = 'PARQUET',
  uris = [
    'gs://ny_taxi_bucket_lds/yellow_tripdata_2024-01.parquet',
    'gs://ny_taxi_bucket_lds/yellow_tripdata_2024-02.parquet',
    'gs://ny_taxi_bucket_lds/yellow_tripdata_2024-03.parquet',
    'gs://ny_taxi_bucket_lds/yellow_tripdata_2024-04.parquet',
    'gs://ny_taxi_bucket_lds/yellow_tripdata_2024-05.parquet',
    'gs://ny_taxi_bucket_lds/yellow_tripdata_2024-06.parquet'
  ]
);
```

### Create Materialized Table

```sql
CREATE OR REPLACE TABLE
  `dtc-de-course-484604.ny_taxi_ds_lds.yellow_tripdata_2024_01-06_parquet_non_partitioned` AS
SELECT *
FROM `dtc-de-course-484604.ny_taxi_ds_lds.yellow_tripdata_2024_01-06_parquet_ext`;
```

---

## Homework Questions

### Question 1

**What is the count of records for the 2024 Yellow Taxi Data?**

```sql
SELECT COUNT(*)
FROM `dtc-de-course-484604`.`ny_taxi_ds_lds`.`yellow_tripdata_2024_01-06_parquet_ext`;
```

**Answer:** 20,332,093

---

### Question 2

**Write a query to count the distinct number of PULocationIDs for the entire dataset on both tables. What is the estimated amount of data that will be read when this query is executed on the External Table and the Table?**

```sql
-- External Table
SELECT COUNT(DISTINCT PULocationID)
FROM `dtc-de-course-484604`.`ny_taxi_ds_lds`.`yellow_tripdata_2024_01-06_parquet_ext`;

-- Materialized Table
SELECT COUNT(DISTINCT PULocationID)
FROM `dtc-de-course-484604`.`ny_taxi_ds_lds`.`yellow_tripdata_2024_01-06_parquet_non_partitioned`;
```

**Answer:** 0 MB (External), 155.12 MB (Materialized)

---

### Question 3

**Write a query to retrieve the PULocationID from the table (not the external table) in BigQuery. Now write a query to retrieve the PULocationID and DOLocationID on the same table. Why are the estimated number of bytes different?**

```sql
-- Single column
SELECT PULocationID
FROM `dtc-de-course-484604`.`ny_taxi_ds_lds`.`yellow_tripdata_2024_01-06_parquet_non_partitioned`;

-- Two columns
SELECT PULocationID, DOLocationID
FROM `dtc-de-course-484604`.`ny_taxi_ds_lds`.`yellow_tripdata_2024_01-06_parquet_non_partitioned`;
```

**Answer:** 155.12 MB vs 310.24 MB - BigQuery uses columnar storage, so reading two columns requires reading twice as much data.

---

### Question 4

**How many records have a fare_amount of 0?**

```sql
SELECT COUNT(*)
FROM `dtc-de-course-484604`.`ny_taxi_ds_lds`.`yellow_tripdata_2024_01-06_parquet_ext`
WHERE fare_amount = 0;
```

**Answer:** 8,333

---

### Question 5

**What is the best strategy to make an optimized table in BigQuery if your query will always filter based on tpep_dropoff_datetime and order the results by VendorID?**

**Answer:** Partition by `tpep_dropoff_datetime` and Cluster on `VendorID`

---

### Question 6

**Write a query to retrieve the distinct VendorIDs between tpep_dropoff_datetime 2024-03-01 and 2024-03-15 (inclusive). Use the materialized table and the partitioned table. What are the estimated bytes processed?**

```sql
-- Non-partitioned table
SELECT DISTINCT(VendorID)
FROM `dtc-de-course-484604`.`ny_taxi_ds_lds`.`yellow_tripdata_2024_01-06_parquet_non_partitioned`
WHERE tpep_dropoff_datetime >= '2024-03-01 00:00:00'
  AND tpep_dropoff_datetime < '2024-03-15 00:00:00';

-- External table
SELECT DISTINCT(VendorID)
FROM `dtc-de-course-484604`.`ny_taxi_ds_lds`.`yellow_tripdata_2024_01-06_parquet_ext`
WHERE tpep_dropoff_datetime >= '2024-03-01 00:00:00'
  AND tpep_dropoff_datetime < '2024-03-15 00:00:00';
```

**Answer:** 310.24 MB (non-partitioned), 0 MB (external) — Closest option: 310.24 MB, 26.84 MB

---

### Question 7

**Where is the data stored in the External Table you created?**

**Answer:** GCP Bucket

---

### Question 8

**It is best practice in BigQuery to always cluster your data:**

**Answer:** False

---

### Question 9

**Write a SELECT count(*) query FROM the materialized table you created. How many bytes does it estimate will be read? Why?**

```sql
SELECT count(*) FROM
  `dtc-de-course-484604`.`ny_taxi_ds_lds`.`yellow_tripdata_2024_01-06_parquet_non_partitioned`
```

**Answer:** 0.  The row count is stored in the metadata of the table.

## HOMEWORK 3

**Creating External Table Referring to GCS**

```sql
CREATE EXTERNAL TABLE `taxi-rides-ny-452215.taxi_rides_module3.external_yellow_tripdata`
OPTIONS (
  format = 'PARQUET',
  uris = ['gs://taxi-rides-ny-mrv/module3/yellow_tripdata_2024-*.parquet']
);
```
**Creating non partitioned table**

```sql
CREATE TABLE taxi-rides-ny-452215.taxi_rides_module3.yellow_tripdata_non_partitioned AS
SELECT * FROM taxi-rides-ny-452215.taxi_rides_module3.external_yellow_tripdata ;
```
----------------------------------------------------------------------------------------------------
**Count of records for January to June 2024 Yellow Taxi Data (Question 1)**

```sql
SELECT COUNT(*) 
FROM taxi-rides-ny-452215.taxi_rides_module3.external_yellow_tripdata ;
```
Result: 20332093
----------------------------------------------------------------------------------------------------
**Counting distinct PULocationID for tables (Question 2)**

```sql
SELECT COUNT(DISTINCT PULocationID)
FROM taxi-rides-ny-452215.taxi_rides_module3.external_yellow_tripdata ;

SELECT COUNT(DISTINCT PULocationID)
FROM taxi-rides-ny-452215.taxi_rides_module3.yellow_tripdata_non_partitioned ;
```
Estimated amount of data is 0 MB for the External Table and 155.12 MB for the Materialized Table
----------------------------------------------------------------------------------------------------
**Retrieving PULocationID from materialized table (Question 3)**

```sql
SELECT PULocationID
FROM taxi-rides-ny-452215.taxi_rides_module3.yellow_tripdata_non_partitioned;
```
**Retrieving PULocationID and DOLocationID from materialized table**

```sql
SELECT PULocationID, DOLocationID
FROM taxi-rides-ny-452215.taxi_rides_module3.yellow_tripdata_non_partitioned;
```
BigQuery is a columnar database, and it only scans the specific columns requested in the query. Querying two columns (PULocationID, DOLocationID) requires reading more data than querying one column (PULocationID), leading to a higher estimated number of bytes processed.
----------------------------------------------------------------------------------------------------
**Counting the number of records where fare_amount is 0 (Question 4)**


```sql
SELECT COUNT(*) 
FROM `taxi-rides-ny-452215.taxi_rides_module3.yellow_tripdata_non_partitioned`
WHERE fare_amount = 0 ;
```
Result: 8333
----------------------------------------------------------------------------------------------------
**Creating a partition and cluster table (Question 5)**


```sql
CREATE OR REPLACE TABLE taxi-rides-ny-452215.taxi_rides_module3.yellow_tripdata_partitioned_clustered
PARTITION BY DATE(tpep_pickup_datetime)
CLUSTER BY VendorID AS
SELECT * FROM taxi-rides-ny-452215.taxi_rides_module3.external_yellow_tripdata ;
```
Best strategy to make an optimized table is Partition by tpep_dropoff_datetime and Cluster on VendorID
----------------------------------------------------------------------------------------------------
**Retrieving VendorID (Question 6)**

```sql
SELECT DISTINCT VendorID
FROM `taxi-rides-ny-452215.taxi_rides_module3.yellow_tripdata_non_partitioned`
WHERE tpep_dropoff_datetime BETWEEN '2024-03-01' AND '2024-03-15';
```
```sql
SELECT DISTINCT VendorID
FROM `taxi-rides-ny-452215.taxi_rides_module3.yellow_tripdata_partitioned_clustered`
WHERE tpep_dropoff_datetime BETWEEN '2024-03-01' AND '2024-03-15';
```
Result: 310.24 MB for non-partitioned table and 26.84 MB for the partitioned table
----------------------------------------------------------------------------------------------------
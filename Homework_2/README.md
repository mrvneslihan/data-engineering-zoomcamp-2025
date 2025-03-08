1. **What is the uncompressed file size (i.e. the output file yellow_tripdata_2020-12.csv of the extract task)?**  
   **Answer:** 128.3 MiB  
   The file size can be accessed from the outputFiles section in the outputs when the December 2020 data is executed. The file size is 128.3 MiB.

2. **What is the rendered value of the variable file when the inputs taxi is set to green, year is set to 2020, and month is set to 04 during execution?**  
   **Answer:** green_tripdata_2020-04.csv  
   The file name format is defined as `{{inputs.taxi}}_tripdata_{{inputs.year}}-{{inputs.month}}.csv`. With the inputs set to "green" for taxi, "2020" for year, and "04" for the month, the rendered file name will be green_tripdata_2020-04.csv.

3. **How many rows are there for the Yellow Taxi data for all CSV files in the year 2020?**  
   **Answer:** 24,648,499  
   The answer to this question can be obtained by selecting yellow taxi data from the beginning to the end of 2020 through a Backfill execution.  
   ```sql
   SELECT COUNT(*) 
   FROM public.yellow_tripdata;
   ```
   This query counts all the rows in the table that correspond to Yellow Taxi data for the year 2020, resulting in a total of **24,648,499** rows.

4. **How many rows are there for the Green Taxi data for all CSV files in the year 2020?**  
   **Answer:** 1,734,051  
   The answer to this question can be obtained by selecting green taxi data from the beginning to the end of 2020 through a Backfill execution.
   ```sql
   SELECT COUNT(*) 
   FROM public.green_tripdata;
   ```
   The answer to this question can be obtained by selecting green taxi data from the beginning to the end of 2020 through a Backfill execution.

5. **How many rows are there for the Yellow Taxi data for the March 2021 CSV file?**  
   **Answer:** 1,925,152  
   The answer to this question can be obtained by selecting Yellow Taxi data for March 2021 through a direct execution.
    ```sql
   SELECT COUNT(*) 
   FROM public.yellow_tripdata_staging;
   ```
   This query counts all the rows in the staging table that correspond to Yellow Taxi data for March 2021, resulting in a total of  1,925,152 rows. 

6. **How would you configure the timezone to New York in a Schedule trigger?**  
   **Answer:** “Add a timezone property set to America/New_York in the Schedule trigger configuration.”

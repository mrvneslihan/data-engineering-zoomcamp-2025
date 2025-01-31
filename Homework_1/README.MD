Question 1

docker run -it --entrypoint=bash python:3.12.8
root@cf29aaf92196:/# pip --version
pip 24.3.1 from /usr/local/lib/python3.12/site-packages/pip (python 3.12)

Question 3

SELECT
COUNT(CASE WHEN trip_distance <= 1 THEN 1 END) AS "Up to 1 mile",
COUNT(CASE WHEN trip_distance > 1 AND trip_distance <= 3 THEN 1 END) AS "Between 1 and 3 miles",
COUNT(CASE WHEN trip_distance > 3 AND trip_distance <= 7 THEN 1 END) AS "Between 3 and 7 miles",
COUNT(CASE WHEN trip_distance > 7 AND trip_distance <= 10 THEN 1 END) AS "Between 7 and 10 miles",
COUNT(CASE WHEN trip_distance > 10 THEN 1 END) AS "Over 10 miles"
FROM green_taxi_data
WHERE lpep_pickup_datetime >= '2019-10-01 00:00:00'
AND lpep_pickup_datetime < '2019-11-01 00:00:00';

Question 4

WITH Daily_Max_Distance AS (
    SELECT 
        DATE(lpep_pickup_datetime) AS pickup_day, 
        MAX(trip_distance) AS max_trip_distance
    FROM green_taxi_data
    WHERE lpep_pickup_datetime >= '2019-10-01 00:00:00' 
      AND lpep_pickup_datetime < '2019-11-01 00:00:00'
    GROUP BY DATE(lpep_pickup_datetime)
)
SELECT pickup_day, max_trip_distance
FROM Daily_Max_Distance
ORDER BY max_trip_distance DESC
LIMIT 1;

Question 5

SELECT
z."Zone",
SUM(gtd.total_amount) AS total_amount
FROM
green_taxi_data gtd
JOIN
zones z
ON
gtd."PULocationID" = z."LocationID"
WHERE
gtd.lpep_pickup_datetime::date = '2019-10-18'
GROUP BY
z."Zone"
HAVING
SUM(gtd.total_amount) > 13000
ORDER BY
total_amount DESC;

Question 6

SELECT 
    z."Zone", 
    MAX(gtd.tip_amount) AS max_tip
FROM 
    green_taxi_data gtd
JOIN 
    zones z 
ON 
    gtd."DOLocationID" = z."LocationID"
WHERE 
    gtd.lpep_pickup_datetime::date BETWEEN '2019-10-01' AND '2019-10-31'
    AND gtd."PULocationID" = (SELECT "LocationID" FROM zones WHERE "Zone" = 'East Harlem North')
GROUP BY 
    z."Zone"
ORDER BY 
    max_tip DESC
LIMIT 1;
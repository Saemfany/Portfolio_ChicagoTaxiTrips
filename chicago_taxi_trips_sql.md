```sql
-- Calculate the average, median, and standard deviation of trip duration (trip_seconds) for trips made on Monday and Saturday. Compare the results for both days.
WITH avg_stddev_seconds_table AS (
  SELECT
    FORMAT_TIMESTAMP('%A', trip_start_timestamp) AS weekday,
    AVG(trip_seconds) AS avg_seconds, --Calculates average of trip_seconds
    STDDEV(trip_seconds) AS stddev_seconds --Calculates standard deviation of trip_seconds
  FROM
    bigquery-public-data.chicago_taxi_trips.taxi_trips
  WHERE
    EXTRACT(DAYOFWEEK FROM trip_start_timestamp) IN (2, 7)  -- 2 for Monday, 7 for Saturday
  GROUP BY
    weekday
),
median_seconds_table AS (
  SELECT
    FORMAT_TIMESTAMP('%A', trip_start_timestamp) AS weekday,
    --Calculates median of trip_seconds:
    PERCENTILE_DISC(trip_seconds, 0.5) OVER (PARTITION BY FORMAT_TIMESTAMP('%A', trip_start_timestamp)) AS median_seconds
  FROM
    bigquery-public-data.chicago_taxi_trips.taxi_trips
  WHERE
    EXTRACT(DAYOFWEEK FROM trip_start_timestamp) IN (2, 7)  -- 2 for Monday, 7 for Saturday
)
SELECT
  a.weekday,
  a.avg_seconds,
  m.median_seconds,
  a.stddev_seconds
FROM
  avg_stddev_seconds_table a
JOIN
  (SELECT DISTINCT weekday, median_seconds FROM median_seconds_table) m
ON
  a.weekday = m.weekday
ORDER BY
  a.weekday DESC;

-- Find the top five routes (from the pickup_community_area to dropoff_community_area) with the most trips in the year 2023.
SELECT
  pickup_community_area,
  dropoff_community_area,
  COUNT(*) AS num_trips
FROM
  bigquery-public-data.chicago_taxi_trips.taxi_trips
WHERE
  pickup_community_area IS NOT NULL
  AND dropoff_community_area IS NOT NULL
  AND EXTRACT(year FROM trip_start_timestamp) = 2023
GROUP BY
  pickup_community_area,
  dropoff_community_area
ORDER BY
  num_trips DESC
LIMIT
  5;

-- Compare the average taxi trip cost (fare, tips, and taxes) based on payment methods in the year 2019.
SELECT
  payment_type,
  AVG(fare) AS average_fare,
  AVG(tips) AS average_tips,
  AVG(tolls) AS average_tolls
FROM
  bigquery-public-data.chicago_taxi_trips.taxi_trips
WHERE
  EXTRACT(year FROM trip_start_timestamp) = 2019
GROUP BY
  payment_type
ORDER BY
  average_fare DESC;

-- Are there any seasonal patterns in taxi trips?
-- Daily Patterns
SELECT
  FORMAT_TIMESTAMP('%A', trip_start_timestamp) AS day_of_week,
  COUNT(*) AS trip_count
FROM
  `bigquery-public-data.chicago_taxi_trips.taxi_trips`
WHERE
  trip_start_timestamp IS NOT NULL
GROUP BY
  day_of_week
ORDER BY
  CASE
    WHEN day_of_week = 'Sunday' THEN 1
    WHEN day_of_week = 'Monday' THEN 2
    WHEN day_of_week = 'Tuesday' THEN 3
    WHEN day_of_week = 'Wednesday' THEN 4
    WHEN day_of_week = 'Thursday' THEN 5
    WHEN day_of_week = 'Friday' THEN 6
    WHEN day_of_week = 'Saturday' THEN 7
  END ASC;
-- Monthly Patterns
SELECT
  FORMAT_TIMESTAMP('%B', trip_start_timestamp) AS month,
  COUNT(*) AS trip_count
FROM
  `bigquery-public-data.chicago_taxi_trips.taxi_trips`
WHERE
  trip_start_timestamp IS NOT NULL
GROUP BY
  month
ORDER BY
  CASE
    WHEN month = 'January' THEN 1
    WHEN month = 'February' THEN 2
    WHEN month = 'March' THEN 3
    WHEN month = 'April' THEN 4
    WHEN month = 'May' THEN 5
    WHEN month = 'June' THEN 6
    WHEN month = 'July' THEN 7
    WHEN month = 'August' THEN 8
    WHEN month = 'September' THEN 9
    WHEN month = 'October' THEN 10
    WHEN month = 'November' THEN 11
    WHEN month = 'December' THEN 12
  END ASC;
-- Seasonal Patterns
SELECT
  CASE 
    WHEN EXTRACT(MONTH FROM trip_start_timestamp) IN (12, 1, 2) THEN 'Winter'
    WHEN EXTRACT(MONTH FROM trip_start_timestamp) IN (3, 4, 5) THEN 'Spring'
    WHEN EXTRACT(MONTH FROM trip_start_timestamp) IN (6, 7, 8) THEN 'Summer'
    WHEN EXTRACT(MONTH FROM trip_start_timestamp) IN (9, 10, 11) THEN 'Fall'
  END AS season,
  COUNT(*) AS trip_count
FROM
  `bigquery-public-data.chicago_taxi_trips.taxi_trips`
WHERE
  trip_start_timestamp IS NOT NULL
GROUP BY
  season
ORDER BY
  CASE
    WHEN season = 'Winter' THEN 1
    WHEN season = 'Spring' THEN 2
    WHEN season = 'Summer' THEN 3
    WHEN season = 'Fall' THEN 4
  END ASC;

-- Which taxi company has the highest revenue?
SELECT
  company,
  sum(trip_total) AS total_revenue
FROM
  `bigquery-public-data.chicago_taxi_trips.taxi_trips`
WHERE
  company IS NOT NULL
  AND trip_total IS NOT NULL
GROUP BY
  company
ORDER BY
  total_revenue DESC
LIMIT
  10;
```

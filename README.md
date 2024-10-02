# Chicago Taxi Trips Data Analysis
This repository contains a detailed analysis of the Chicago Taxi Trips dataset, focusing on various business insights such as popular routes, fare patterns, seasonal trends, and payment methods. The analysis aims to uncover patterns that can be useful for business decisions in the transportation sector.

## Table of Contents
* Introduction
* Dataset
* Objectives
* Methodology
* Analysis
* Results
* Conclusion
* License

## Introduction
The Chicago Taxi Trips dataset is a large-scale collection of taxi trip records from the city of Chicago. The dataset provides valuable insights into travel patterns, popular routes, fare structures, and customer preferences in terms of payment methods.

## Dataset
The dataset used for this analysis is sourced from the Chicago Taxi Trips public data, available on Google BigQuery. It includes key fields such as:
* Taxi trip start and end times
* Trip duration and distance
* Pickup and dropoff locations
* Fare, tips, and tolls
* Payment method and taxi company

## Objectives
The main objectives of this analysis are:
* Identify the most popular taxi routes in Chicago.
* Examine the correlation between trip duration and total fare.
* Analyze seasonal and monthly patterns in taxi usage.
* Compare fare, tips, and tolls based on different payment methods.
* Determine which taxi companies generate the most revenue.

## Methodology
This analysis uses SQL queries to process the data in Google BigQuery, and Python (pandas, matplotlib, seaborn) to visualize the results. Key metrics such as average trip duration, total fare, and revenue by company are calculated.

## Analysis
The analysis includes:
* Most popular pickup and dropoff areas
* Payment method analysis
* Daily, monthly, and seasonal patterns
* Correlation between trip distance and fare
* Top revenue-generating taxi companies

## Results
Key findings from the analysis:
* Certain routes, like those from community area 8 to 8, are the most frequent.
* Credit card payments tend to result in higher tips compared to cash.
* Taxi usage follows strong seasonal patterns, with spring showing the highest number of trips.
* Taxi Affiliation Services leads the market in terms of total revenue.

## Conclusion
The analysis provides valuable insights into taxi operations in Chicago. By identifying popular routes, peak times, and payment preferences, taxi companies can optimize their services and pricing strategies.

## License
This project is licensed under the MIT License. See the LICENSE file for more details.

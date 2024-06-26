# Cyclistic-Ride-Data

# Introduction

Data Analysis case study for the fictional bike share company Cyclistic,
using Excel, SQL and Tableau

This Case Study will follow the APPASA format

### [1. Ask](#ask)  
### [2. Prepare](#prepare)  
### [3. Process](#process)  
### [4. Analyze](#analyze)  
### [5. Share](#share)  
### [6. Act](#act) 

[Tableau Presentation
](https://public.tableau.com/views/CyclisticinMotionAnalyzingRideDataforMarketingSuccess/CyclisticRideDataPresentation?:language=en-US&:sid=&:display_count=n&:origin=viz_share_link)

[Tableau Dashboard
](https://public.tableau.com/views/CyclisticRideDataDashboard/RideDataDashboard?:language=en-US&:sid=&:display_count=n&:origin=viz_share_link)

# Scenario

Cyclistic is a bike-share company in Chicago. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships.
Therefore, your team wants to understand how casual riders and annual members use Cyclistic bikes differently.
From these insights, your team will design a new marketing strategy to convert casual riders into annual members.
But first, Cyclistic executives must approve your recommendations, so they must be backed up with compelling data insights and professional data visualizations.

# Ask
### Main Business Task

* Analyze how members and casual riders use Cyclistic bikes differently. 

* Provide recomendations to convert casual riders into annual members based on the findings.

### Stakeholders

* Lily Moreno: Director of marketing 

* Cyclistic Executive Team: Team that will decide whether to approve the recomended marketing program

* Cyclistic Marketing Analytics Team: Team of data analysts responsible for guiding Cyclistic marketing strategy

# Prepare
### Data Source

Data is downloaded from https://divvy-tripdata.s3.amazonaws.com/index.html

Trip data is organized into 1 CSV file per month for a total of 12 CSVs for the year 2023

Data deemed as credible and passes ROCCC method

- [x] Reliable - High Sample Size, over 5 million rows of data    
- [x] Original - Public Dataset provided by Motivate International Inc. 2nd Party Data Source
- [x] Comprehensive - 5 million rows with 13 columns of relevant data
- [x] Current - Data is from 2023
- [x] Cited - Motivate International Inc.

The 12 CSVs of data will be used

| Filename                  | Rows of Data |
|---------------------------|--------------|
| 202301-divvy-tripdata.csv | 190301       |
| 202302-divvy-tripdata.csv | 190445       |
| 202303-divvy-tripdata.csv | 258678       |
| 202304-divvy-tripdata.csv | 426590       |
| 202305-divvy-tripdata.csv | 604827       |
| 202306-divvy-tripdata.csv | 719618       |
| 202307-divvy-tripdata.csv | 767650       |
| 202308-divvy-tripdata.csv | 771693       |
| 202309-divvy-tripdata.csv | 666371       |
| 202310-divvy-tripdata.csv | 537113       |
| 202311-divvy-tripdata.csv | 362518       |
| 202312-divvy-tripdata.csv | 224073       |
| Total rows of data        | 5719877      |

### Column Overview

| Column Name        | Column Description                                       |
|--------------------|----------------------------------------------------------|
| ride_id            | Alphanumerical ID given to the rides                     |
| rideable_type      | What kind of bicycle was used? Classic, electric, docked |
| start_at           | Timestamp for when the ride started                      |
| ended_at           | Timestamp for when the ride ended                        |
| start_station_name | Name of station where ride started                       |
| start_station_id   | Alphanumeric ID given to the stations                    |
| end_station_name   | Name of station where ride ended                         |
| end_station_id     | Alphanumeric ID given to the stations                    |
| start_lat          | Latitude coordinates for where the ride started          |
| start_lng          | Longitude coordinates for where the ride started         |
| end_lat            | Latitude coordinates for where the ride ended            |
| end_lng            | Longitude coordinates for where the ride ended           |
| member_casual      | Type of ride, Member or Casual                           |
# Process

### Creating Master Table

``` MySQL

-- This will hold all the data from the 12 CSVs as 2023_ride_data

DROP TABLE IF EXISTS 2023_ride_data;
CREATE TABLE 2023_ride_data
(
  ride_id VARCHAR(255),
  rideable_type VARCHAR(255),
  started_at VARCHAR(255),
  ended_at VARCHAR(255),
  start_station_name VARCHAR(255),
  start_station_id VARCHAR(255),
  end_station_name VARCHAR(255),
  end_station_id VARCHAR(255),
  start_lat VARCHAR(255),
  start_lng VARCHAR(255),
  end_lat VARCHAR(255),
  end_lng VARCHAR(255),
  member_casual VARCHAR(255)
)
;
```


### Loading data into Master Table

``` MySQL

-- LOAD DATA INFILE is used to achieve best performance   
-- File path is dependent on where file is stored

LOAD DATA INFILE '/Users/MySQL Import_Export/Trip Data Formatted/202301-divvy-tripdata.csv'
INTO TABLE 2023_ride_data
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS; -- Skip the header row if present
;
LOAD DATA INFILE '/Users/MySQL Import_Export/Trip Data Formatted/202302-divvy-tripdata.csv'
INTO TABLE 2023_ride_data
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS; -- Skip the header row if present
;
LOAD DATA INFILE '/Users/MySQL Import_Export/Trip Data Formatted/202303-divvy-tripdata.csv'
INTO TABLE 2023_ride_data
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS; -- Skip the header row if present
;
LOAD DATA INFILE '/Users/MySQL Import_Export/Trip Data Formatted/202304-divvy-tripdata.csv'
INTO TABLE 2023_ride_data
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS; -- Skip the header row if present
;
LOAD DATA INFILE '/Users/MySQL Import_Export/Trip Data Formatted/202305-divvy-tripdata.csv'
INTO TABLE 2023_ride_data
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS; -- Skip the header row if present
;
LOAD DATA INFILE '/Users/MySQL Import_Export/Trip Data Formatted/202306-divvy-tripdata.csv'
INTO TABLE 2023_ride_data
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS; -- Skip the header row if present
;
LOAD DATA INFILE '/Users/MySQL Import_Export/Trip Data Formatted/202307-divvy-tripdata.csv'
INTO TABLE 2023_ride_data
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS; -- Skip the header row if present
;
LOAD DATA INFILE '/Users/MySQL Import_Export/Trip Data Formatted/202308-divvy-tripdata.csv'
INTO TABLE 2023_ride_data
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS; -- Skip the header row if present
;
LOAD DATA INFILE '/Users/MySQL Import_Export/Trip Data Formatted/202309-divvy-tripdata.csv'
INTO TABLE 2023_ride_data
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS; -- Skip the header row if present
;
LOAD DATA INFILE '/Users/MySQL Import_Export/Trip Data Formatted/202310-divvy-tripdata.csv'
INTO TABLE 2023_ride_data
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS; -- Skip the header row if present
;
LOAD DATA INFILE '/Users/MySQL Import_Export/Trip Data Formatted/202311-divvy-tripdata.csv'
INTO TABLE 2023_ride_data
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS; -- Skip the header row if present
;
LOAD DATA INFILE '/Users/MySQL Import_Export/Trip Data Formatted/202312-divvy-tripdata.csv'
INTO TABLE 2023_ride_data
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS; -- Skip the header row if present
;

-- 5719877 rows imported
``` 

### Deleting rows with NULL or blank values

``` MySQL

-- Several station names are blank and cannot be identified even when using coordinate information

DELETE 
FROM 2023_ride_data
WHERE 
ride_id = '' OR ride_id IS NULL
OR
rideable_type = '' OR rideable_type IS NULL
OR
started_at = '' OR started_at IS NULL
OR
ended_at = '' OR ended_at IS NULL
OR
start_station_name = '' OR start_station_name IS NULL
OR
start_station_id = '' OR start_station_id IS NULL
OR
end_station_name = '' OR end_station_name IS NULL
OR
end_station_id = '' OR end_station_id IS NULL
OR
start_lat = '' OR start_lat IS NULL
OR
start_lng = '' OR start_lng IS NULL
OR
end_lat = '' OR end_lat  IS NULL
OR
end_lng = '' OR end_lng IS NULL
OR
member_casual = '' OR member_casual IS NULL
;

-- 1388170 rows deleted   
-- New 2023_ride_data total - 4331707 rows
```


### Cleaning Station names

``` MySQL

-- Station Name variations lead to unnecessary station duplicates

UPDATE 2023_ride_data
SET 
    start_station_name = REPLACE(start_station_name, 'Public Rack - ', ''),
    start_station_name = REPLACE(start_station_name, ' (Temp)', ''),
    start_station_name = REPLACE(start_station_name, '*', ''),
    start_station_name = REPLACE(start_station_name, '/', ' & '),
    start_station_name = REPLACE(start_station_name, 'Buckingham - Fountain', 'Buckingham Fountain'),
    start_station_name = REPLACE(start_station_name, 'Senka "Edward Duke"" Park"', 'Senka "Edward Duke" Park'),
    
    end_station_name = REPLACE(end_station_name, 'Public Rack - ', ''),
    end_station_name = REPLACE(end_station_name, ' (Temp)', ''),
    end_station_name = REPLACE(end_station_name, '*', ''),
    end_station_name = REPLACE(end_station_name, '/', ' & '),
    end_station_name = REPLACE(end_station_name, 'Buckingham - Fountain', 'Buckingham Fountain'),
    end_station_name = REPLACE(end_station_name, 'Senka "Edward Duke"" Park"', 'Senka "Edward Duke" Park')
;

-- 151158 rows affected  
-- Did not delete rows so table total remains same
```

### Deleting rides with "Test" in Station names

``` MySQL

-- "Test" rides will be deleted   

DELETE 
FROM 2023_ride_data
WHERE start_station_name LIKE "%Test%" OR end_station_name LIKE "%Test%"
;

-- 15 rows deleted  
-- New 2023_ride_data total - 4331692 rows

```

### Setting ride_id column to 16 characters

``` MySQL

-- Standardizng Ride IDs

UPDATE 2023_ride_data
SET ride_id = LEFT(ride_id, 16);

-- 484 rows affected
-- Did not delete rows so table total remains same
```

### Trimming all columns

``` MySQL
UPDATE 2023_ride_data
SET
ride_id = TRIM(ride_id),
rideable_type = TRIM(rideable_type),
started_at = TRIM(started_at),
ended_at = TRIM(ended_at),
start_station_name = TRIM(start_station_name),
start_station_id = TRIM(start_station_id),
end_station_name = TRIM(end_station_name),
end_station_id = TRIM(end_station_id),
start_lat = TRIM(start_lat),
start_lng = TRIM(start_lng),
end_lat = TRIM(end_lat),
end_lng = TRIM(end_lng),
member_casual = TRIM(member_casual)
;

-- 257 rows affected
-- Did not delete rows so table total remains same
```

### Dropping start_station_id and end_station_id columns

``` MySQL

-- Station ID columns are not useful or necessary

ALTER TABLE 2023_ride_data
DROP COLUMN start_station_id,
DROP COLUMN end_station_id
;
```

### Creating Temporary Table with Window function to add row number to duplicates

```MySQL

-- Window function and temporary table is used over subqueries to optimize performance
-- Row number function will assign row numbers to entries on all columns except member_casual
-- The first ride entry prioritizing member rides will marked with row number 1  
-- Any duplicate entry will be marked with a row number > 1  
-- Result is stored in temporary table for later use  

DROP TABLE IF EXISTS member_row;

CREATE TEMPORARY TABLE member_row
SELECT 
            ride_id, rideable_type, started_at, ended_at, start_station_name, end_station_name, start_lat, start_lng, end_lat, end_lng, member_casual,
            ROW_NUMBER() OVER (PARTITION BY ride_id, rideable_type, started_at, ended_at, start_station_name, end_station_name ORDER BY member_casual DESC) AS row_num
FROM 2023_ride_data
ORDER BY ride_id
;

-- member_row - temporary table created with 4331692 rows
```

### "Deleting" duplicates prioritzing member rides


```MySQL

-- We are only importing where row num = 1   
-- If duplicate entries exists, they are "deleted" by filtering out row numbers != 1 using the previous temporary table  

TRUNCATE TABLE 2023_ride_data;

INSERT INTO 2023_ride_data  
SELECT ride_id, rideable_type, started_at, ended_at, start_station_name, end_station_name, start_lat, start_lng, end_lat, end_lng, member_casual
FROM member_row
WHERE row_num = 1
ORDER BY ride_id
;

-- New 2023_ride_data total - 4331692 (No duplicates)
```

### Deleting rides with trip durations over 1 day or negative ride duration  

```MySQL

-- Likely technical errors or extreme outliers so they are deleted

DELETE
FROM 2023_ride_data
WHERE TIMESTAMPDIFF (YEAR, started_at, ended_at) > 0
OR TIMESTAMPDIFF (MONTH, started_at, ended_at) > 0
OR TIMESTAMPDIFF (DAY, started_at, ended_at) > 0
OR TIMESTAMPDIFF (MINUTE, started_at, ended_at) < 0
;

-- 167 rows deleted
-- New 2023_ride_data total - 4331525 rows

```

### Deleting rides with same start and end station and ride_durations under 1 minute 

```MySQL

-- If these conditions are met then it is mostly likely a test or accident so they are deleted

DELETE
FROM 2023_ride_data
WHERE start_station_name = end_station_name AND TIMESTAMPDIFF (MINUTE, started_at, ended_at) < 1
ORDER BY ride_id
;

-- 84179 rows deleted
-- New 2023_ride_data total - 4247346 rows
```

## Additions 
Now that the data have been cleaned, two columns are added for analysis

### Adding column route

```MySQL

-- To analyze popular start station + end station combination

ALTER TABLE 2023_ride_data
ADD COLUMN ride_route VARCHAR(255) AS (CONCAT(start_station_name, ' - ', end_station_name))
;
```
### Adding column trip duration

```MySQL

-- To analyze trip duration in minutes

ALTER TABLE 2023_ride_data
ADD COLUMN ride_duration_min INT AS (TIMESTAMPDIFF(MINUTE, started_at, ended_at))
;
```
## Export
 
```MySQL

-- Exporting Table into CSV as 2023_ride_data_export.csv for further analysis in Tableau  
-- SELECT INTO OUTFILE is used to acheive best performance    
-- Filepath is dependent on where file will be stored   

SELECT 'ride_id', 'rideable_type', 'started_at', 'ended_at', 'start_station_name', 'end_station_name', 'start_lat', 'start_lng', 'end_lat', 'end_lng', 'member_casual', 'ride_route', 'ride_duration_min'
UNION ALL
SELECT 'ride_id', 'rideable_type', 'started_at', 'ended_at', 'start_station_name', 'end_station_name', 'start_lat', 'start_lng', 'end_lat', 'end_lng', 'member_casual', 'ride_route', 'ride_duration_min'
INTO OUTFILE '/Users/MySQL Import_Export/2023_ride_data_export.csv'
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
FROM 2023_ride_data;
```
# Analyze 

## Membership Percentage
![
](https://github.com/nathanielcastillo/Cyclistic-Case-Study/blob/main/Images/Member%20Percent.png)

In 2023 approximately 2/3 of the rides were member rides

| Rider Type | Percentage | Count   |  
|------------|------------|---------|   
| Total      | 100%       | 4247346 |  
| Casual     | 35.44%     | 1505329 |  
| Member     | 64.56%     | 2742017 |

## Bicycle Type
![
](https://github.com/nathanielcastillo/Cyclistic-Case-Study/blob/main/Images/Bicycle%20Type.png)

Both members and casual riders ride classic bikes more than electric. Docked bicycles only appeared in casual rides

## Average Ride Duration
![
](https://github.com/nathanielcastillo/Cyclistic-Case-Study/blob/main/Images/Avg%20Ride%20Duration%20Minutes.png)

Members on average have shorter rides than casuals

| Rider Type | Average Ride Duration |
|------------|-----------------------|
| Casual     | 22.71                 |
| Member     | 11.87                 |

## Total Ride Count Per
### Hour
![
](https://github.com/nathanielcastillo/Cyclistic-Case-Study/blob/main/Images/Total%20Ride%20Count%20Per%20Hour.png)

Members are most likely commuting to and from work

| Rider Type | Peak Riding Hours | Count  |
|------------|-------------------|--------|
| Casual     | 5:00 PM           | 147950 |
| Member     | 8:00 AM           | 191013 |
| Member     | 5:00 PM           | 297284 |

### Weekday
![
](https://github.com/nathanielcastillo/Cyclistic-Case-Study/blob/main/Images/Total%20Ride%20Count%20per%20Weekday.png)

Casual and member riders have inverse ride preferences  
Members most likely ride for midweek work commutes and casuals most likely ride for weekend leisure

| Casual                 | Member                    |
|------------------------|---------------------------|
| Rides most on weekends | Rides most during midweek |

### Month
![
](https://github.com/nathanielcastillo/Cyclistic-Case-Study/blob/main/Images/Total%20Ride%20Count%20per%20Month.png)

Both Casuals and member ride peak in the warmer summer months and fall off during the colder winter months

## Start and End Stations

![
](https://github.com/nathanielcastillo/Cyclistic-Case-Study/blob/main/Images/Start%20Station%20Map.png)

| Casual                           | Member                         |
|----------------------------------|--------------------------------|
| More concentrated start stations | More spread out start stations |

![
](https://github.com/nathanielcastillo/Cyclistic-Case-Study/blob/main/Images/End%20Station%20Map.png)

| Casual                         | Member                       |
|--------------------------------|------------------------------|
| More concentrated end stations | More spread out end stations |

![
](https://github.com/nathanielcastillo/Cyclistic-Case-Study/blob/main/Images/Top%2010%20Routes.png)

| Casual                                                 | Member                                                  |
|--------------------------------------------------------|---------------------------------------------------------|
| Most popular rides have the same start and end station | Most popular rides have different start and end station |

# Share

The Data has been analyzed and visualized in Tableau  
Interactive versions are available at links below

[Tableau Presentation
](https://public.tableau.com/views/CyclisticinMotionAnalyzingRideDataforMarketingSuccess/CyclisticRideDataPresentation?:language=en-US&:sid=&:display_count=n&:origin=viz_share_link)

[Tableau Dashboard
](https://public.tableau.com/views/CyclisticRideDataDashboard/RideDataDashboard?:language=en-US&:sid=&:display_count=n&:origin=viz_share_link)

A PowerPoint Presentation is also available in the files 

# Act
## Recommendations

### Top 3 Recommendations to convert Casual Riders to Annual Members

1. During the Summer Months, offer a discount on the annual membership
Since summer are busiest months, casual riders may opt to purchase an annual membership if they plan on riding a lot during the summer

2. Display advertising in & around popular casual stations
Popular casual stations such as Streeter Dr/Grand Ave and DuSable Lake Shore Dr/Monroe St offer high visibility to casual riders

3. Increase casual price during the weekends to incentivize membership purchases
Since weekends are the most popular casual days, casual riders may opt for an annual membership if the daily prices are going to be higher on the weekends



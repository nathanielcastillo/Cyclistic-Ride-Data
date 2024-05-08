# Cyclistic-Case-Study


# Introduction

Cyclistic is a bike-share company in Chicago. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships.
Therefore, your team wants to understand how casual riders and annual memebers use Cyclistic bikes differently.
From these insights, your team will design a new maketing strategy to convert casual riders into annual members.
But first, Cyclistic excutives must approve your recommendations, so they must be backed up with compelling data insights and professional data visualizations.

# Ask
### Main Business Task

* Analyze how members and casual riders use Cyclistic bikes differently. 

* Provide reccomendations to convert casual riders into annual members based on the findings.

### Stakeholders

* Lily Moreno: Director of marketing 

* Cyclistic Executive Team: Will decide whether to appove the reccomended marketing program

* Cyclistc Marketing Analytics Team: Team of data analysts reposonsible for guiding Cyclistic marketing strategey

# Prepare
### Data Source

Data is downloaded from https://divvy-tripdata.s3.amazonaws.com/index.html

Trip data is organized into 1 CSV file per month for a total of 12 CSVs for the year 2023

Data deemed as credible and passes ROCCC method

- [x] Reliable - High Sample Size, over 5 million rows of data    
- [x] Original - Public Dataset provided by Motivate International Inc. 
- [x] Comprehensive - 5 million rows with 13 columns of relevant data
- [x] Current - Data is from 2023
- [x] Cited - Motivate International Inc.

Note that data-privacy issues prevent access from rider's personally identifiable information.
Only individual ride information is collected, information about the riders themselves is not.
Therefore, unable to determine things like if casual riders live within Cyclistic service areas or if they purchased multiple single passes.

Data needs to be cleaned before further analysis due to data quality issues such as null values.

The 12 CSVs of data will be used

202301-divvy-tripdata.csv - 190301 rows of data  
202302-divvy-tripdata.csv - 190445 rows of data   
202303-divvy-tripdata.csv - 258678 rows of data  
202304-divvy-tripdata.csv - 426590 rows of data  
202305-divvy-tripdata.csv - 604827 rows of data  
202306-divvy-tripdata.csv - 719618 rows of data  
202307-divvy-tripdata.csv - 767650 rows of data  
202308-divvy-tripdata.csv - 771693 rows of data  
202309-divvy-tripdata.csv - 666371 rows of data  
202310-divvy-tripdata.csv - 537113 rows of data  
202311-divvy-tripdata.csv - 362518 rows of data  
202312-divvy-tripdata.csv - 224073 rows of data

Total - 5719877 rows of data

# Process
### Data Cleaning Tools

Data cleaning will be done in MySQL as there are a total of over 5 million rows of data which is too much for Microsoft Excel to process efficiently  
The code used will be documented below  

### Creating Master Table
This will hold all the data from the 12 CSVs as 2023_ride_data

``` MySQL
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
LOAD DATA INFILE is used to achieve best performance   
File path is dependent on where file is stored

``` MySQL
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
```
* 2023_ride_data - 5719877 rows imported

### Deleting rows with NULL or blank values
Several station names are blank and cannot be identified even when using coordinate information

``` MySQL
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
```
* 1388170 rows deleted   
* New 2023_ride_data total - 4331707 rows 

### Cleaning Station names
Station Name variations lead to unnecessary station duplicates
``` MySQL
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
```
* 151158 rows affected  
* Did not delete rows so table total remains same

### Deleting rides with "Test" in Station names
"Test" rides will be deleted   
``` MySQL
DELETE 
FROM 2023_ride_data
WHERE start_station_name LIKE "%Test%" OR end_station_name LIKE "%Test%"
;
```
* 15 rows deleted  
* New 2023_ride_data total - 4331692 rows

### Setting ride_id column to 16 characters
Standardizng Ride IDs
``` MySQL
UPDATE 2023_ride_data
SET ride_id = LEFT(ride_id, 16);
```
* 484 rows affected
* Did not delete rows so table total remains same
 
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
```
* 257 rows affected
* Did not delete rows so table total remains same

### Dropping start_station_id and end_station_id columns
Station ID columns are not useful or necessary

``` MySQL
ALTER TABLE 2023_ride_data
DROP COLUMN start_station_id,
DROP COLUMN end_station_id
;
```

### Creating Temporary Table with Window function to add row number to duplicates
Window function will assign row numbers to entries to determine if duplicate entries exist  
The first ride entry prioritizing member rides will marked with row number 1  
Any duplicate entry will be marked with a row number > 1  
Result is stored in temporary table for later use  

```MySQL
DROP TABLE IF EXISTS member_row;
CREATE TEMPORARY TABLE member_row
SELECT 
            ride_id, rideable_type, started_at, ended_at, start_station_name, end_station_name, start_lat, start_lng, end_lat, end_lng, member_casual,
            ROW_NUMBER() OVER (PARTITION BY ride_id, rideable_type, started_at, ended_at, start_station_name, end_station_name ORDER BY member_casual DESC) AS row_num
FROM 2023_ride_data
ORDER BY ride_id
;
```
* member_row - temporary table created with 4331692 rows

### "Deleting" duplicates prioritzing member rides
We are only importing where row num = 1   
If duplicate entries exists, they are "deleted" by filtering out row numbers != 1 from the previous temporary table  

```MySQL
TRUNCATE TABLE 2023_ride_data; 
INSERT INTO 2023_ride_data  
SELECT ride_id, rideable_type, started_at, ended_at, start_station_name, end_station_name, start_lat, start_lng, end_lat, end_lng, member_casual
FROM member_row
WHERE row_num = 1
ORDER BY ride_id
;
```
* New 2023_ride_data total - 4331692 (No duplicates)

### Deleting rides with trip durations over 1 day or negative ride duration  
Likely technical errors or extreme outliers so they are deleted
```MySQL
DELETE
FROM 2023_ride_data
WHERE TIMESTAMPDIFF (YEAR, started_at, ended_at) > 0
OR TIMESTAMPDIFF (MONTH, started_at, ended_at) > 0
OR TIMESTAMPDIFF (DAY, started_at, ended_at) > 0
OR TIMESTAMPDIFF (MINUTE, started_at, ended_at) < 0
;
```
* 167 rows deleted
* New 2023_ride_data total - 4331525 rows
  
### Deleting rides with same start and end station and ride_durations under 1 minute 
If these conditions are met then it is mostly likely a test or accident so they are deleted
```MySQL
DELETE
FROM 2023_ride_data
WHERE start_station_name = end_station_name AND TIMESTAMPDIFF (MINUTE, started_at, ended_at) < 1
ORDER BY ride_id
;
```
* 84179 rows deleted
* New 2023_ride_data total - 4247346 rows

## Additions 
Now that the data have been cleaned, two columns are added for analysis

### Adding column route
To analyze popular start station + end station combination
```MySQL
ALTER TABLE 2023_ride_data
ADD COLUMN ride_route VARCHAR(255) AS (CONCAT(start_station_name, ' - ', end_station_name))
;
```
### Adding column trip duration
To analyze trip duration in minutes
```MySQL
ALTER TABLE 2023_ride_data
ADD COLUMN ride_duration_min INT AS (TIMESTAMPDIFF(MINUTE, started_at, ended_at))
;
```
# Analyze & Share

The Data has been analyzed and visualized in Tableau 

[2023 Cyclistic Ride Data Story 
](https://public.tableau.com/views/2023CyclisticRideDataStory/RideDataStory?:language=en-US&:sid=&:display_count=n&:origin=viz_share_link)

[2023 Cyclistic Ride Data Dashboard
](https://public.tableau.com/views/2023CyclisticRideDataDashboard/RideDataDashboard?:language=en-US&:sid=&:display_count=n&:origin=viz_share_link)

![
](https://github.com/nathanielcastillo/Cyclistic-Case-Study/blob/main/Images/Total%20Ride%20Count%20per%20Month.png)

![
](https://github.com/nathanielcastillo/Cyclistic-Case-Study/blob/main/Images/test.png)


# Act
## Reccomendations



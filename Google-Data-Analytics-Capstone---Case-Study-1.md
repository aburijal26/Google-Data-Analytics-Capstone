Google Data Analytics Capstone - Case Study 1
================
Abu RIjal

# Summary

Cyclistic has a fleet of 5,824 bikes that are geo-tracked and locked
into a network of 692 stations across Chicago. Cyclistic financial
analysts have concluded that annual members are much more profitable
than regular riders. Designing a marketing strategy aimed at converting
regular riders into annual members is the key to future growth.

<br>

# PHASE 1 : ASK

##### **Key objectives**:

1.Identify the business task:

-   The company wants to increase revenue by appealing to its “casual”
    riders. To do this, they must first determine how the “casual” and
    annual customers vary in order to develop a targeted and effective
    marketing message that would persuade the “casual” customers to
    switch to an annual membership.

2.Consider key stakeholders:

-   Lily Moreno: Marketing director and manager.
-   Cyclistic marketing analytics team: A team of data analysts
    responsible for collecting, analyzing and reporting data that helps
    guide Cyclistic’s marketing strategy.
-   Cyclistic executive team: A highly detail-oriented executive team
    will decide whether they approve the recommended marketing program.

3.The business task:

-   The main objective is to design marketing strategies aimed at
    converting casual riders to annual members by understanding how they
    differ.

<br>

# PHASE 2 : Prepare

##### **Key objectives**:

1.Determine the credibility of the data:

-   The data is open access from a bike-sharing company. There isn’t
    much of a naming system because the files are occasionally sorted by
    quarter, month, or year, and their names change a lot. It runs from
    the year 2013 to August 2022. Over time, some columns are added and
    removed, and the names of the columns also change. Despite this, the
    information appears to be accurate and was gathered directly by the
    business. It has a large number of entries and a wealth of relevant
    information.

2.Sort and filter the data:

-   For this analysis I’m going to focus on the last 12 Months
    (September 2021 - August 2022) period as it’s the more relevant
    period to the business task and it has the more complete data with
    geo-location coordinates, and types of bike used.

3.Install and load necessary packages:

-   `tidyverse`
-   `lubridate`
-   `ggplot2`
-   `dplyr`
-   `geosphere`

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.3.6      ✔ purrr   0.3.4 
    ## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
    ## ✔ tidyr   1.2.1      ✔ stringr 1.4.1 
    ## ✔ readr   2.1.3      ✔ forcats 0.5.2 
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(lubridate)
```

    ## 
    ## Attaching package: 'lubridate'
    ## 
    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

``` r
library(ggplot2)
library(dplyr)
library(geosphere)
```

4.Import data to R studio and Merge data

``` r
september21= read_csv('202109-divvy-tripdata.csv')
```

    ## Rows: 756147 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
october21= read_csv('202110-divvy-tripdata.csv')
```

    ## Rows: 631226 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
november21= read_csv('202111-divvy-tripdata.csv')
```

    ## Rows: 359978 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
december21= read_csv('202112-divvy-tripdata.csv')
```

    ## Rows: 247540 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
january22= read_csv('202201-divvy-tripdata.csv')
```

    ## Rows: 103770 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
february22= read_csv('202202-divvy-tripdata.csv')
```

    ## Rows: 115609 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
march22= read_csv('202203-divvy-tripdata.csv')
```

    ## Rows: 284042 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
april22= read_csv('202204-divvy-tripdata.csv')
```

    ## Rows: 371249 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
may22= read_csv('202205-divvy-tripdata.csv')
```

    ## Rows: 634858 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
june22= read_csv('202206-divvy-tripdata.csv')
```

    ## Rows: 769204 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
july22= read_csv('202207-divvy-tripdata.csv')
```

    ## Rows: 823488 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
august22= read_csv('202208-divvy-tripdata.csv')
```

    ## Rows: 785932 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

here I put together all the data from september 2021 to august 2022

``` r
trip_data = bind_rows(september21,  october21,  november21, december21, january22,  february22, march22,    april22,    may22,  june22, july22, august22)
```

# PHASE 3 : Process

##### **Key objectives**:

1.Clean the data, and prepare the data for analysis:

-   Now that all the data is in one location, we can begin to remove any
    mistakes like NA. In order to speed up our research and produce more
    insightful results, we will also make some adjustments to the data,
    adding relevant new columns based on calculations of previously
    existing columns.

``` r
glimpse(trip_data)
```

    ## Rows: 5,883,043
    ## Columns: 13
    ## $ ride_id            <chr> "9DC7B962304CBFD8", "F930E2C6872D6B32", "6EF7213790…
    ## $ rideable_type      <chr> "electric_bike", "electric_bike", "electric_bike", …
    ## $ started_at         <dttm> 2021-09-28 16:07:10, 2021-09-28 14:24:51, 2021-09-…
    ## $ ended_at           <dttm> 2021-09-28 16:09:54, 2021-09-28 14:40:05, 2021-09-…
    ## $ start_station_name <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, "Clark St &…
    ## $ start_station_id   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, "TA13070001…
    ## $ end_station_name   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,…
    ## $ end_station_id     <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,…
    ## $ start_lat          <dbl> 41.89000, 41.94000, 41.81000, 41.80000, 41.88000, 4…
    ## $ start_lng          <dbl> -87.68000, -87.64000, -87.72000, -87.72000, -87.740…
    ## $ end_lat            <dbl> 41.89, 41.98, 41.80, 41.81, 41.88, 41.88, 41.74, 41…
    ## $ end_lng            <dbl> -87.67, -87.67, -87.72, -87.72, -87.71, -87.74, -87…
    ## $ member_casual      <chr> "casual", "casual", "casual", "casual", "casual", "…

``` r
summary(trip_data)
```

    ##    ride_id          rideable_type        started_at                    
    ##  Length:5883043     Length:5883043     Min.   :2021-09-01 00:00:06.00  
    ##  Class :character   Class :character   1st Qu.:2021-11-06 13:47:33.00  
    ##  Mode  :character   Mode  :character   Median :2022-05-07 12:30:47.00  
    ##                                        Mean   :2022-03-22 05:41:41.57  
    ##                                        3rd Qu.:2022-07-06 16:00:29.50  
    ##                                        Max.   :2022-08-31 23:59:39.00  
    ##                                                                        
    ##     ended_at                      start_station_name start_station_id  
    ##  Min.   :2021-09-01 00:00:41.00   Length:5883043     Length:5883043    
    ##  1st Qu.:2021-11-06 14:07:58.50   Class :character   Class :character  
    ##  Median :2022-05-07 12:52:55.00   Mode  :character   Mode  :character  
    ##  Mean   :2022-03-22 06:01:26.78                                        
    ##  3rd Qu.:2022-07-06 16:18:47.00                                        
    ##  Max.   :2022-09-06 21:49:04.00                                        
    ##                                                                        
    ##  end_station_name   end_station_id       start_lat       start_lng     
    ##  Length:5883043     Length:5883043     Min.   :41.64   Min.   :-87.84  
    ##  Class :character   Class :character   1st Qu.:41.88   1st Qu.:-87.66  
    ##  Mode  :character   Mode  :character   Median :41.90   Median :-87.64  
    ##                                        Mean   :41.90   Mean   :-87.65  
    ##                                        3rd Qu.:41.93   3rd Qu.:-87.63  
    ##                                        Max.   :45.64   Max.   :-73.80  
    ##                                                                        
    ##     end_lat         end_lng       member_casual     
    ##  Min.   :41.39   Min.   :-88.97   Length:5883043    
    ##  1st Qu.:41.88   1st Qu.:-87.66   Class :character  
    ##  Median :41.90   Median :-87.64   Mode  :character  
    ##  Mean   :41.90   Mean   :-87.65                     
    ##  3rd Qu.:41.93   3rd Qu.:-87.63                     
    ##  Max.   :42.37   Max.   :-87.50                     
    ##  NA's   :5727    NA's   :5727

drop All NA.

``` r
trip_data_clean = drop_na(trip_data)
```

Adding columns for date, month, year, day of the week into the data
frame.

``` r
trip_data_clean$date <- as.Date(trip_data_clean$started_at) 
trip_data_clean$month <- format(as.Date(trip_data_clean$date), "%m")
trip_data_clean$day <- format(as.Date(trip_data_clean$date), "%d")
trip_data_clean$year <- format(as.Date(trip_data_clean$date), "%Y")
trip_data_clean$day_of_week <- format(as.Date(trip_data_clean$date), "%A")
colnames(trip_data_clean) #to get the names of all the columns
```

    ##  [1] "ride_id"            "rideable_type"      "started_at"        
    ##  [4] "ended_at"           "start_station_name" "start_station_id"  
    ##  [7] "end_station_name"   "end_station_id"     "start_lat"         
    ## [10] "start_lng"          "end_lat"            "end_lng"           
    ## [13] "member_casual"      "date"               "month"             
    ## [16] "day"                "year"               "day_of_week"

create a new column named “ride_length.” Calculate the length of each
trip by subtracting the “started_at” column from the “ended_at” column

``` r
trip_data_clean$ride_length = difftime(trip_data_clean$ended_at,trip_data_clean$started_at)
```

create a new column named “ride_distance”

``` r
trip_data_clean$ride_distance <- distGeo(matrix(c(trip_data_clean$start_lng, trip_data_clean$start_lat), ncol = 2), matrix(c(trip_data_clean$end_lng, trip_data_clean$end_lat), ncol = 2))

trip_data_clean$ride_distance <- trip_data_clean$ride_distance/1000
```

Check the detail of clean data frame

``` r
glimpse(trip_data_clean)
```

    ## Rows: 4,560,146
    ## Columns: 20
    ## $ ride_id            <chr> "E92C804563F261EC", "9ECA91210441E847", "3DAA144C4E…
    ## $ rideable_type      <chr> "classic_bike", "classic_bike", "classic_bike", "cl…
    ## $ started_at         <dttm> 2021-09-05 01:25:08, 2021-09-05 13:33:41, 2021-09-…
    ## $ ended_at           <dttm> 2021-09-05 01:35:46, 2021-09-05 14:06:08, 2021-09-…
    ## $ start_station_name <chr> "Wells St & Walton St", "Larrabee St & Armitage Ave…
    ## $ start_station_id   <chr> "TA1306000011", "TA1309000006", "TA1305000006", "KA…
    ## $ end_station_name   <chr> "Desplaines St & Kinzie St", "Clark St & Leland Ave…
    ## $ end_station_id     <chr> "TA1306000003", "TA1309000014", "TA1305000006", "TA…
    ## $ start_lat          <dbl> 41.89993, 41.91808, 41.88132, 41.88918, 41.90096, 4…
    ## $ start_lng          <dbl> -87.63443, -87.64375, -87.62952, -87.63851, -87.623…
    ## $ end_lat            <dbl> 41.88872, 41.96710, 41.88132, 41.90292, 41.90096, 4…
    ## $ end_lng            <dbl> -87.64445, -87.66743, -87.62952, -87.63772, -87.623…
    ## $ member_casual      <chr> "casual", "casual", "casual", "casual", "casual", "…
    ## $ date               <date> 2021-09-05, 2021-09-05, 2021-09-04, 2021-09-14, 20…
    ## $ month              <chr> "09", "09", "09", "09", "09", "09", "09", "09", "09…
    ## $ day                <chr> "05", "05", "04", "14", "13", "14", "04", "21", "13…
    ## $ year               <chr> "2021", "2021", "2021", "2021", "2021", "2021", "20…
    ## $ day_of_week        <chr> "Sunday", "Sunday", "Saturday", "Tuesday", "Monday"…
    ## $ ride_length        <drtn> 638 secs, 1947 secs, 579 secs, 587 secs, 4 secs, 6…
    ## $ ride_distance      <dbl> 1.497511954, 5.787201780, 0.000000000, 1.528324769,…

``` r
summary(trip_data_clean)
```

    ##    ride_id          rideable_type        started_at                    
    ##  Length:4560146     Length:4560146     Min.   :2021-09-01 00:00:06.00  
    ##  Class :character   Class :character   1st Qu.:2021-11-04 18:45:57.00  
    ##  Mode  :character   Mode  :character   Median :2022-05-09 21:54:37.50  
    ##                                        Mean   :2022-03-22 14:56:07.29  
    ##                                        3rd Qu.:2022-07-06 14:20:31.75  
    ##                                        Max.   :2022-08-31 23:58:50.00  
    ##     ended_at                      start_station_name start_station_id  
    ##  Min.   :2021-09-01 00:03:37.00   Length:4560146     Length:4560146    
    ##  1st Qu.:2021-11-04 18:57:43.25   Class :character   Class :character  
    ##  Median :2022-05-09 22:14:53.00   Mode  :character   Mode  :character  
    ##  Mean   :2022-03-22 15:14:07.24                                        
    ##  3rd Qu.:2022-07-06 14:37:10.75                                        
    ##  Max.   :2022-09-01 19:10:49.00                                        
    ##  end_station_name   end_station_id       start_lat       start_lng     
    ##  Length:4560146     Length:4560146     Min.   :41.65   Min.   :-87.83  
    ##  Class :character   Class :character   1st Qu.:41.88   1st Qu.:-87.66  
    ##  Mode  :character   Mode  :character   Median :41.90   Median :-87.64  
    ##                                        Mean   :41.90   Mean   :-87.64  
    ##                                        3rd Qu.:41.93   3rd Qu.:-87.63  
    ##                                        Max.   :45.64   Max.   :-73.80  
    ##     end_lat         end_lng       member_casual           date           
    ##  Min.   :41.65   Min.   :-87.83   Length:4560146     Min.   :2021-09-01  
    ##  1st Qu.:41.88   1st Qu.:-87.66   Class :character   1st Qu.:2021-11-04  
    ##  Median :41.90   Median :-87.64   Mode  :character   Median :2022-05-09  
    ##  Mean   :41.90   Mean   :-87.64                      Mean   :2022-03-22  
    ##  3rd Qu.:41.93   3rd Qu.:-87.63                      3rd Qu.:2022-07-06  
    ##  Max.   :42.17   Max.   :-87.53                      Max.   :2022-08-31  
    ##     month               day                year           day_of_week       
    ##  Length:4560146     Length:4560146     Length:4560146     Length:4560146    
    ##  Class :character   Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##  ride_length       ride_distance     
    ##  Length:4560146    Min.   :   0.000  
    ##  Class :difftime   1st Qu.:   0.891  
    ##  Mode  :numeric    Median :   1.586  
    ##                    Mean   :   2.106  
    ##                    3rd Qu.:   2.764  
    ##                    Max.   :1192.246

# PHASE 4 : Analyze

##### **Key objectives**:

1.Identify trends and relationships:

-   We now have a complete data frame with all of the information we
    need to detect the behavioral differences between casual and member
    users.
-   Then we export the data frame into a csv file and perform analysis
    and visualization through TABLEAU

``` r
write.csv(trip_data_clean,"Z:\\FGA Google Data Analytics\\Course 8\\Capstone\\trip_data_clean.csv", row.names = FALSE)
```

Here’s a
[Link](https://public.tableau.com/views/GoogleDataAnalyticsCapstone-CyclisticBikeshareDashboard/GoogleDataAnalyticsCapstone-Dashboard?:language=en-US&:display_count=n&:origin=viz_share_link)
to the results of my Tableau visualization.

# PHASE 5 : Share

##### **Key objectives**:

1.Share my conclusions:

-   Casual users generally ride electric bikes on the weekends for
    recreation and tourism.
-   The member users commute or go for practical rides every day of the
    week on both electric and vintage bikes.

I would give the marketing team this information, the data, and my
analysis, and I would suggest that it would be interesting to focus the
messages on the leisure aspect of the service and perhaps offer some
kind of promotion related to weekends and/or electric bikes in order to
convert the casual users to the annual users.

### Thank you for reading, and I hope you enjoy it. I also want to gather feedback to improve it in the future.

Contact :

[Muhammad Abu Rijal
Kusnaedi](https://www.linkedin.com/in/muhammad-abu-rijal-kusnaedi/)

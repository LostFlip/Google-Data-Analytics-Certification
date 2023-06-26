# Intro
We are a junior data analyst for Cyclistic, a company providing bike-sharing services in Chicago. Riders may opt to purchase single-ride or full-day passes for casual use or sign up for the annual Cyclistic membership. Lily Moreno, the director of marketing, believes that maximizing the number of annual memberships is the way to go in ensuring the company's growth. In order to effectively create marketing strategies supported by this claim, we need to examine more closely the difference of a casual and member rider and analyze how they utilize the service provided to them. Given the usage data of all the trips done within the last year, along with the help of our chosen spreadsheet/data visualization software, we have everything we need to probe and extract insight that are useful and valuable to the company.

The following study will be split into chapters following the steps in the data analysis process: ask, prepare, process, analyze, share and act. 

# Chapter 1: Ask
In this case study, we are trying to establish what differentiates a casual rider to a annual member, identifying the quantifiable metrics or data that is generated through each transaction they have with Cyclistic. By identifying these, we can formulate a marketing strategy driven by facts and data given our better understanding to the needs of each type of riders. We will also challenge or provide an input whether if annual membership is the best way to secure company growth.

# Chapter 2: Prepare
The data is stored internally within the company’s boundaries but is shared publicly through file hosting platforms and/or online storage websites. The data is generated monthly and also follows this trend when considering how the data is organized. The database contains information regarding the transactions users have done with the company services such as what kind of bike, start and end locations, longitudinal and latitudinal data etc. It may also be observed that not all of the data inputted has complete information so it is wise to perform inspection and data cleaning before proceeding with the analysis of the data.

Data is obtained through the various transaction mediums of Cyclistic, which means we are reading through data that is reliable and also relevant to our purpose. For the same reason, we may also say that the data is original since we are obtaining the data firsthand through the official public sources. Reviewing the questions that need to be answered in this presentation, it is comprehensive enough to be able to draw out valuable insights that may translate into profit. Since starting this analysis on February 2023, the company has been actively updating their records for the analysts to sift through the latest data. Lastly, we are able to cite Motivate International Inc. as the primary data provider for this analysis.

The dataset comes with a license in which the providers allow its users to use and perform analysis on the database given that the users will not breach any of the indicated clauses in the license. Since the data contains millions of rows across the 12-month scope of the data, a spreadsheet program would have a difficult time processing and verifying whether the data has any null or error values. Once cleaned, we are left with raw data with relevant information waiting for the analysts to discover and answer the business questions that bring value to the stakeholders.

# Chapter 3: Process
Before proceeding with any kind of insight gathering, the dataset must first be checked and cleaned for any irrelevant or dummy entries. Our software of choice to perform this will be RStudio, since we can also use its data visualization features later on in the project. Lets start the data cleaning process by opening a New Project in RStudio and writing the code.

To start with, we will set the working directory of the project in our computer. This will allow us to load straight into the project whenever we open RStudio, as well as identifying the working locations of the data visualization we will create later on.
```
setwd("C:/Users/ADMIN/Documents/Google")
```
We will now install the tidyverse package which will supply us the tools we need to properly clean, format and visualize the data. Since we are using a newer version of RStudio, tidyverse already comes with the other packages we need. We use the library function to load the packages we need.
```
install.packages("tidyverse")
library(tidyverse)
library(ggplot2)
library(lubridate)
library(dplyr)
```
```
> install.packages("tidyverse")
WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

https://cran.rstudio.com/bin/windows/Rtools/
trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.2/tidyverse_2.0.0.zip'
Content type 'application/zip' length 430921 bytes (420 KB)
downloaded 420 KB

package ‘tidyverse’ successfully unpacked and MD5 sums checked

The downloaded binary packages are in
	C:\Users\ADMIN\AppData\Local\Temp\Rtmp2Hjktf\downloaded_packages
> library(tidyverse)
── Attaching core tidyverse packages ──────────────── tidyverse 2.0.0 ──
✔ dplyr     1.1.2     ✔ readr     2.1.4
✔ forcats   1.0.0     ✔ stringr   1.5.0
✔ ggplot2   3.4.2     ✔ tibble    3.2.1
✔ lubridate 1.9.2     ✔ tidyr     1.3.0
✔ purrr     1.0.1     
── Conflicts ────────────────────────────────── tidyverse_conflicts() ──
✖ dplyr::filter() masks stats::filter()
✖ dplyr::lag()    masks stats::lag()
ℹ Use the conflicted package to force all conflicts to become errors
Warning messages:
1: package ‘tidyverse’ was built under R version 4.2.3 
2: package ‘ggplot2’ was built under R version 4.2.3 
3: package ‘tibble’ was built under R version 4.2.3 
4: package ‘readr’ was built under R version 4.2.3 
5: package ‘dplyr’ was built under R version 4.2.3 
6: package ‘lubridate’ was built under R version 4.2.3
>library(ggplot2)
>library(lubridate)
>library(dplyr)
```
Before proceeding to the next step, the downloaded data set should already be stored in the same location as the working directory. read.csv will import the .csv files as a data frame we can work on in Rstudio.
```
Feb2022 <- read.csv("202202-divvy-tripdata.csv")
Mar2022 <- read.csv("202203-divvy-tripdata.csv")
Apr2022 <- read.csv("202204-divvy-tripdata.csv")
May2022 <- read.csv("202205-divvy-tripdata.csv")
Jun2022 <- read.csv("202206-divvy-tripdata.csv")
Jul2022 <- read.csv("202207-divvy-tripdata.csv")
Aug2022 <- read.csv("202208-divvy-tripdata.csv")
Sep2022 <- read.csv("202209-divvy-publictripdata.csv")
Oct2022 <- read.csv("202210-divvy-tripdata.csv")
Nov2022 <- read.csv("202211-divvy-tripdata.csv")
Dec2022 <- read.csv("202212-divvy-tripdata.csv")
Jan2023 <- read.csv("202301-divvy-tripdata.csv")
```
Next up, we combine the data frame representing each month into one data frame to make data cleaning manageable. The other data frames will be removed to conserve computer storage space by the rm function. We can review the data frame by clicking on the Environment panel or by the View function.
```
trip_data <- bind_rows(Feb2022, Mar2022, Apr2022, May2022, Jun2022, Jul2022, Aug2022, Sep2022, Oct2022, Nov2022, Dec2022, Jan2023)
rm(Feb2022, Mar2022, Apr2022, May2022, Jun2022, Jul2022, Aug2022, Sep2022, Oct2022, Nov2022, Dec2022, Jan2023)
View(trip_data)
```
The following functions can be run in order to initially inspect the data frame we made. 
```
str(trip_data)
```
```
'data.frame':	5754248 obs. of  13 variables:
 $ ride_id           : chr  "E1E065E7ED285C02" "1602DCDC5B30FFE3" "BE7DD2AF4B55C4AF" "A1789BDF844412BE" ...
 $ rideable_type     : chr  "classic_bike" "classic_bike" "classic_bike" "classic_bike" ...
 $ started_at        : chr  "2022-02-19 18:08:41" "2022-02-20 17:41:30" "2022-02-25 18:55:56" "2022-02-14 11:57:03" ...
 $ ended_at          : chr  "2022-02-19 18:23:56" "2022-02-20 17:45:56" "2022-02-25 19:09:34" "2022-02-14 12:04:00" ...
 $ start_station_name: chr  "State St & Randolph St" "Halsted St & Wrightwood Ave" "State St & Randolph St" "Southport Ave & Waveland Ave" ...
 $ start_station_id  : chr  "TA1305000029" "TA1309000061" "TA1305000029" "13235" ...
 $ end_station_name  : chr  "Clark St & Lincoln Ave" "Southport Ave & Wrightwood Ave" "Canal St & Adams St" "Broadway & Sheridan Rd" ...
 $ end_station_id    : chr  "13179" "TA1307000113" "13011" "13323" ...
 $ start_lat         : num  41.9 41.9 41.9 41.9 41.9 ...
 $ start_lng         : num  -87.6 -87.6 -87.6 -87.7 -87.6 ...
 $ end_lat           : num  41.9 41.9 41.9 42 41.9 ...
 $ end_lng           : num  -87.6 -87.7 -87.6 -87.6 -87.6 ...
 $ member_casual     : chr  "member" "member" "member" "member" ...
```
```
glimpse(trip_data)
```
```
Rows: 5,754,248
Columns: 13
$ ride_id            <chr> "E1E065E7ED285C02", "1602DCDC5B30FFE3", "BE…
$ rideable_type      <chr> "classic_bike", "classic_bike", "classic_bi…
$ started_at         <chr> "2022-02-19 18:08:41", "2022-02-20 17:41:30…
$ ended_at           <chr> "2022-02-19 18:23:56", "2022-02-20 17:45:56…
$ start_station_name <chr> "State St & Randolph St", "Halsted St & Wri…
$ start_station_id   <chr> "TA1305000029", "TA1309000061", "TA13050000…
$ end_station_name   <chr> "Clark St & Lincoln Ave", "Southport Ave & …
$ end_station_id     <chr> "13179", "TA1307000113", "13011", "13323", …
$ start_lat          <dbl> 41.88462, 41.92914, 41.88462, 41.94815, 41.…
$ start_lng          <dbl> -87.62783, -87.64908, -87.62783, -87.66394,…
$ end_lat            <dbl> 41.91569, 41.92877, 41.87926, 41.95283, 41.…
$ end_lng            <dbl> -87.63460, -87.66391, -87.63990, -87.64999,…
$ member_casual      <chr> "member", "member", "member", "member", "me…
```
```
head(trip_data)
```
```
           ride_id rideable_type          started_at
1 E1E065E7ED285C02  classic_bike 2022-02-19 18:08:41
2 1602DCDC5B30FFE3  classic_bike 2022-02-20 17:41:30
3 BE7DD2AF4B55C4AF  classic_bike 2022-02-25 18:55:56
4 A1789BDF844412BE  classic_bike 2022-02-14 11:57:03
5 07DE78092C62F7B3  classic_bike 2022-02-16 05:36:06
6 9A2F204F04AB7E24  classic_bike 2022-02-07 09:51:57
             ended_at           start_station_name start_station_id
1 2022-02-19 18:23:56       State St & Randolph St     TA1305000029
2 2022-02-20 17:45:56  Halsted St & Wrightwood Ave     TA1309000061
3 2022-02-25 19:09:34       State St & Randolph St     TA1305000029
4 2022-02-14 12:04:00 Southport Ave & Waveland Ave            13235
5 2022-02-16 05:39:00       State St & Randolph St     TA1305000029
6 2022-02-07 10:07:53       St. Clair St & Erie St            13016
                end_station_name end_station_id start_lat start_lng
1         Clark St & Lincoln Ave          13179  41.88462 -87.62783
2 Southport Ave & Wrightwood Ave   TA1307000113  41.92914 -87.64908
3            Canal St & Adams St          13011  41.88462 -87.62783
4         Broadway & Sheridan Rd          13323  41.94815 -87.66394
5          Franklin St & Lake St   TA1307000111  41.88462 -87.62783
6        Franklin St & Monroe St   TA1309000007  41.89435 -87.62280
   end_lat   end_lng member_casual
1 41.91569 -87.63460        member
2 41.92877 -87.66391        member
3 41.87926 -87.63990        member
4 41.95283 -87.64999        member
5 41.88584 -87.63550        member
6 41.88032 -87.63519        member
```
```
tail(trip_data)
```
```
                 ride_id rideable_type          started_at
5754243 A3DC3E8358DB1FAA electric_bike 2023-01-17 18:36:00
5754244 A303816F2E8A35A8 electric_bike 2023-01-11 17:46:23
5754245 BCDBB142CC610382  classic_bike 2023-01-30 15:08:10
5754246 7D1C7CA80517183B  classic_bike 2023-01-06 19:34:50
5754247 1A4EB636346DF527  classic_bike 2023-01-13 18:59:24
5754248 069971675AC7DC62 electric_bike 2023-01-02 13:48:29
                   ended_at       start_station_name start_station_id
5754243 2023-01-17 19:00:26        Clark St & Elm St     TA1307000039
5754244 2023-01-11 17:57:31        Clark St & Elm St     TA1307000039
5754245 2023-01-30 15:33:26 Western Ave & Leland Ave     TA1307000140
5754246 2023-01-06 19:50:01        Clark St & Elm St     TA1307000039
5754247 2023-01-13 19:14:44        Clark St & Elm St     TA1307000039
5754248 2023-01-02 13:59:29        Clark St & Elm St     TA1307000039
                    end_station_name end_station_id start_lat start_lng
5754243 Southport Ave & Clybourn Ave   TA1309000030  41.90276 -87.63149
5754244 Southport Ave & Clybourn Ave   TA1309000030  41.90263 -87.63159
5754245   Clarendon Ave & Gordon Ter          13379  41.96640 -87.68870
5754246 Southport Ave & Clybourn Ave   TA1309000030  41.90297 -87.63128
5754247 Southport Ave & Clybourn Ave   TA1309000030  41.90297 -87.63128
5754248 Southport Ave & Clybourn Ave   TA1309000030  41.90282 -87.63169
         end_lat   end_lng member_casual
5754243 41.92077 -87.66371        casual
5754244 41.92077 -87.66371        casual
5754245 41.95787 -87.64951        member
5754246 41.92077 -87.66371        casual
5754247 41.92077 -87.66371        casual
5754248 41.92077 -87.66371        casual
```
Since we were able to take a quick peek at our data frame, we can see that the data types of our columns pertaining to start and end dates are set as the "chr" type. The as.Date function will allow us to convert this to date for us to be able to extract each part of the date format. Let's use the name of our data frame followed by an $ operator, then the name of our new column to be occupied by each of the data format we need.
```
trip_data$month <- format(as.Date(trip_data$started_at),"%B")
trip_data$day <- format(as.Date(trip_data$started_at),"%d")  
trip_data$year <- format(as.Date(trip_data$started_at),"%Y") 
trip_data$weekday <- weekdays(as.Date(trip_data$started_at))
```
To ensure that we are dealing with a data frame with complete entries, we will use the drop_na function in order to remove these rows. But first we need to assign NA to the applicable data since in RStudio, empty is NOT equal to null or NA data.
```
trip_data_test$start_station_name[trip_data_test$start_station_name==""] <- NA
trip_data_test$start_station_id[trip_data_test$start_station_id==""] <- NA
trip_data_test$end_station_name[trip_data_test$end_station_name==""] <- NA
trip_data_test$end_station_id[trip_data_test$end_station_id==""] <- NA
drop_na(trip_data_test)
```
Now, we will be obtaining the ride time of each entry by subtracting the start and end times. The difftime function allows us also to specify what unit of measurement we will use. Afterwards, we round the result to two decimal places.
```
trip_data$ride_time <- difftime(trip_data$ended_at, trip_data$started_at, units = "mins")
trip_data$ride_time <- round(trip_data$ride_time, 2)
```
To perform further calculations and/or data cleaning, we convert the ride time to numeric data type (instead of date). We then filter out rides in which the ride time is less than zero.
```
trip_data$ride_time <- as.numeric(trip_data$ride_time)
trip_data <- filter(trip_data, ride_time > 0)
```
Our data is now clean! Let's use the summarise function and the corresponding statistical operators to get the mean, median, min and max of our ride times.
```
trip_data %>%
group_by(member_casual) %>%
summarise(avg_ride_length = mean(ride_time), median_ride_length = median(ride_time), max_ride_length = max(ride_time), min_ride_length = min(ride_time)) %>%
print(n=4)
```
```
# A tibble: 2 × 5
  member_casual avg_ride_length median_ride_length max_ride_length
  <chr>                   <dbl>              <dbl>           <dbl>
1 casual                   29.0              12.9           41387.
2 member                   12.6               8.77           1560.
# ℹ 1 more variable: min_ride_length <dbl>
```
We use the ordered function here to ensure the data will follow this heirarchy later during data visualization and in the next step.
```
trip_data$weekday <- ordered(trip_data$weekday, levels=c("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"))
trip_data$month <- ordered(trip_data$month, levels=c("January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"))
```
To separate the statistical data of casual and member riders, we then use the aggregate function to obtain the ride time as sorted by the member type.
```
aggregate(trip_data$ride_time ~ trip_data$member_casual, FUN = mean)
aggregate(trip_data$ride_time ~ trip_data$member_casual, FUN = median)
aggregate(trip_data$ride_time ~ trip_data$member_casual, FUN = max)
aggregate(trip_data$ride_time ~ trip_data$member_casual, FUN = min)
aggregate(trip_data$ride_time ~ trip_data$weekday + trip_data$member_casual, FUN=mean)
```
```
> aggregate(trip_data$ride_time ~ trip_data$member_casual, FUN = mean)
  trip_data$member_casual trip_data$ride_time
1                  casual            29.03219
2                  member            12.62950
> aggregate(trip_data$ride_time ~ trip_data$member_casual, FUN = median)
  trip_data$member_casual trip_data$ride_time
1                  casual               12.92
2                  member                8.77
> aggregate(trip_data$ride_time ~ trip_data$member_casual, FUN = max)
  trip_data$member_casual trip_data$ride_time
1                  casual            41387.25
2                  member             1559.90
> aggregate(trip_data$ride_time ~ trip_data$member_casual, FUN = min)
  trip_data$member_casual trip_data$ride_time
1                  casual                0.02
2                  member                0.02
> aggregate(trip_data$ride_time ~ trip_data$weekday + trip_data$member_casual, FUN=mean)
   trip_data$weekday trip_data$member_casual trip_data$ride_time
1                 Friday                  casual            27.95607
2                 Monday                  casual            29.05488
3               Saturday                  casual            32.50139
4                 Sunday                  casual            34.09580
5               Thursday                  casual            25.40979
6                Tuesday                  casual            25.68575
7              Wednesday                  casual            24.52966
8                 Friday                  member            12.46029
9                 Monday                  member            12.20151
10              Saturday                  member            14.07203
11                Sunday                  member            13.95328
12              Thursday                  member            12.20819
13               Tuesday                  member            12.01598
14             Wednesday                  member            12.02477
```
The next step will create a new data frame arranged firstly by the membership type, then the day of the week. Summarise is used to get the count of each occurrence as well as the average ride time that was recorded on those occurrences.
```
trip_by_day <- trip_data %>%
  group_by(member_casual, weekday) %>%
  summarise(num_of_rides = n(), avg_duration = mean(ride_time)) %>%
  arrange(member_casual, weekday)
```
```
`summarise()` has grouped output by 'member_casual'. You can override
using the `.groups` argument.
```
We are now done processing and cleaning the data and are ready to move on to data visualization. First, let's see how many riders use Cyclistic's ride sharing service across the days of the week by setting our x-axis to describe the weekday, and the y-axis to describe the rider count. Functions that improve the layout and add to the overall readability of the resulting plot may be added after the ggplot syntax.
```
trip_data %>% 
  group_by(member_casual, weekday) %>%
  summarise(num_of_rides = n(), avg_duration = mean(ride_time)) %>%
  arrange(member_casual, weekday)%>%
  ggplot(aes(x = weekday, y=num_of_rides, fill=member_casual)) +
  geom_col(position="dodge2") + 
  labs(title="Total number of Rides (by Weekday)", x="Week Day", y="Number of Rides") + #TOTAL RIDES WEEKDAY
  scale_y_continuous(labels = scales::comma) +
  scale_fill_manual(values = c("green", "#FF00FF"))
```
---
![image](https://github.com/LostFlip/Google-Data-Analytics-Certification/assets/136613906/e944ec70-809c-4b64-a3c2-a0ff99ff1435)
---
Let's quantify what percentage of rider type use the ride sharing service during the weekends versus the weekdays.
```
total_rides_casual_weekend <- NROW(filter(trip_data, member_casual == "casual" & (weekday == "Saturday" | weekday == "Sunday")))
total_rides_member_weekend <- NROW(filter(trip_data, member_casual == "member" & (weekday == "Saturday" | weekday == "Sunday")))
total_rides_casual_weekday <- NROW(filter(trip_data, member_casual == "casual" & !(weekday == "Saturday" | weekday == "Sunday")))
total_rides_member_weekday <- NROW(filter(trip_data, member_casual == "member" & !(weekday == "Saturday" | weekday == "Sunday")))

percent_casual_weekend <-  round(100 * (total_rides_casual_weekend/ (total_rides_casual_weekend+total_rides_member_weekend)),2)
percent_member_weekend <-  round(100 * (total_rides_member_weekend/ (total_rides_casual_weekend+total_rides_member_weekend)),2)
percent_casual_weekday <-  round(100 * (total_rides_casual_weekday/ (total_rides_casual_weekday+total_rides_member_weekday)),2)
percent_member_weekday <-  round(100 * (total_rides_member_weekday/ (total_rides_casual_weekday+total_rides_member_weekday)),2)

percent_weekday <- c(percent_casual_weekday, percent_member_weekday)
percent_weekend <- c(percent_casual_weekend, percent_member_weekend)
week_labeler <- c("Casual", "Member")

pie_weekdays <- paste(week_labeler, percent_weekday)
pie_weekdays <- paste(pie_weekdays, "%", sep="")
pie(percent_weekday, label = pie_weekdays, main = "Member Type Distribution on Weekdays")
```
---
![image](https://github.com/LostFlip/Google-Data-Analytics-Certification/assets/136613906/26af181b-41d5-452a-ac7d-0b0a6e79b20f)
---
```
pie_weekends <- paste(week_labeler, percent_weekend)
pie_weekends <- paste(pie_weekends, "%", sep="")
pie(percent_weekend, label = pie_weekends, main = "Member Type Distribution on Weekend")
```
---
![image](https://github.com/LostFlip/Google-Data-Analytics-Certification/assets/136613906/b0e8bdd8-0920-499d-9c71-f85af45c6fd7)
---
Using the same syntax as the weekday plot, the following will create a plot for the monthly data of Cyclistic riders.
```
trip_data %>% 
  group_by(member_casual, month) %>%
  summarise(num_of_rides = n()) %>%
  arrange(member_casual, month) %>%
  ggplot(aes(x = month, y=num_of_rides, fill=member_casual)) +
  geom_col(position="dodge2") +
  labs(title="Total Number of Rides 
  By Month", x="Month", y="Number of Rides") + #TOTAL RIDES MONTH
  scale_y_continuous(labels = scales::comma) +
  scale_fill_manual(values = c("green", "#FF00FF"))
```
---
![image](https://github.com/LostFlip/Google-Data-Analytics-Certification/assets/136613906/e524f786-1ecf-49e7-bc4d-ec53333f018f)
---
The following plots weekly and monthly data for the rideable type usage.
```
trip_data %>%
  group_by(rideable_type, weekday) %>%
  summarise(num_of_rides = n(), avg_duration = mean(ride_time)) %>%
  arrange(rideable_type, weekday)%>%
  ggplot(aes(x = weekday, y=num_of_rides, fill=rideable_type)) +
  geom_col(position="dodge2") + 
  labs(title="Total number of Rides per type of bike 
  By Weekday", x="Week Day", y="Number of Rides") + #TOTAL RIDES PER BIKE PER WEEK
  scale_y_continuous(labels = scales::comma) +
  scale_fill_manual(values = c("red", "green", "blue"))
```
---
![image](https://github.com/LostFlip/Google-Data-Analytics-Certification/assets/136613906/54a5df76-bb87-4268-be5a-748b3cb97ebf)
---
```
trip_data %>%
  group_by(rideable_type, month) %>%
  summarise(num_of_rides = n(), avg_duration = mean(ride_time)) %>%
  arrange(rideable_type, month)%>%
  ggplot(aes(x = month, y=num_of_rides, fill=rideable_type)) +
  geom_col(position="dodge2") + 
  labs(title="Total number of Rides per type of bike
  By Month", x="Month", y="Number of Rides") + #TOTAL RIDES PER BIKE PER MONTH
  scale_y_continuous(labels = scales::comma) +
  scale_fill_manual(values = c("red", "green", "blue"))
```
---
![image](https://github.com/LostFlip/Google-Data-Analytics-Certification/assets/136613906/b3138a68-93ce-4104-9990-c32c8cd68a51)
---
Creating a data frame for each member type. 
```
trip_data_member <- filter(trip_data, member_casual == "member")
trip_data_casual <- filter(trip_data, member_casual == "casual")
```

```
#Weekly Rideable Type (Member)
trip_data_member %>%
  group_by(rideable_type, weekday) %>%
  summarise(num_of_rides = n(), avg_duration = mean(ride_time)) %>%
  arrange(rideable_type, weekday)%>%
  ggplot(aes(x = weekday, y=num_of_rides, fill=rideable_type)) +
  geom_col(position="dodge2") + 
  labs(title="Total number of Rides per type of bike (Member)
  By Weekday", x="Week Day", y="Number of Rides") + 
  scale_y_continuous(labels = scales::comma) +
  scale_fill_manual(values = c("red", "green", "blue"))
```
---
![image](https://github.com/LostFlip/Google-Data-Analytics-Certification/assets/136613906/d2c5aee4-a798-4962-a59b-fa83b4a6763a)
---
```
#Monthly Rideable Type (Member)
trip_data_member %>%
  group_by(rideable_type, month) %>%
  summarise(num_of_rides = n(), avg_duration = mean(ride_time)) %>%
  arrange(rideable_type, month)%>%
  ggplot(aes(x = month, y=num_of_rides, fill=rideable_type)) +
  geom_col(position="dodge2") + 
  labs(title="Total number of Rides per type of bike (Member)
  By Month", x="Month", y="Number of Rides") +
  scale_y_continuous(labels = scales::comma) +
  scale_fill_manual(values = c("red", "green", "blue"))
```
---
![image](https://github.com/LostFlip/Google-Data-Analytics-Certification/assets/136613906/68ce1918-2279-4bb9-9087-d0302c32db90)
---
```
#Weekly Rideable Type (Casual)
trip_data_casual %>%
  group_by(rideable_type, weekday) %>%
  summarise(num_of_rides = n(), avg_duration = mean(ride_time)) %>%
  arrange(rideable_type, weekday)%>%
  ggplot(aes(x = weekday, y=num_of_rides, fill=rideable_type)) +
  geom_col(position="dodge2") + 
  labs(title="Total number of Rides per type of bike (Member)
  By Weekday", x="Week Day", y="Number of Rides") + 
  scale_y_continuous(labels = scales::comma) +
  scale_fill_manual(values = c("red", "green", "blue"))
```
---
![image](https://github.com/LostFlip/Google-Data-Analytics-Certification/assets/136613906/b2cd940e-a32c-4d8e-8606-be612b871960)
---
```
#Monthly Rideable Type (Casual)
trip_data_casual %>%
  group_by(rideable_type, month) %>%
  summarise(num_of_rides = n(), avg_duration = mean(ride_time)) %>%
  arrange(rideable_type, month)%>%
  ggplot(aes(x = month, y=num_of_rides, fill=rideable_type)) +
  geom_col(position="dodge2") + 
  labs(title="Total number of Rides per type of bike (Casual)
  By Month", x="Month", y="Number of Rides") + 
  scale_y_continuous(labels = scales::comma) +
  scale_fill_manual(values = c("red", "green", "blue"))
```
---
![image](https://github.com/LostFlip/Google-Data-Analytics-Certification/assets/136613906/96a3a318-9d8f-4a7d-9b5f-4ef2e85f9469)
---
$$$ complete the head shit
```
trip_data_member <- trip_data_member %>%
  mutate(route = paste(start_station_name, "to", sep= " "))
trip_data_member <- trip_data_member %>%
  mutate(route = paste(route, end_station_name, sep= " "))

trip_data_casual <- trip_data_member %>%
  mutate(route = paste(start_station_name, "to", sep= " "))
trip_data_casual <- trip_data_member %>%
  mutate(route = paste(route, end_station_name, sep= " "))
```

```

casual_start_station <- trip_data_casual %>%
  group_by(start_station_name) %>%
  summarise(num_of_rides = n(), avg_duration_mins = mean(ride_time)) %>%
  arrange( desc(num_of_rides), avg_duration_mins)

casual_end_station <- trip_data_casual %>%
  group_by(end_station_name) %>%
  summarise(num_of_rides = n(), avg_duration_mins = mean(ride_time)) %>%
  arrange( desc(num_of_rides), avg_duration_mins)

member_start_station <- trip_data_member %>%
  group_by(start_station_name) %>%
  summarise(num_of_rides = n(), avg_duration_mins = mean(ride_time)) %>%
  arrange( desc(num_of_rides), avg_duration_mins)
  
member_end_station <- trip_data_member %>%
  group_by(end_station_name) %>%
  summarise(num_of_rides = n(), avg_duration_mins = mean(ride_time)) %>%
  arrange( desc(num_of_rides), avg_duration_mins)
```

```


5748349

1.	Calculate mean of ride_length
mean(trip_data$ride_time)
2.	Calculate the max ride_length
max(trip_data$ride_time)
3.	Calculate mode of day of week
head(trip_by_day,14)
4.	Average ride_length for casual and members. Try rows = members, values =  average of ride length
mean(trip_data_casual$ride_time)
mean(trip_data_member$ride_time)
5.	Average ride length of users by day of week. Try columns = day of week, rows = members_casual, values = average of ride length
head(trip_by_day,14)
6.	Calculate number of rides for users by day_of_week by adding Count of trip_id to values
head(trip_by_day,14)

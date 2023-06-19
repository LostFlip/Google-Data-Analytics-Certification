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

```
#description
install.packages("tidyverse")

library(tidyverse)
library(ggplot2)
library(lubridate)
library(dplyr)

```
R version 4.2.2 (2022-10-31 ucrt) -- "Innocent and Trusting"
Copyright (C) 2022 The R Foundation for Statistical Computing
Platform: x86_64-w64-mingw32/x64 (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under certain conditions.
Type 'license()' or 'licence()' for distribution details.

R is a collaborative project with many contributors.
Type 'contributors()' for more information and
'citation()' on how to cite R or R packages in publications.

Type 'demo()' for some demos, 'help()' for on-line help, or
'help.start()' for an HTML browser interface to help.
Type 'q()' to quit R.

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

```
trip_data <- bind_rows(Feb2022, Mar2022, Apr2022, May2022, Jun2022, Jul2022, Aug2022, Sep2022, Oct2022, Nov2022, Dec2022, Jan2023)
rm(Feb2022, Mar2022, Apr2022, May2022, Jun2022, Jul2022, Aug2022, Sep2022, Oct2022, Nov2022, Dec2022, Jan2023)
```

```
trip_data$month <- format(as.Date(trip_data$date),"%B")![Type Per Month](https://github.com/LostFlip/Google-Data-Analytics-Certification/assets/136613906/7fed079a-5c23-4656-ba13-076781e97a31)

trip_data$day <- format(as.Date(trip_data$date),"%d")  
trip_data$year <- format(as.Date(trip_data$date),"%Y") 
trip_data$day_of_week <- weekdays(trip_data$date) 
```

```
trip_data_test$start_station_name[trip_data_test$start_station_name==""] <- NA
trip_data_test$start_station_id[trip_data_test$start_station_id==""] <- NA
trip_data_test$end_station_name[trip_data_test$end_station_name==""] <- NA
trip_data_test$end_station_id[trip_data_test$end_station_id==""] <- NA
drop_na(trip_data_test)
```

```
trip_data_station <- trip_data[, c(5, 9, 10)]
NROW(unique(trip_data_station))
```

```
trip_data$ride_time <- difftime(trip_data$ended_at, trip_data$started_at, units = "mins")
trip_data$ride_time <- round(trip_data$ride_time, 2)
```

```
trip_data$ride_time <- as.numeric(as.character(trip_data$ride_time))
trip_data <- filter(trip_data, ride_time > 0)
```

```
trip_data %>%
  +     group_by(member_casual) %>%
  +     summarise(avg_ride_length = mean(ride_time), median_ride_length = median(ride_time), max_ride_length = max(ride_time), min_ride_length = min(ride_time)) %>%
  +     print(n=4)
```

```

trip_data$weekday <- ordered(trip_data$weekday, levels=c("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"))
trip_data$month <- ordered(trip_data$month, levels=c("January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November",
                                                       "December"))
```

```
aggregate(trip_data$ride_time ~ trip_data$member_casual, FUN = mean)
aggregate(trip_data$ride_time ~ trip_data$member_casual, FUN = median)
aggregate(trip_data$ride_time ~ trip_data$member_casual, FUN = max)
aggregate(trip_data$ride_time ~ trip_data$member_casual, FUN = min)
aggregate(trip_data$ride_time ~ trip_data$weekday + trip_data$member_casual, FUN=mean)
```

```
trip_by_day <- trip_data %>%
  group_by(member_casual, weekday) %>%
  summarise(num_of_rides = n(), avg_duration = mean(ride_time)) %>%
  arrange(member_casual, weekday)
```

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

```
total_rides_casual_weekend <- NROW(filter(trip_data, member_casual == "casual" & (weekday == "Saturday" | weekday == "Sunday")))
total_rides_casual_weekday <- NROW(filter(trip_data, member_casual == "casual" & !(weekday == "Saturday" | weekday == "Sunday")))
```

```
week <- c("Weekday", "Weekend")
casual_week <- c(total_rides_casual_weekday, total_rides_casual_weekend)
piepercent <- round(100 * casual_week / sum(casual_week), 2)
weekride <- paste(week, piepercent)
weekride_casual <- paste(weekride, "%", sep="")
weekride_casual
```

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

```
trip_data_member <- filter(trip_data, member_casual == "member")
trip_data_casual <- filter(trip_data, member_casual == "casual")
```

```
trip_data_member %>%
  group_by(rideable_type, weekday) %>%
  summarise(num_of_rides = n(), avg_duration = mean(ride_time)) %>%
  arrange(rideable_type, weekday)%>%
  ggplot(aes(x = weekday, y=num_of_rides, fill=rideable_type)) +
  geom_col(position="dodge2") + 
  labs(title="Total number of Rides per type of bike (Member)
  By Weekday", x="Week Day", y="Number of Rides") + #TOTAL RIDES PER BIKE PER WEEK
  scale_y_continuous(labels = scales::comma) +
  scale_fill_manual(values = c("red", "green", "blue"))
```

```
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

```
trip_data_casual %>%
  group_by(rideable_type, weekday) %>%
  summarise(num_of_rides = n(), avg_duration = mean(ride_time)) %>%
  arrange(rideable_type, weekday)%>%
  ggplot(aes(x = weekday, y=num_of_rides, fill=rideable_type)) +
  geom_col(position="dodge2") + 
  labs(title="Total number of Rides per type of bike (Member)
  By Weekday", x="Week Day", y="Number of Rides") + #TOTAL RIDES PER BIKE PER WEEK
  scale_y_continuous(labels = scales::comma) +
  scale_fill_manual(values = c("red", "green", "blue"))
```

```
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

# Intro
We are a junior data analyst for Cyclistic, a company providing bike-sharing services in Chicago. Riders may opt to purchase single-ride or full-day passes for casual use or sign up for the annual Cyclistic membership. Lily Moreno, the director of marketing, believes that maximizing the number of annual memberships is the way to go in ensuring the company's growth. In order to effectively create marketing strategies supported by this claim, we need to examine more closely the difference of a casual and member rider and analyze how they utilize the service provided to them. Given the usage data of all the trips done within the last year, along with the help of our chosen spreadsheet/data visualization software, we have everything we need to probe and extract insight that are useful and valuable to the company.

The following study will be split into chapters following the steps in the data analysis process: ask, prepare, process, analyze, share and act. 

# Chapter 1: Ask
W



```
setwd("C:/Users/ADMIN/Documents/Google")
```

```
library(tidyverse)
library(ggplot2)
library(lubridate)
library(dplyr)
```


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

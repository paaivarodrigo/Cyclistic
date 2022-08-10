---
Title: "Cyclistic Bike Share"
Author: "Rodrigo"
Date: "2022-08-09"
Output: html_document
---

# Cyclistic Bike Share Analysis

This is a case study of the Google Data Analytics Professional Certificate.

According to Google, there are six phases or steps of data analysis: ask, prepare, process, analyze, share, and act. I will follow these steps to answer the business tasks.

## Case Study: How Does a Bike-Share Navigate Speedy Success?
### Introduction 
Welcome to the Cyclistic bike-share analysis case study! In this case study, you will perform many real-world tasks of a junior data analyst. You will work for a fictional company, Cyclistic, and meet different characters and team members. In order to answer the key business questions, you will follow the steps of the data analysis process: ask, prepare, process, analyze, share, and act. Along the way, the Case Study Roadmap tables — including guiding questions and key tasks — will help you stay on the right path. By the end of this lesson, you will have a portfolio-ready case study. Download the packet and reference the details of this case study anytime. Then, when you begin your job hunt, your case study will be a tangible way to demonstrate your knowledge and skills to potential employers.

### Scenario
You are a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, your team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, your team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve your recommendations, so they must be backed up with compelling data insights and professional data visualizations.

### Characters and teams
● Cyclistic: A bike-share program that features more than 5,800 bicycles and 600 docking stations. Cyclistic sets itself apart by also offering reclining bikes, hand tricycles, and cargo bikes, making bike-share more inclusive to people with disabilities and riders who can’t use a standard two-wheeled bike. The majority of riders opt for traditional bikes; about 8% of riders use the assistive options. Cyclistic users are more likely to ride for leisure, but about 30% use them to commute to work each day.

● Lily Moreno: The director of marketing and your manager. Moreno is responsible for the development of campaigns and initiatives to promote the bike-share program. These may include email, social media, and other channels.

● Cyclistic marketing analytics team: A team of data analysts who are responsible for collecting, analyzing, and reporting data that helps guide Cyclistic marketing strategy. You joined this team six months ago and have been busy learning about Cyclistic’s mission and business goals — as well as how you, as a junior data analyst, can help Cyclistic achieve them.

● Cyclistic executive team: The notoriously detail-oriented executive team will decide whether to approve the recommended marketing program.

### About the company
In 2016, Cyclistic launched a successful bike-share offering. Since then, the program has grown to a fleet of 5,824 bicycles that are geotracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and returned to any other station in the system anytime.

Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members.

Cyclistic’s finance analysts have concluded that annual members are much more profitable than casual riders. Although the pricing flexibility helps Cyclistic attract more customers, Moreno believes that maximizing the number of annual members will be key to future growth. Rather than creating a marketing campaign that targets all-new customers, Moreno believes there is a very good chance to convert casual riders into members. She notes that casual riders are already aware of the Cyclistic program and have chosen Cyclistic for their mobility needs.

Moreno has set a clear goal: Design marketing strategies aimed at converting casual riders into annual members. In order to do that, however, the marketing analyst team needs to better understand how annual members and casual riders differ, why casual riders would buy a membership, and how digital media could affect their marketing tactics. Moreno and her team are interested in analyzing the Cyclistic historical bike trip data to identify trends.

## Data Documentation

### PHASE 1: ASK

1)	The business tasks

The director of Marketing (Lily Moreno) wants to know how annual members and casual riders use bike differently, why casual riders would buy Cyclistic annual membership and how Cyclistic can use digital media to influence casual riders to become members. As a Jr. Data Analyst, I was assigned the first question: How do annual members and casual riders use the bike differently?

Note: So I need to look at the data and find out how different types of subscribers behave and think about what might motivate casual riders to become annual members.

2)	Consider Key Stakeholders

The stakeholders here are the director of marketing, Cyclistic marketing analytics team and Cyclistic executive team.

### Phase 2: PREPARE

1)	Data Source

The last 12 months of casual and annual member trip data. I created a folder on my computer and a subfolder where the original data was stored. The modified data would be in the main folder.

### Phase 3: Process

1)	Data Cleaning and Manipulation

For data cleaning and manipulation, I used Power Query in Excel. First, each of the 12 tables contained 13 columns with the same name and a different number of rows. I verified that, in the columns that contained the name of the beginning and the end of the station, there were null or blank values, as these columns are important to answer the question that was given to me, I removed the data that contained these values(blank or nulls). It is important to point out that, in this cleaning, it was not possible to use more than 1 million data.

In all tables, I removed 6 columns referring to latitude, longitude, the start and end station id, as I would not initially use them to answer commercial questions and would reduce the amount of data processed, saving time - latitude and longitude data were later used using RStudio to identify the most accessed stations - after that, I added 2 columns that calculate the duration of each trip and the day the user took the trip. Then I changed the data type to match what I expected.

In the “trip_length” column, I noticed that some values were negative or equal to zero, as I don't have more information and I don't know the team's behavior in these cases, I decided to remove these values, as they will interfere in the final numbers and, consequently, in the analysis. As the number of rows summed from all tables was 4,678,621, it would not be possible to have the data in Excel cells. So I used the Power Query data model, which allows me to use all the data and continue working in Excel. Then I used the pivot table to do the relationships, calculations and views.

However, to practice other technologies used in the course, I decided to use the R programming language and RStudio as software.

## Cyclistic Case Study with RStudio

### Setting up my environment

```{r }
install.packages("skimr")
install.packages("plotrix")
```

Loading the libraries
```{r }
library("tidyverse")
library("lubridate")
library("ggplot2")
library("geosphere")
library("gridExtra") 
library("ggmap")
library("dplyr")
library("plotrix")
library("readr")
library("skimr")
```

Importing the csv files.

Note: As I'm using RStudio desktop, I imported from my directory.
```{r }
trip_data_2021_07 <- read.csv("C:/Users/rodri/OneDrive/Área de Trabalho/Cyclistic Case Study/Original worksheets/202107-divvy-tripdata.csv")

trip_data_2021_08 <- read.csv("C:/Users/rodri/OneDrive/Área de Trabalho/Cyclistic Case Study/Original worksheets/202108-divvy-tripdata.csv")

trip_data_2021_09 <- read.csv("C:/Users/rodri/OneDrive/Área de Trabalho/Cyclistic Case Study/Original worksheets/202109-divvy-tripdata.csv")

trip_data_2021_10 <- read.csv("C:/Users/rodri/OneDrive/Área de Trabalho/Cyclistic Case Study/Original worksheets/202110-divvy-tripdata.csv")

trip_data_2021_11 <- read.csv("C:/Users/rodri/OneDrive/Área de Trabalho/Cyclistic Case Study/Original worksheets/202111-divvy-tripdata.csv")

trip_data_2021_12 <- read.csv("C:/Users/rodri/OneDrive/Área de Trabalho/Cyclistic Case Study/Original worksheets/202112-divvy-tripdata.csv")

trip_data_2022_01 <- read.csv("C:/Users/rodri/OneDrive/Área de Trabalho/Cyclistic Case Study/Original worksheets/202201-divvy-tripdata.csv")

trip_data_2022_02 <- read.csv("C:/Users/rodri/OneDrive/Área de Trabalho/Cyclistic Case Study/Original worksheets/202202-divvy-tripdata.csv")

trip_data_2022_03 <- read.csv("C:/Users/rodri/OneDrive/Área de Trabalho/Cyclistic Case Study/Original worksheets/202203-divvy-tripdata.csv")

trip_data_2022_04 <- read.csv("C:/Users/rodri/OneDrive/Área de Trabalho/Cyclistic Case Study/Original worksheets/202204-divvy-tripdata.csv")

trip_data_2022_05 <- read.csv("C:/Users/rodri/OneDrive/Área de Trabalho/Cyclistic Case Study/Original worksheets/202205-divvy-tripdata.csv")

trip_data_2022_06 <- read.csv("C:/Users/rodri/OneDrive/Área de Trabalho/Cyclistic Case Study/Original worksheets/202206-divvy-tripdata.csv")
```

Checking the structures of the data
```{r }
str(trip_data_2021_07)
str(trip_data_2021_08)
str(trip_data_2021_09)
str(trip_data_2021_10)
str(trip_data_2021_11)
str(trip_data_2021_12)
str(trip_data_2022_01)
str(trip_data_2022_02)
str(trip_data_2022_03)
str(trip_data_2022_04)
str(trip_data_2022_05)
str(trip_data_2022_06)
```

Analyzing each dataset, each column is with the same data type, therefore, the first step will be to merge all the tables into one, and then perform the data type change and cleaning in sequence. Instead of changing and cleaning one table at a time.


Another point to notice is that there are several null or blank fields in the "start_station_name" and "end_station_name" columns, as we will use these columns to know which stations and routes are most used by different types of subscribers, we will need to clean them so as not to cause bias.

I also want to change "started_at" and "ended_at" to date data type.

Loading the last 12 months of data in a single dataframe (df)
```{r }
df_all_tripdata <- bind_rows(trip_data_2021_07,trip_data_2021_08,trip_data_2021_09,trip_data_2021_10,trip_data_2021_11,trip_data_2021_12,trip_data_2022_01,trip_data_2022_02,trip_data_2022_03,trip_data_2022_04,trip_data_2022_05,trip_data_2022_06)
```

Checking the data
```{r }
skim_without_charts(df_all_tripdata)
```

First, "started at" and "ended at" for the date data type. I want to break them down by year, month, day and day of the week for further analysis as well.

```{r }
df_all_tripdata$date <- as.Date(df_all_tripdata$started_at)
df_all_tripdata$date <- as.Date(df_all_tripdata$ended_at)
df_all_tripdata$month <- format(as.Date(df_all_tripdata$date), "%B")
df_all_tripdata$day <- format(as.Date(df_all_tripdata$date), "%d")
df_all_tripdata$year <- format(as.Date(df_all_tripdata$date), "%Y")
df_all_tripdata$day_of_week <- format(as.Date(df_all_tripdata$date), "%A")
```

Checking the data
```{r }
glimpse(df_all_tripdata)
```

Removing nulls first
```{r }
df_all_tripdata <- drop_na(df_all_tripdata)
skim_without_charts(df_all_tripdata)
```

Note: I also want to remove empty rows over the start and end station name, because I want to know the top 10 stations used, both at start and finish! When we last checked our data, there were 836,018 missing start station names and 886,729 missing end station names.

Removing empty rows
```{r }
df_all_tripdata <- filter(df_all_tripdata,start_station_name!="" & end_station_name!="")
skim_without_charts(df_all_tripdata)
```

Now I want to add another column called "ride_length". As the name suggests I will calculate the trip duration.
```{r }
df_all_tripdata$ride_length <- difftime(df_all_tripdata$ended_at, df_all_tripdata$started_at)
glimpse(df_all_tripdata)
```

The result is in seconds, so I will convert it to minutes and round it to 2.
```{r }
df_all_tripdata$ride_length <- df_all_tripdata$ride_length/60
df_all_tripdata$ride_length <- round(df_all_tripdata$ride_length, 2)
```

Converting "ride_length" to numeric.
```{r }
df_all_tripdata$ride_length <- as.numeric(as.character(df_all_tripdata$ride_length))
glimpse(df_all_tripdata)
```

Next, I want to remove any value in the "ride_length" column that is <= 0
```{r }
df_all_tripdata <- filter(df_all_tripdata, ride_length > 0)
```
Note: As we do not know the route the user took, only the start and end point, the calculations about distance and travel speed may be incorrect and, therefore, be biased for our analysis.


Note: The output settings were showing the results in Portuguese, I searched on how to change this, but I didn't find the solution for now, so I will pass the data that is in Portuguese to English.
```{r }
df_all_tripdata$day_of_week[df_all_tripdata$day_of_week =="domingo"] <- "Sunday"
df_all_tripdata$day_of_week[df_all_tripdata$day_of_week =="segunda-feira"] <- "Monday"
df_all_tripdata$day_of_week[df_all_tripdata$day_of_week =="terça-feira"] <- "Tuesday"
df_all_tripdata$day_of_week[df_all_tripdata$day_of_week =="quarta-feira"] <- "Wednesday"
df_all_tripdata$day_of_week[df_all_tripdata$day_of_week =="quinta-feira"] <- "Thursday"
df_all_tripdata$day_of_week[df_all_tripdata$day_of_week =="sexta-feira"] <- "Friday"
df_all_tripdata$day_of_week[df_all_tripdata$day_of_week =="sábado"] <- "Saturday"
```

```{r }
df_all_tripdata$month[df_all_tripdata$month =="janeiro"] <- "January"
df_all_tripdata$month[df_all_tripdata$month =="fevereiro"] <- "February"
df_all_tripdata$month[df_all_tripdata$month =="março"] <- "March"
df_all_tripdata$month[df_all_tripdata$month =="abril"] <- "April"
df_all_tripdata$month[df_all_tripdata$month =="maio"] <- "May"
df_all_tripdata$month[df_all_tripdata$month =="junho"] <- "June"
df_all_tripdata$month[df_all_tripdata$month =="julho"] <- "July"
df_all_tripdata$month[df_all_tripdata$month =="agosto"] <- "August"
df_all_tripdata$month[df_all_tripdata$month =="setembro"] <- "September"
df_all_tripdata$month[df_all_tripdata$month =="outubro"] <- "October"
df_all_tripdata$month[df_all_tripdata$month =="novembro"] <- "November"
df_all_tripdata$month[df_all_tripdata$month =="dezembro"] <- "December"
```

Checking the data
```{r }
glimpse(df_all_tripdata)
```

Before starting the analysis, I will order the days of the week and the months.
```{r }
df_all_tripdata$day_of_week <- ordered(df_all_tripdata$day_of_week, levels=c("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"))

df_all_tripdata$month <- ordered(df_all_tripdata$month, levels=c("January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"))
```

Since we are going to deal with big numbers like the total number of rides, R Plots will give us graphs with scientific notation and I don't want that.
```{r }
options(scipen=999)
```

### Phase 4: Analyse

1)	Analysis Summary

To understand how casual subscribers behave differently from annual subscribers, data such as the average and maximum duration of the trip, the number of monthly, quarterly and annual trips, and the type of bicycle they use can be the way to respond to the difference of behavior.

Let's visualize how our customers behave analyzing the number of rides by day in the last 12 months.
```{r }
df_all_tripdata %>% 
  group_by(member_casual, day_of_week) %>% 
  summarise(number_of_rides = n()) %>%
  arrange(member_casual, day_of_week)%>% 
  ggplot(aes(x = day_of_week, y = number_of_rides, fill = member_casual)) +
  geom_col(width=0.5, position = "dodge") + labs(title="Total Number of Rides by Day", x = "Day of the Week", y = "Number of Rides") + theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

Now let's check the average trip duration by day.
```{r }
df_all_tripdata %>% 
  group_by(member_casual, day_of_week) %>% 
  summarise(average_duration = mean(ride_length)) %>% 
  arrange(member_casual, day_of_week)	%>% 
  ggplot(aes(x = day_of_week, y = average_duration, fill = member_casual)) +
  geom_col(width=0.5, position = "dodge") + labs(title="Average Ride Length by Day", x = "Day of the Week", y = "Average Ride Length") + theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

I also want to check by month.
```{r }
df_all_tripdata %>% 
  group_by(member_casual, month) %>% 
  summarise(number_of_rides = n()) %>% 
  arrange(member_casual, month)	%>% 
  ggplot(aes(x = month, y = number_of_rides, fill = member_casual)) +
  geom_col(width=0.5, position = "dodge") + labs(title="Total Number of Rides by Month", x = "Month", y = "Number of Rides") + theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

```{r }
df_all_tripdata %>% 
  group_by(member_casual, month) %>% 
  summarise(average_duration = mean(ride_length)) %>% 
  arrange(member_casual, month)	%>% 
  ggplot(aes(x = month, y = average_duration, fill = member_casual)) +
  geom_col(width=0.5, position = "dodge") + labs(title="Average Ride Length by Month", x = "Month", y = "Average Ride Length") + theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

Now I want to analyze the day of Sunday and Saturday(weekend) compared to the rest(weekdays), by type of subscriber. I will begin with casual riders.
```{r }
casual_weekends_ride <- NROW(filter(df_all_tripdata, member_casual == "casual" & (day_of_week == "Saturday" | day_of_week == "Sunday")))

casual_weekdays_ride <- NROW(filter(df_all_tripdata, member_casual == "casual" & !(day_of_week == "Saturday" | day_of_week == "Sunday")))

labels <- c("Weekdays:", "Weekend:")
casual_week <- c(casual_weekdays_ride, casual_weekends_ride)
casual_ratio <- round(100 *casual_week / sum(casual_week), 1)
casual_pie_labels <- paste(labels, casual_ratio)
labels_casual_week <- paste(casual_pie_labels, "%", sep="")

pos
pos[1]<-1.6
pos[2]<-4.8

pie3D(casual_week, labels = labels_casual_week, explode = 0.1, col= rainbow(3), main = "Ratio of Casual Subscribers on Weekends Compared to Weekdays",labelpos=pos)
```

Now with members.
Note: pos is for label placement, without it the text will be on top with the pie chart.
```{r }
member_weekends_ride <- NROW(filter(df_all_tripdata, member_casual == "member" & (day_of_week == "Saturday" | day_of_week == "Sunday")))

member_weekdays_ride <- NROW(filter(df_all_tripdata, member_casual == "member" & !(day_of_week == "Saturday" | day_of_week == "Sunday")))

labels <- c("Weekdays:", "Weekend:")
member_week <- c(member_weekdays_ride, member_weekends_ride)
member_ratio <- round(100 *member_week / sum(member_week), 1)
member_pie_labels <- paste(labels, member_ratio)
labels_member_week <- paste(member_pie_labels, "%", sep="")

pos
pos[1]<-1.6
pos[2]<-4.8

pie3D(casual_week, labels = labels_member_week, explode = 0.1, col= rainbow(3), main = "Ratio of Member Subscribers on Weekends Compared to Weekdays",labelpos=pos)
```

Now I want to analyze the period from May to September for each type of rider. Starting with casual.
```{r }
casual_may_september_ride <- NROW(filter(df_all_tripdata, member_casual == "casual" & (month == "May" | month == "June" | month == "July" | month == "August" | month == "September")))

casual_rest_of_year_ride <- NROW(filter(df_all_tripdata, member_casual == "casual" & !(month == "May" | month == "June" | month == "July" | month == "August" | month == "September")))

labels <- c("Rest of the Year:","May-Sept:")
casual_week <- c(casual_rest_of_year_ride, casual_may_september_ride)
casual_ratio <- round(100 *casual_week / sum(casual_week), 1)
casual_pie_labels <- paste(labels, casual_ratio)
labels_casual_week <- paste(casual_pie_labels, "%", sep="")

pos
pos[1]<-1.6
pos[2]<-4.8
pie3D(casual_week, labels = labels_casual_week, explode = 0.1, col= rainbow(3), main = "Ratio of Casual Subscribers from May to September Compared to the Rest of the Year",labelpos=pos)
```

Now, for members.
```{r }
member_may_september_ride <- NROW(filter(df_all_tripdata, member_casual == "member" & (month == "May" | month == "June" | month == "July" | month == "August" | month == "September")))

member_rest_of_year_ride <- NROW(filter(df_all_tripdata, member_casual == "member" & !(month == "May" | month == "June" | month == "July" | month == "August" | month == "September")))

labels <- c("Rest of the Year:","May-Sept:")
member_week <- c(member_rest_of_year_ride, member_may_september_ride)
member_ratio <- round(100 *member_week / sum(member_week), 1)
member_pie_labels <- paste(labels, member_ratio)
labels_member_week <- paste(member_pie_labels, "%", sep="")

pos
pos[1]<-1.6
pos[2]<-4.8
pie3D(casual_week, labels = labels_member_week, explode = 0.1, col= rainbow(3), main = "Ratio of Member Subscribers from May to September Compared to the Rest of the Year",labelpos=pos)
```

It will also be interesting to analyze the type of bicycle that each customer uses. First, I will create a new dataframe for members only and casual only. For members, we will have:
```{r }
df_all_tripdata_member <-  filter(df_all_tripdata, member_casual == "member")
df_all_tripdata_member %>% 
  group_by(rideable_type, month) %>% 
  summarise(number_of_rides = n()) %>% 		
  arrange(rideable_type, month)	%>% 
  ggplot(aes(x = month, y = number_of_rides, fill = rideable_type)) +
  geom_col(width=0.5, position = "dodge") + labs(title="Total Number of Bike Types Used by Members per Month", x = "Month", y = "Number of Rides") + theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

For casual:
```{r }
df_all_tripdata_casual <-  filter(df_all_tripdata, member_casual == "casual")
df_all_tripdata_casual %>% 
  group_by(rideable_type, month) %>% 
  summarise(number_of_rides = n()) %>% 		
  arrange(rideable_type, month)	%>% 
  ggplot(aes(x = month, y = number_of_rides, fill = rideable_type)) +
  geom_col(width=0.5, position = "dodge") + labs(title="Total Number of Bike Types Used by Casuals per Month", x = "Month", y = "Number of Rides") + theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

I want to see the top 10 start stations and end stations for members and casuals as well.
Members:
```{r }
top10_member_start_station <- df_all_tripdata_member %>% 
  group_by(start_station_name) %>%
  summarise(number_of_rides  = n()) %>% 
  arrange(desc(number_of_rides))
top10_member_start_station <- head(arrange(top10_member_start_station,desc(number_of_rides)), 10)
head(top10_member_start_station, 10)

top10_member_start_station %>% 
  group_by(start_station_name, number_of_rides) %>% 
  arrange(start_station_name,number_of_rides)	%>% 
  ggplot(aes(x = reorder(start_station_name,-number_of_rides), y = number_of_rides, fill = number_of_rides)) +
  geom_col(width=0.5, position = "dodge") + labs(title="Top 10 Start Stations for Members", x = "Station Name", y = "Number of Rides") + theme(axis.text.x = element_text(angle = 70, hjust = 1))


top10_member_end_station <- df_all_tripdata_member %>% 
  group_by(end_station_name) %>%
  summarise(number_of_rides  = n()) %>% 
  arrange(desc(number_of_rides))
top10_member_end_station <- head(arrange(top10_member_end_station,desc(number_of_rides)), 10)
head(top10_member_end_station, 10)

top10_member_end_station %>% 
  group_by(end_station_name, number_of_rides) %>% 
  arrange(end_station_name,number_of_rides)	%>% 
  ggplot(aes(x = reorder(end_station_name,-number_of_rides), y = number_of_rides, fill = number_of_rides)) +
  geom_col(width=0.5, position = "dodge") + labs(title="Top 10 End Stations for Members", x = "Station Name", y = "Number of Rides") + theme(axis.text.x = element_text(angle = 70, hjust = 1))
```

Casuals:
```{r }
top10_casual_start_station <- df_all_tripdata_casual %>% 
  group_by(start_station_name) %>%
  summarise(number_of_rides  = n()) %>% 
  arrange(desc(number_of_rides))
top10_casual_start_station <- head(arrange(top10_casual_start_station,desc(number_of_rides)), 10)
head(top10_casual_start_station, 10)

top10_casual_start_station %>% 
  group_by(start_station_name, number_of_rides) %>% 
  arrange(start_station_name,number_of_rides)	%>% 
  ggplot(aes(x = reorder(start_station_name,-number_of_rides), y = number_of_rides, fill = number_of_rides)) +
  geom_col(width=0.5, position = "dodge") + labs(title="Top 10 Start Stations for Casuals", x = "Station Name", y = "Number of Rides") + theme(axis.text.x = element_text(angle = 70, hjust = 1))


top10_casual_end_station <- df_all_tripdata_casual %>% 
  group_by(end_station_name) %>%
  summarise(number_of_rides  = n()) %>% 
  arrange(desc(number_of_rides))
top10_casual_end_station <- head(arrange(top10_casual_end_station,desc(number_of_rides)), 10)
head(top10_casual_end_station, 10)

top10_casual_end_station %>% 
  group_by(end_station_name, number_of_rides) %>% 
  arrange(end_station_name,number_of_rides)	%>% 
  ggplot(aes(x = reorder(end_station_name,-number_of_rides), y = number_of_rides, fill = number_of_rides)) +
  geom_col(width=0.5, position = "dodge") + labs(title="Top 10 End Stations for Casuals", x = "Station Name", y = "Number of Rides") + theme(axis.text.x = element_text(angle = 70, hjust = 1))
```
Now, let's check the routes. I want to know the top 10 routes each customer uses. Members first.
```{r }
df_all_tripdata_member <- df_all_tripdata_member %>%
  mutate(route = paste(start_station_name, "To", sep=" "))
         
df_all_tripdata_member <- df_all_tripdata_member %>%       
  mutate(route = paste(route, end_station_name, sep =" "))

top10_member_route <- df_all_tripdata_member %>% 
  group_by(route) %>%
  summarise(number_of_rides  = n()) %>% 
  arrange(route, number_of_rides)

top10_member_route <- head(arrange(top10_member_route, desc(number_of_rides)),10)
head(top10_member_route, 10)

top10_member_route %>% 
  group_by(route, number_of_rides) %>% 
  arrange(route,number_of_rides)	%>% 
  ggplot(aes(x = reorder(route,-number_of_rides), y = number_of_rides, fill = number_of_rides)) +
  geom_col(width=0.5, position = "dodge") + labs(title="Top 10 Routes for Members", x = "Route", y = "Number of Rides") + theme(axis.text.x = element_text(angle = 70, hjust = 1))
```

Top 10 routes for casuals:
```{r }
df_all_tripdata_casual <- df_all_tripdata_casual %>%
  mutate(route = paste(start_station_name, "To", sep=" "))
         
df_all_tripdata_casual <- df_all_tripdata_casual %>%       
  mutate(route = paste(route, end_station_name, sep =" "))

top10_casual_route <- df_all_tripdata_casual %>% 
  group_by(route) %>%
  summarise(number_of_rides  = n()) %>% 
  arrange(route, number_of_rides)

top10_casual_route <- head(arrange(top10_casual_route, desc(number_of_rides)),10)
head(top10_casual_route, 10)

top10_casual_route %>% 
  group_by(route, number_of_rides) %>% 
  arrange(route,number_of_rides)	%>% 
  ggplot(aes(x = reorder(route,-number_of_rides), y = number_of_rides, fill = number_of_rides)) +
  geom_col(width=0.5, position = "dodge") + labs(title="Top 10 Routes for Casuals", x = "Route", y = "Number of Rides") + theme(axis.text.x = element_text(size=8,angle = 70, hjust = 1))
```

Now I want to plot routes greater than or equal to 500 trips for members and casuals at Chicago stations.
```{r }
df_top10_routes <- df_all_tripdata%>% 
filter(start_lng != end_lng & start_lat != end_lat) %>%
group_by(start_lng, start_lat, end_lng, end_lat, member_casual, rideable_type) %>%
summarise(total  = n(),.groups="drop")%>%
filter(total >= 500)
```

As usual, members first
```{r }
member <- df_top10_routes %>% filter(member_casual == "member")

chi_bb <- c(
  left = -87.72000,
  bottom = 41.75400,
  right = -87.57000,
  top = 42.10230
)

chicago_stamen <- get_stamenmap(
  bbox = chi_bb,
  zoom = 12,
  maptype = "toner"
)

ggmap(chicago_stamen,darken = c(0.8, "white")) +
    geom_curve(member, mapping = aes(x = start_lng, y = start_lat, xend = end_lng, yend = end_lat, alpha= 200, color = rideable_type), size = 0.5, curvature = .2,arrow = arrow(length=unit(.3,"cm"), ends="first", type = "closed")) +  
    coord_cartesian() +
    labs(title = "Most Popular Routes by Annual Members",x=NULL,y=NULL) +
    theme(legend.position="right")
```

For casuals
```{r }
casual <- df_top10_routes %>% filter(member_casual == "casual")

chi_bb <- c(
  left = -87.72000,
  bottom = 41.75400,
  right = -87.57000,
  top = 42.10230
)

chicago_stamen <- get_stamenmap(
  bbox = chi_bb,
  zoom = 12,
  maptype = "toner"
)

ggmap(chicago_stamen,darken = c(0.8, "white")) +
    geom_curve(casual, mapping = aes(x = start_lng, y = start_lat, xend = end_lng, yend = end_lat, alpha= 200, color = rideable_type), size = 0.5, curvature = .2,arrow = arrow(length=unit(.3,"cm"), ends="first", type = "closed")) +  
    coord_cartesian() +
    labs(title = "Most Popular Routes by Casual Riders",x=NULL,y=NULL) +
    theme(legend.position="right")
```
### PHASE 5: SHARE
1)	Visualizations and Key Findings

Through the analysis of the data, it is possible to perceive some differences in use, they are:

• Casual subscribers use bicycles more during the weekends while annual members use more during the weekdays;

![Number of Rides by Day](https://user-images.githubusercontent.com/106841477/184502973-218fc1ce-265d-4047-afdb-77a57f690c2a.png)

• For casuals, 40.7% of the tours are done on weekends while for members, 75% of the tours are on weekdays;

![Member Ratio](https://user-images.githubusercontent.com/106841477/184503221-e77294c3-297f-4470-8400-dbed2a9e9b65.png) ![Casual Ratio](https://user-images.githubusercontent.com/106841477/184503222-4cc8e306-b398-422f-a82b-2f0e1ede41fd.png)

• Casual customers spend nearly twice as much time on their bikes as annual members any day of the week;

![Avg Ride by day](https://user-images.githubusercontent.com/106841477/184503248-9e0b81df-3dd1-4a44-9e3b-b19fa38a71b0.png)

• The same idea mentioned above happens when analyzed by month, while annual members spend 10-13 minutes on the bikes, casual members spend 23-33 minutes on average;

![Avg Ride Length by Month](https://user-images.githubusercontent.com/106841477/184503265-4b00aa1a-0ea6-4cf8-bd49-f8616d2d9751.png)

• The number of outings among casual subscribers increases considerably between May and September, indicating a possible relationship with the school break that takes place in the period highlighted above;

![Number of Rides by Month](https://user-images.githubusercontent.com/106841477/184503309-55ec8cbc-cd46-44b7-8d2d-8fc1492872d4.png)

• May to September corresponds to 75.5% of all tours by casual subscribers;

![Casual School Break Ratio](https://user-images.githubusercontent.com/106841477/184503329-fb7806b5-7422-42fb-81b7-be0a7b9d5aab.png)

• May to September accounts for 59.7% of all annual member tours;

![Member School Break Ratio](https://user-images.githubusercontent.com/106841477/184503338-096382bf-5925-46b4-bd0b-968b5f2872ca.png)

• Both members and casuals prefer classic bikes. Members use only 2 types, the classic and the electric, the casual ones use 3, the two mentioned above plus the docked bike;

![Total Bike Types Casuals](https://user-images.githubusercontent.com/106841477/184503365-008da429-0a45-4edc-afe0-43415eecc8d1.png) ![Total Bike Types Member](https://user-images.githubusercontent.com/106841477/184503368-be47fd35-034b-43da-818a-4c64ab3ca0ab.png)

• Members' main departure and arrival station is Kingsbury St & Kinzie St, while for casuals, the main station is Streeter Dr & Grand Ave;

![Top10 Members Start Stations](https://user-images.githubusercontent.com/106841477/184503401-0018b081-ac17-4c70-b08d-50562b6d620e.png) ![Top10 Members End Stations](https://user-images.githubusercontent.com/106841477/184503400-95afe842-34fc-46b7-ad4b-c48e01731686.png)
![Top10 Casuals Start Stations](https://user-images.githubusercontent.com/106841477/184503403-3f75608f-1bd0-4c99-a288-49a68121e244.png) ![Top10 Casuals End Stations](https://user-images.githubusercontent.com/106841477/184503402-c51e6bb4-2234-4c7c-a85b-2d31abb2a9c4.png)

• For annual members, there are 4 routes that stand out in the top 10, while for casual members, one route stands out from the others;

![Top10 Routes for Members](https://user-images.githubusercontent.com/106841477/184503446-03b91b10-3576-49eb-ac67-0d88d999be33.png) ![Top 10 Routes for Casuals](https://user-images.githubusercontent.com/106841477/184503445-403c11ca-a112-43e2-8692-8c3d0220789e.png)

• Finally, the Chicago map shows the top 10 routes used by members and casuals and the type of bike.

![Members Map top routes](https://user-images.githubusercontent.com/106841477/184503479-049ea6ec-7ee8-42c5-96cc-efb5f1af4d8d.png) ![Casuals Map Top Routes](https://user-images.githubusercontent.com/106841477/184503485-e6659350-f390-4186-b49a-f337219d694c.png)

### PHASE 6: ACT
My top 3 recommendations are:

• Marketing campaigns to convert casual customers into annual ones should be focused on the period before and referring to school breaks;

• Marketing campaigns in physical locations should focus on the main routes for casuals;

• As there is evidence that casual customers are the people who use the service most on average, on weekends, leisure time and vacations, the marketing team should promote this fun side of the service, offering promotions and other related advantages longer journeys and summer use.

### Thanks to
I want to thank Julen Aranguren for sharing his case study which helped me a lot when plotting the maps.
https://www.kaggle.com/code/julenaranguren/cyclistic-bike-share-a-case-study
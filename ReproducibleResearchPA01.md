---
title: "Reproducible Research. Peer Assessment 1"
author: "Roman Savchenko"
date: "2022-12-24"
output:
   html_document:
      keep_md: true
---


## Change the locale for the R process.

```r
Sys.getlocale()
```

```
## [1] "LC_COLLATE=Russian_Russia.utf8;LC_CTYPE=Russian_Russia.utf8;LC_MONETARY=Russian_Russia.utf8;LC_NUMERIC=C;LC_TIME=Russian_Russia.utf8"
```

```r
Sys.setlocale("LC_ALL","English")
```

```
## Warning in Sys.setlocale("LC_ALL", "English"): using locale code page other than
## 65001 ("UTF-8") may cause problems
```

```
## [1] "LC_COLLATE=English_United States.1252;LC_CTYPE=English_United States.1252;LC_MONETARY=English_United States.1252;LC_NUMERIC=C;LC_TIME=English_United States.1252"
```

```r
Sys.getlocale()
```

```
## [1] "LC_COLLATE=English_United States.1252;LC_CTYPE=English_United States.1252;LC_MONETARY=English_United States.1252;LC_NUMERIC=C;LC_TIME=English_United States.1252"
```


## Loading and preprocessing the data

The reading of the data from the activity.csv file to the R data frame.

```r
activity <- read.csv(file = 'activity.csv')
head(activity)
```

```
##   steps       date interval
## 1    NA 2012-10-01        0
## 2    NA 2012-10-01        5
## 3    NA 2012-10-01       10
## 4    NA 2012-10-01       15
## 5    NA 2012-10-01       20
## 6    NA 2012-10-01       25
```

```r
str(activity)
```

```
## 'data.frame':	17568 obs. of  3 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : chr  "2012-10-01" "2012-10-01" "2012-10-01" "2012-10-01" ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
```

The creation of an additional variable to count cases when activity$steps=NA.

```r
activity$stepsNA <- ifelse(is.na(activity$steps), 1, 0)
head(activity)
```

```
##   steps       date interval stepsNA
## 1    NA 2012-10-01        0       1
## 2    NA 2012-10-01        5       1
## 3    NA 2012-10-01       10       1
## 4    NA 2012-10-01       15       1
## 5    NA 2012-10-01       20       1
## 6    NA 2012-10-01       25       1
```

```r
str(activity)
```

```
## 'data.frame':	17568 obs. of  4 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : chr  "2012-10-01" "2012-10-01" "2012-10-01" "2012-10-01" ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
##  $ stepsNA : num  1 1 1 1 1 1 1 1 1 1 ...
```

```r
unique(activity$stepsNA)
```

```
## [1] 1 0
```

The creation of an additional variable for 2 digit value of the month.

```r
activity$month <- substr(activity$date, 6, 7)
head(activity)
```

```
##   steps       date interval stepsNA month
## 1    NA 2012-10-01        0       1    10
## 2    NA 2012-10-01        5       1    10
## 3    NA 2012-10-01       10       1    10
## 4    NA 2012-10-01       15       1    10
## 5    NA 2012-10-01       20       1    10
## 6    NA 2012-10-01       25       1    10
```

```r
str(activity)
```

```
## 'data.frame':	17568 obs. of  5 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : chr  "2012-10-01" "2012-10-01" "2012-10-01" "2012-10-01" ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
##  $ stepsNA : num  1 1 1 1 1 1 1 1 1 1 ...
##  $ month   : chr  "10" "10" "10" "10" ...
```

```r
unique(activity$month)
```

```
## [1] "10" "11"
```

The creation of an additional variable for the numeric date variable.

```r
activity$dateN <- as.Date(activity$date, format = "%Y-%m-%d")
head(activity)
```

```
##   steps       date interval stepsNA month      dateN
## 1    NA 2012-10-01        0       1    10 2012-10-01
## 2    NA 2012-10-01        5       1    10 2012-10-01
## 3    NA 2012-10-01       10       1    10 2012-10-01
## 4    NA 2012-10-01       15       1    10 2012-10-01
## 5    NA 2012-10-01       20       1    10 2012-10-01
## 6    NA 2012-10-01       25       1    10 2012-10-01
```

```r
str(activity)
```

```
## 'data.frame':	17568 obs. of  6 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : chr  "2012-10-01" "2012-10-01" "2012-10-01" "2012-10-01" ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
##  $ stepsNA : num  1 1 1 1 1 1 1 1 1 1 ...
##  $ month   : chr  "10" "10" "10" "10" ...
##  $ dateN   : Date, format: "2012-10-01" "2012-10-01" ...
```

The creation of an additional variable for the day of the week.

```r
activity$weekDay <- weekdays(activity$dateN, abbreviate = TRUE)
head(activity)
```

```
##   steps       date interval stepsNA month      dateN weekDay
## 1    NA 2012-10-01        0       1    10 2012-10-01     Mon
## 2    NA 2012-10-01        5       1    10 2012-10-01     Mon
## 3    NA 2012-10-01       10       1    10 2012-10-01     Mon
## 4    NA 2012-10-01       15       1    10 2012-10-01     Mon
## 5    NA 2012-10-01       20       1    10 2012-10-01     Mon
## 6    NA 2012-10-01       25       1    10 2012-10-01     Mon
```

```r
str(activity)
```

```
## 'data.frame':	17568 obs. of  7 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : chr  "2012-10-01" "2012-10-01" "2012-10-01" "2012-10-01" ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
##  $ stepsNA : num  1 1 1 1 1 1 1 1 1 1 ...
##  $ month   : chr  "10" "10" "10" "10" ...
##  $ dateN   : Date, format: "2012-10-01" "2012-10-01" ...
##  $ weekDay : chr  "Mon" "Mon" "Mon" "Mon" ...
```

```r
unique(activity$weekDay)
```

```
## [1] "Mon" "Tue" "Wed" "Thu" "Fri" "Sat" "Sun"
```

The creation of an additional factor variable for the day of the week.

```r
activity$weekDayFact <- as.factor(ifelse(activity$weekDay %in% c("Sat", "Sun"),
                                         "weekend", "weekday"))
head(activity)
```

```
##   steps       date interval stepsNA month      dateN weekDay weekDayFact
## 1    NA 2012-10-01        0       1    10 2012-10-01     Mon     weekday
## 2    NA 2012-10-01        5       1    10 2012-10-01     Mon     weekday
## 3    NA 2012-10-01       10       1    10 2012-10-01     Mon     weekday
## 4    NA 2012-10-01       15       1    10 2012-10-01     Mon     weekday
## 5    NA 2012-10-01       20       1    10 2012-10-01     Mon     weekday
## 6    NA 2012-10-01       25       1    10 2012-10-01     Mon     weekday
```

```r
str(activity)
```

```
## 'data.frame':	17568 obs. of  8 variables:
##  $ steps      : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date       : chr  "2012-10-01" "2012-10-01" "2012-10-01" "2012-10-01" ...
##  $ interval   : int  0 5 10 15 20 25 30 35 40 45 ...
##  $ stepsNA    : num  1 1 1 1 1 1 1 1 1 1 ...
##  $ month      : chr  "10" "10" "10" "10" ...
##  $ dateN      : Date, format: "2012-10-01" "2012-10-01" ...
##  $ weekDay    : chr  "Mon" "Mon" "Mon" "Mon" ...
##  $ weekDayFact: Factor w/ 2 levels "weekday","weekend": 1 1 1 1 1 1 1 1 1 1 ...
```

```r
unique(activity$weekDayFact)
```

```
## [1] weekday weekend
## Levels: weekday weekend
```

The including of the R Stats package for the calculations.

```r
library(stats)
```

The including of the R Graphics package to produce histogram.

```r
library(graphics)
```

The including of the R package to produce time series plot.

```r
library(ggplot2)
```


## What is mean total number of steps taken per day?

The calculation of the total number of steps taken per day.

```r
stepsPerDay <- aggregate(x = activity$steps, by = list(activity$date), 
                         FUN = sum, na.rm = TRUE)
    colnames(stepsPerDay)[colnames(stepsPerDay) == 'Group.1'] <- 'date'
    colnames(stepsPerDay)[colnames(stepsPerDay) == 'x'] <- 'StepsTot'
print(stepsPerDay)
```

```
##          date StepsTot
## 1  2012-10-01        0
## 2  2012-10-02      126
## 3  2012-10-03    11352
## 4  2012-10-04    12116
## 5  2012-10-05    13294
## 6  2012-10-06    15420
## 7  2012-10-07    11015
## 8  2012-10-08        0
## 9  2012-10-09    12811
## 10 2012-10-10     9900
## 11 2012-10-11    10304
## 12 2012-10-12    17382
## 13 2012-10-13    12426
## 14 2012-10-14    15098
## 15 2012-10-15    10139
## 16 2012-10-16    15084
## 17 2012-10-17    13452
## 18 2012-10-18    10056
## 19 2012-10-19    11829
## 20 2012-10-20    10395
## 21 2012-10-21     8821
## 22 2012-10-22    13460
## 23 2012-10-23     8918
## 24 2012-10-24     8355
## 25 2012-10-25     2492
## 26 2012-10-26     6778
## 27 2012-10-27    10119
## 28 2012-10-28    11458
## 29 2012-10-29     5018
## 30 2012-10-30     9819
## 31 2012-10-31    15414
## 32 2012-11-01        0
## 33 2012-11-02    10600
## 34 2012-11-03    10571
## 35 2012-11-04        0
## 36 2012-11-05    10439
## 37 2012-11-06     8334
## 38 2012-11-07    12883
## 39 2012-11-08     3219
## 40 2012-11-09        0
## 41 2012-11-10        0
## 42 2012-11-11    12608
## 43 2012-11-12    10765
## 44 2012-11-13     7336
## 45 2012-11-14        0
## 46 2012-11-15       41
## 47 2012-11-16     5441
## 48 2012-11-17    14339
## 49 2012-11-18    15110
## 50 2012-11-19     8841
## 51 2012-11-20     4472
## 52 2012-11-21    12787
## 53 2012-11-22    20427
## 54 2012-11-23    21194
## 55 2012-11-24    14478
## 56 2012-11-25    11834
## 57 2012-11-26    11162
## 58 2012-11-27    13646
## 59 2012-11-28    10183
## 60 2012-11-29     7047
## 61 2012-11-30        0
```

The creation of a histogram of the total number of steps taken each day.

```r
hist(stepsPerDay$StepsTot,
     main = "Histogram of The Total Number of Steps Taken Each Day",
     breaks = 25,
     xlim = c(0, 25000),
     ylim = c(0, 12),
     xlab = "Total Number of Steps",
     labels = TRUE)
```

![](ReproducibleResearchPA01_files/figure-html/hist_totsteps-1.png)<!-- -->

The calculation of the mean of the total number of steps taken per day.

```r
MeanSteps <- mean(stepsPerDay$StepsTot, na.rm = TRUE)
MeanSteps
```

```
## [1] 9354.23
```

```r
MeanStepsRnd <- round(MeanSteps, digits = 2)
MeanStepsRnd
```

```
## [1] 9354.23
```

The calculation of the median of the total number of steps taken per day.

```r
MedianSteps <- median(stepsPerDay$StepsTot, na.rm = TRUE)
MedianSteps
```

```
## [1] 10395
```

```r
MedianStepsRnd <- round(MedianSteps, digits = 2)
MedianStepsRnd
```

```
## [1] 10395
```


## What is the average daily activity pattern?
The making of a time series plot of the 5-minute interval and the average number of steps taken.

```r
stepsPerInterval <- aggregate(x = activity$steps, by = list(activity$interval), 
                              FUN = mean, na.rm = TRUE)
    colnames(stepsPerInterval)[colnames(stepsPerInterval) == 'Group.1'] <- 'interval'
    colnames(stepsPerInterval)[colnames(stepsPerInterval) == 'x'] <- 'StepsAvg'
plot <- ggplot(stepsPerInterval, aes(x = interval, y = StepsAvg)) +
        geom_line() +
        labs(x = "5-minute Interval", y = "Avereage of the steps", 
             title="Avereage of number of steps taken per 5-minue intervals")
plot
```

![](ReproducibleResearchPA01_files/figure-html/timeseries_plot-1.png)<!-- -->

The selection of the 5-minute interval with the maximum number of steps

```r
MaxStepAvg <- max(stepsPerInterval$StepsAvg)
MaxInt <- subset(stepsPerInterval, StepsAvg == MaxStepAvg,
                 select = c(interval, StepsAvg))
MaxInt
```

```
##     interval StepsAvg
## 104      835 206.1698
```


## Imputing missing values

The calculation of the total number of missing values in the data frame.

```r
sum(activity$stepsNA)
```

```
## [1] 2304
```

The missing values will be imputed with the average value for the same time interval

```r
PreImp <- merge(activity, stepsPerInterval, by = "interval")
StepsImp <- PreImp
   StepsImp$steps <- ifelse(is.na(StepsImp$steps), StepsImp$StepsAvg, StepsImp$steps)
head(StepsImp)
```

```
##   interval    steps       date stepsNA month      dateN weekDay weekDayFact
## 1        0 1.716981 2012-10-01       1    10 2012-10-01     Mon     weekday
## 2        0 0.000000 2012-11-23       0    11 2012-11-23     Fri     weekday
## 3        0 0.000000 2012-10-28       0    10 2012-10-28     Sun     weekend
## 4        0 0.000000 2012-11-06       0    11 2012-11-06     Tue     weekday
## 5        0 0.000000 2012-11-24       0    11 2012-11-24     Sat     weekend
## 6        0 0.000000 2012-11-15       0    11 2012-11-15     Thu     weekday
##   StepsAvg
## 1 1.716981
## 2 1.716981
## 3 1.716981
## 4 1.716981
## 5 1.716981
## 6 1.716981
```

```r
str(StepsImp)
```

```
## 'data.frame':	17568 obs. of  9 variables:
##  $ interval   : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ steps      : num  1.72 0 0 0 0 ...
##  $ date       : chr  "2012-10-01" "2012-11-23" "2012-10-28" "2012-11-06" ...
##  $ stepsNA    : num  1 0 0 0 0 0 0 0 0 0 ...
##  $ month      : chr  "10" "11" "10" "11" ...
##  $ dateN      : Date, format: "2012-10-01" "2012-11-23" ...
##  $ weekDay    : chr  "Mon" "Fri" "Sun" "Tue" ...
##  $ weekDayFact: Factor w/ 2 levels "weekday","weekend": 1 1 2 1 2 1 2 1 1 2 ...
##  $ StepsAvg   : num  1.72 1.72 1.72 1.72 1.72 ...
```

The creation of a histogram of the total number of imputed steps.

```r
StepsImpDay <- aggregate(x = StepsImp$steps, by = list(StepsImp$date), 
                         FUN = sum, na.rm = TRUE)
    colnames(StepsImpDay)[colnames(StepsImpDay) == 'Group.1'] <- 'date'
    colnames(StepsImpDay)[colnames(StepsImpDay) == 'x'] <- 'StepsImpTot'
print(StepsImpDay)
```

```
##          date StepsImpTot
## 1  2012-10-01    10766.19
## 2  2012-10-02      126.00
## 3  2012-10-03    11352.00
## 4  2012-10-04    12116.00
## 5  2012-10-05    13294.00
## 6  2012-10-06    15420.00
## 7  2012-10-07    11015.00
## 8  2012-10-08    10766.19
## 9  2012-10-09    12811.00
## 10 2012-10-10     9900.00
## 11 2012-10-11    10304.00
## 12 2012-10-12    17382.00
## 13 2012-10-13    12426.00
## 14 2012-10-14    15098.00
## 15 2012-10-15    10139.00
## 16 2012-10-16    15084.00
## 17 2012-10-17    13452.00
## 18 2012-10-18    10056.00
## 19 2012-10-19    11829.00
## 20 2012-10-20    10395.00
## 21 2012-10-21     8821.00
## 22 2012-10-22    13460.00
## 23 2012-10-23     8918.00
## 24 2012-10-24     8355.00
## 25 2012-10-25     2492.00
## 26 2012-10-26     6778.00
## 27 2012-10-27    10119.00
## 28 2012-10-28    11458.00
## 29 2012-10-29     5018.00
## 30 2012-10-30     9819.00
## 31 2012-10-31    15414.00
## 32 2012-11-01    10766.19
## 33 2012-11-02    10600.00
## 34 2012-11-03    10571.00
## 35 2012-11-04    10766.19
## 36 2012-11-05    10439.00
## 37 2012-11-06     8334.00
## 38 2012-11-07    12883.00
## 39 2012-11-08     3219.00
## 40 2012-11-09    10766.19
## 41 2012-11-10    10766.19
## 42 2012-11-11    12608.00
## 43 2012-11-12    10765.00
## 44 2012-11-13     7336.00
## 45 2012-11-14    10766.19
## 46 2012-11-15       41.00
## 47 2012-11-16     5441.00
## 48 2012-11-17    14339.00
## 49 2012-11-18    15110.00
## 50 2012-11-19     8841.00
## 51 2012-11-20     4472.00
## 52 2012-11-21    12787.00
## 53 2012-11-22    20427.00
## 54 2012-11-23    21194.00
## 55 2012-11-24    14478.00
## 56 2012-11-25    11834.00
## 57 2012-11-26    11162.00
## 58 2012-11-27    13646.00
## 59 2012-11-28    10183.00
## 60 2012-11-29     7047.00
## 61 2012-11-30    10766.19
```

```r
hist(StepsImpDay$StepsImpTot,
     main = "Histogram of The Total Number of Imputed Steps Taken Each Day",
     breaks = 25,
     xlim = c(0, 25000),
     ylim = c(0, 25),
     xlab = "Total Number of Steps",
     labels = TRUE)
```

![](ReproducibleResearchPA01_files/figure-html/hist_impsteps-1.png)<!-- -->

The calculation of the mean of the total number of steps taken per day.

```r
MeanImpSteps <- mean(StepsImpDay$StepsImpTot, na.rm = TRUE)
MeanImpSteps
```

```
## [1] 10766.19
```

```r
MeanImpStepsRnd <- round(MeanImpSteps, digits = 2)
MeanImpStepsRnd
```

```
## [1] 10766.19
```

The calculation of the median of the total number of steps taken per day.

```r
MedianImpSteps <- median(StepsImpDay$StepsImpTot, na.rm = TRUE)
MedianImpSteps
```

```
## [1] 10766.19
```

```r
MedImpStepsRnd <- round(MedianImpSteps, digits = 2)
MedImpStepsRnd
```

```
## [1] 10766.19
```

The mean value is changed from 9354.23 to 1.076619\times 10^{4} and the median is changed from 1.0395\times 10^{4} to 1.076619\times 10^{4}.


## Are there differences in activity patterns between weekdays and weekends?

Splitting data for weekdays and weekends in different data frames.

```r
WeekDays <- subset(activity, weekDayFact == "weekday")
WeekEnds <- subset(activity, weekDayFact == "weekend")
```

Calculation of the average number of steps for each new data frame.

```r
WeekDaysInt <- aggregate(x = WeekDays$steps, by = list(WeekDays$interval), 
                         FUN = mean, na.rm = TRUE)
    colnames(WeekDaysInt)[colnames(WeekDaysInt) == 'Group.1'] <- 'interval'
    colnames(WeekDaysInt)[colnames(WeekDaysInt) == 'x'] <- 'StepsAvg'
plot <- ggplot(WeekDaysInt, aes(x = interval, y = StepsAvg)) +
        geom_line() +
        labs(x = "5-minute Interval", y = "Avereage of the steps", 
             title="Avereage of number of steps across weekdays")
plot
```

![](ReproducibleResearchPA01_files/figure-html/unnamed-chunk-1-1.png)<!-- -->

```r
WeekEndsInt <- aggregate(x = WeekEnds$steps, by = list(WeekEnds$interval), 
                         FUN = mean, na.rm = TRUE)
    colnames(WeekEndsInt)[colnames(WeekEndsInt) == 'Group.1'] <- 'interval'
    colnames(WeekEndsInt)[colnames(WeekEndsInt) == 'x'] <- 'StepsAvg'
plot <- ggplot(WeekEndsInt, aes(x = interval, y = StepsAvg)) +
        geom_line() +
        labs(x = "5-minute Interval", y = "Avereage of the steps", 
             title="Avereage of number of steps across weekends")
plot
```

![](ReproducibleResearchPA01_files/figure-html/unnamed-chunk-1-2.png)<!-- -->

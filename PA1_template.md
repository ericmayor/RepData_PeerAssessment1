# Peer assessement 1
====

## Loading and preprocessing the data

1 Load the data (i.e. read.csv())
2 Process/transform the data (if necessary) into a format suitable for your analysis


```r
data = read.csv("activity.csv")
complete = na.omit(data)
```


## What is mean total number of steps taken per day?
For this part of the assignment, you can ignore the missing values in the dataset.

1 Make a histogram of the total number of steps taken each day


```r

byDay = data.frame(unlist(tapply(complete$steps, complete$date, sum)))
byDay = as.numeric(unlist(byDay))
nas = is.na(byDay)
byDay[nas] = 0
hist(byDay)
```

![plot of chunk Histogram](figure/Histogram.png) 


2 Calculate and report the mean and median total number of steps taken per day

```r
paste("The mean number of steps by day is: ", mean(byDay), " and the median is: ", 
    median(byDay))
```

```
## [1] "The mean number of steps by day is:  9354.22950819672  and the median is:  10395"
```

## What is the average daily activity pattern?
1 Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)

```r

stepsByInterval = tapply(complete$steps, complete$interval, mean)
plot(stepsByInterval, type = "l")
```

![plot of chunk daily activity](figure/daily_activity.png) 

2 Which five-minute interval, on average across all the days inthe dataset, contains the maximum number of steps?

```r
paste("The interval starting at the following minute has the maximum number of steps:", 
    names(which.max(stepsByInterval)))
```

```
## [1] "The interval starting at the following minute has the maximum number of steps: 835"
```

## Imputing missing values
Note that there are a number of days/intervals where there are missing values (coded as NA). The presence of missing days may introduce bias into some calculations or summaries of the data.

1 Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)

```r
paste("The total number of missing values is: ", sum(is.na(data)))
```

```
## [1] "The total number of missing values is:  2304"
```

2 Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.
3 Create a new dataset that is equal to the original dataset but with the missing data filled in.

```r
data2 = data
data2$steps[is.na(data2$steps)] = mean(na.omit(data2$steps))
```


4 Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?

```r
byDay = tapply(data2$steps, data2$date, sum)
hist(byDay)
```

![plot of chunk by day histogram](figure/by_day_histogram.png) 

```r
paste("The mean number of steps by day is: ", mean(byDay), " and the median is: ", 
    median(byDay))
```

```
## [1] "The mean number of steps by day is:  10766.1886792453  and the median is:  10766.1886792453"
```

The values do differ from the ones found without imputation. In this case, imputing the data impacts both the mean and the median, which are higher than previously. Further, after imputation, the mean and the median have the same value.

## Are there differences in activity patterns between weekdays and weekends?

For this part the weekdays() function may be of some help here. Use the dataset with the filled-in missing values for this part.

1 Create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend day.

```r
weekday = weekdays(as.Date(complete$date))
complete$day[weekday == "samedi" | weekday == "dimanche"] = "weekend"
complete$day[!(weekday == "samedi" | weekday == "dimanche")] = "weekday"
complete$day = as.factor(complete$day)
```

2 Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). The plot should look something like the following, which was creating using simulated data:


```r
ofweekends = subset(complete, day == "weekend")
ofweekdays = subset(complete, day == "weekday")

par(mfrow = c(2, 1))
plot(tapply(ofweekends$steps, ofweekends$interval, mean), type = "l", main = "weekend", 
    xlab = "interval", ylab = "number of steps")
plot(tapply(ofweekdays$steps, ofweekdays$interval, mean), type = "l", main = "weekend", 
    xlab = "interval", ylab = "number of steps")
```

![plot of chunk timeseries](figure/timeseries.png) 


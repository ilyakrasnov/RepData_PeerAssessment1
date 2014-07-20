
# Data Science Track on Coursera 
### Course "Reproducible Research"  - Assigenment 1
### Ilya Krasnov


### Loading data


```r
activity <- read.csv("activity.csv")
```


### What is mean total number of steps taken per day?


```r
stepsperday <- aggregate(activity["steps"],activity["date"],FUN=sum)

hist(stepsperday$steps)
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2.png) 

#### Claculate mean and median

```r
mean(stepsperday$steps,na.rm=TRUE)
```

```
## [1] 10766
```

```r
median(stepsperday$steps,na.rm=TRUE)
```

```
## [1] 10765
```

### What is the average daily activity pattern?

```r
avgstepsperinterval <- aggregate(activity["steps"],activity["interval"],FUN=mean, na.rm=TRUE)
plot(avgstepsperinterval,type="l",xlab="5 min interval",ylab="average steps taken")
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4.png) 

#### Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

```r
avgstepsperinterval[which.max(avgstepsperinterval$steps),"interval"]
```

```
## [1] 835
```

### Imputing missing values

#### Total number of missing values in the dataset

```r
sum(!complete.cases(activity))
```

```
## [1] 2304
```

#### Create new dataset and fill it with average steps for intervals

```r
activityfillna <- activity

for (i in 1:nrow(activityfillna)) {
     if (is.na(activityfillna[i,"steps"])) activityfillna[i,"steps"] <- avgstepsperinterval[which(avgstepsperinterval$interval==activityfillna[i,"interval"]),"steps"]
 }
```

#### New daily stats

```r
stepsperdayfillna <- aggregate(activityfillna["steps"],activityfillna["date"],FUN=sum)

hist(stepsperdayfillna$steps)
```

![plot of chunk unnamed-chunk-8](figure/unnamed-chunk-8.png) 

#### Claculate mean and median

```r
mean(stepsperdayfillna$steps,na.rm=TRUE)
```

```
## [1] 10766
```

```r
median(stepsperdayfillna$steps,na.rm=TRUE)
```

```
## [1] 10766
```

### Difference between weekdays and weekends

#### Transform dates column to dates format

```r
activity$date <- as.Date(activity$date)
```

#### Add new factor variable weekday

```r
activity$weekday <- ifelse(weekdays(activity$date)=="Sunday","weekend",ifelse(weekdays(activity$date)=="Saturday","weekend","weekday"))
activity$weekday <- as.factor(activity$weekday)
```
#### Summarize average steps per interval per weekday/weekend and plot it

```r
library(lattice)
avgstepsperintervalweekday <- aggregate(activity["steps"],c(activity["interval"],activity["weekday"]),FUN=mean, na.rm=TRUE)
xyplot(steps ~ interval | weekday, data=avgstepsperintervalweekday, layout = c(1,2),type="l")
```

![plot of chunk unnamed-chunk-12](figure/unnamed-chunk-12.png) 

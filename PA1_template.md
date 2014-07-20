
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

```r
sum(!complete.cases(activity))
```

```
## [1] 2304
```

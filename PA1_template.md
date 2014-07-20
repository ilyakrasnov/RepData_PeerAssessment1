
# Data Science Track on Coursera 
### Course "Reproducible Research"  - Assigenment 1
### Ilya Krasnov


### Loading data


```r
activity <- read.csv("activity.csv")
```

### What is mean total number of steps taken per day?


```r
library(plyr)
stepsperday <- ddply(activity,~date,summarise,sum=sum(steps))
stepsperday <-stepsperday[,"sum"]
stepsperday <- na.omit(stepsperday)
hist(stepsperday)
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2.png) 

#### Claculate mean and median

```r
mean(stepsperday)
```

```
## [1] 10766
```

```r
median(stepsperday)
```

```
## [1] 10765
```

### What is the average daily activity pattern?


### Imputing missing values

```r
sum(!complete.cases(activity))
```

```
## [1] 2304
```

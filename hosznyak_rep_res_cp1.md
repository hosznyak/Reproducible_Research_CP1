Loading and preprocessing the data
==================================

The first step is to load the data from the activity.csv. The summary
shows that there are many empty values on the steps column.

    activity <- read.csv("activity.csv")
    summary(activity)

    ##      steps                date          interval     
    ##  Min.   :  0.00   2012-10-01:  288   Min.   :   0.0  
    ##  1st Qu.:  0.00   2012-10-02:  288   1st Qu.: 588.8  
    ##  Median :  0.00   2012-10-03:  288   Median :1177.5  
    ##  Mean   : 37.38   2012-10-04:  288   Mean   :1177.5  
    ##  3rd Qu.: 12.00   2012-10-05:  288   3rd Qu.:1766.2  
    ##  Max.   :806.00   2012-10-06:  288   Max.   :2355.0  
    ##  NA's   :2304     (Other)   :15840

What is mean total number of steps taken per day?
=================================================

The total number of steps taken per day
---------------------------------------

Before analyse the total number of steps taken per day, have to
aggregate the data by days in a new variable. For the summary, the
maximum value of x axis and the frequency breaks can be set.

    activity_day_total <- with(activity, aggregate(steps, by = list(date), FUN = sum, na.rm = TRUE))
    names(activity_day_total) <- c("date", "sum_of_step")
    summary(activity_day_total)

    ##          date     sum_of_step   
    ##  2012-10-01: 1   Min.   :    0  
    ##  2012-10-02: 1   1st Qu.: 6778  
    ##  2012-10-03: 1   Median :10395  
    ##  2012-10-04: 1   Mean   : 9354  
    ##  2012-10-05: 1   3rd Qu.:12811  
    ##  2012-10-06: 1   Max.   :21194  
    ##  (Other)   :55

Histogram of the total number of steps taken each day
-----------------------------------------------------

The histogram shows the frequency of the total number of steps taken
each day.

    hist(activity_day_total$sum_of_step, main = "The total number of steps taken each day", breaks = 15, xlab = "Total steps by day", col = "red", ylim = c(0,20), xlim = c(0,25000) )

![](hosznyak_rep_res_cp1_files/figure-markdown_strict/unnamed-chunk-3-1.png)

The mean and median of the total number of steps taken per day
--------------------------------------------------------------

The median of the total number of steps taken per day is the following:

    median(activity_day_total$sum_of_step)

    ## [1] 10395

The mean of the total number of steps taken per day is the following:

    mean(activity_day_total$sum_of_step)

    ## [1] 9354.23

What is the average daily activity pattern?
===========================================

Before analyse the mean number of steps taken per interval, have to
average the data by interval in a new variable.

    activity_int_mean <- with(activity, aggregate(steps, by = list(interval), FUN = mean, na.rm = TRUE))
    names(activity_int_mean) <- c("interval", "mean_of_step")
    summary(activity_int_mean)

    ##     interval       mean_of_step    
    ##  Min.   :   0.0   Min.   :  0.000  
    ##  1st Qu.: 588.8   1st Qu.:  2.486  
    ##  Median :1177.5   Median : 34.113  
    ##  Mean   :1177.5   Mean   : 37.383  
    ##  3rd Qu.:1766.2   3rd Qu.: 52.835  
    ##  Max.   :2355.0   Max.   :206.170

Time series of the average number of steps
------------------------------------------

    plot(activity_int_mean$interval, activity_int_mean$mean_of_step, type = "l", lwd = 3, xlab = "Intervals (hhmm)", ylab = "Average step", main = "Time series of the average number of steps", col = "red")

![](hosznyak_rep_res_cp1_files/figure-markdown_strict/unnamed-chunk-7-1.png)

Which 5-minute interval contains the maximum number of steps?
-------------------------------------------------------------

    activity_int_mean[which.max(activity_int_mean$mean_of_step),]$interval

    ## [1] 835

Imputing missing values
=======================

There are a number of days/intervals where there are missing values
(coded as NA). The presence of missing days may introduce bias into some
calculations or summaries of the data.

The total number of rows with NA in the dataset
-----------------------------------------------

    na_index <- is.na(activity$steps)
    sum(na_index)

    ## [1] 2304

Devise a strategy for filling in all of the missing values in the dataset
-------------------------------------------------------------------------

The strategy does not need to be sophisticated, but I use the mean for
that 5-minute interval. This value is 37.3826.

    na_fill_mean <- mean(activity_int_mean$mean_of_step)
    na_fill_mean

    ## [1] 37.3826

Create a new dataset
--------------------

Create a new dataset that is equal to the original dataset but with the
missing data filled in.

    activity_fill <- activity
    activity_fill[na_index, 1] <- na_fill_mean
    head(activity_fill)

    ##     steps       date interval
    ## 1 37.3826 2012-10-01        0
    ## 2 37.3826 2012-10-01        5
    ## 3 37.3826 2012-10-01       10
    ## 4 37.3826 2012-10-01       15
    ## 5 37.3826 2012-10-01       20
    ## 6 37.3826 2012-10-01       25

    summary(activity_fill)

    ##      steps                date          interval     
    ##  Min.   :  0.00   2012-10-01:  288   Min.   :   0.0  
    ##  1st Qu.:  0.00   2012-10-02:  288   1st Qu.: 588.8  
    ##  Median :  0.00   2012-10-03:  288   Median :1177.5  
    ##  Mean   : 37.38   2012-10-04:  288   Mean   :1177.5  
    ##  3rd Qu.: 37.38   2012-10-05:  288   3rd Qu.:1766.2  
    ##  Max.   :806.00   2012-10-06:  288   Max.   :2355.0  
    ##                   (Other)   :15840

Aggregate the data for the histogram
------------------------------------

    activity_fill_day_total <- with(activity_fill, aggregate(steps, by = list(date), FUN = sum, na.rm = TRUE))
    names(activity_fill_day_total) <- c("date", "sum_of_step")
    summary(activity_fill_day_total)

    ##          date     sum_of_step   
    ##  2012-10-01: 1   Min.   :   41  
    ##  2012-10-02: 1   1st Qu.: 9819  
    ##  2012-10-03: 1   Median :10766  
    ##  2012-10-04: 1   Mean   :10766  
    ##  2012-10-05: 1   3rd Qu.:12811  
    ##  2012-10-06: 1   Max.   :21194  
    ##  (Other)   :55

Histogram of the total number of steps taken each day
-----------------------------------------------------

The histogram shows the frequency of the total number of steps taken
each day.

    hist(activity_fill_day_total$sum_of_step, main = "The total number of steps taken each day", breaks = 15, xlab = "Total steps by day", col = "red", ylim = c(0,20), xlim = c(0,25000) )

![](hosznyak_rep_res_cp1_files/figure-markdown_strict/unnamed-chunk-13-1.png)

The mean and median of the total number of steps taken per day
--------------------------------------------------------------

The median of the total number of steps taken per day is the following:

    median(activity_fill_day_total$sum_of_step)

    ## [1] 10766.19

The mean of the total number of steps taken per day is the following:

    mean(activity_fill_day_total$sum_of_step)

    ## [1] 10766.19

Are there differences in activity patterns between weekdays and weekends?
=========================================================================

Create a new factor variable (weekday, weekend)
-----------------------------------------------

Creating a new factor variable - called weekday - With the weekdays and
ifelse functions.

    activity_fill$day_name <- as.factor(weekdays(as.Date(activity_fill$date)))
    activity_fill$weekday <- as.factor(ifelse(activity_fill$day_name == "szombat" | activity_fill$day_name == "vasárnap", "weekend", "weekday"))

Time series plot for weekdays and weekends
------------------------------------------

The following plot shows the difference of the number of the average
steps on weekdays or weekends.

    activity_fill_weekday_mean <- with(activity_fill, aggregate(steps ~ weekday + interval, data = activity_fill, FUN = mean))

    library(ggplot2)
    plot <- ggplot(activity_fill_weekday_mean, aes(x = interval, y = steps)) +
                geom_line(size = 1, color = "red") +
                facet_wrap( ~ weekday, nrow = 2) +
                labs(title = "Average steps by interval", x = "Interval", y = "Steps")

    plot

![](hosznyak_rep_res_cp1_files/figure-markdown_strict/unnamed-chunk-17-1.png)

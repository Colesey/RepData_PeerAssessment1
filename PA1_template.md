# Reproducible Research: Peer Assessment 1
Chris Coles  
Nov 8th 2015  
##Introduction  
This assignment makes use of data from a personal activity monitoring
device. This device collects data at 5 minute intervals through out the
day. The data consists of two months of data from an anonymous
individual collected during the months of October and November, 2012
and include the number of steps taken in 5 minute intervals each day.


The objective of this reproducible research is to answer a number of questions using the data provided:  
1. What is mean total number of steps taken per day?  
2. What is the average daily activity pattern?  
3. Imputing missing values  
4. Are there differences in activity patterns between weekdays and weekends?  

In the data set, the variables are:

* **steps**: Number of steps taking in a 5-minute interval (missing
    values are coded as `NA`)

* **date**: The date on which the measurement was taken in YYYY-MM-DD
    format

* **interval**: Identifier for the 5-minute interval in which
    measurement was taken


## Loading and preprocessing the data
The following code assumes that the data files have been downloaded and unzipped in the current working directory.

The data is loaded from activities.csv and into the data frame called activities. Then convert the date variable from a factor to a date using strptime() function and converting it's output to POSIXct as required later when using the group_by() function.


```r
activities <- read.csv(file= "activity.csv", header=TRUE)
activities$date <- as.POSIXct(strptime(activities$date, format = "%Y-%m-%d"))
```

The summary function output is displayed here (only because I can). I've used the kable() function instead of the xtable becasue the later refused to format properly into the HTML document. The kable() snippet was found on Stack Overflow (and is better than xtable but still not perfect).


```r
library(knitr)

sum_info <- summary(activities)
kable(sum_info, format = "html", padding = 1, row.names=TRUE)
```

<table>
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:left;">     steps </th>
   <th style="text-align:left;">      date </th>
   <th style="text-align:left;">    interval </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Min.   :  0.00 </td>
   <td style="text-align:left;"> Min.   :2012-10-01 00:00:00 </td>
   <td style="text-align:left;"> Min.   :   0.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> 1st Qu.:  0.00 </td>
   <td style="text-align:left;"> 1st Qu.:2012-10-16 00:00:00 </td>
   <td style="text-align:left;"> 1st Qu.: 588.8 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Median :  0.00 </td>
   <td style="text-align:left;"> Median :2012-10-31 00:00:00 </td>
   <td style="text-align:left;"> Median :1177.5 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Mean   : 37.38 </td>
   <td style="text-align:left;"> Mean   :2012-10-30 23:32:27 </td>
   <td style="text-align:left;"> Mean   :1177.5 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> 3rd Qu.: 12.00 </td>
   <td style="text-align:left;"> 3rd Qu.:2012-11-15 00:00:00 </td>
   <td style="text-align:left;"> 3rd Qu.:1766.2 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Max.   :806.00 </td>
   <td style="text-align:left;"> Max.   :2012-11-30 00:00:00 </td>
   <td style="text-align:left;"> Max.   :2355.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> NA's   :2304 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
  </tr>
</tbody>
</table>


## What is mean total number of steps taken per day?
Ignoring NA values for this exercise. 
Using dplyr to group the data by the date variable, I've created a table called byDate that is then used to summarise (I'm English) the data into a table called stepsPerDay. This is used to produce the histogram the mean and the median.

First task is to poduce a histogram plot of the steps per day. This is shown with the base plotting hist() function with the breaks argument set to 20 to provide a little granularity in the plot.
The We are calculate both the mean and the median for the number of steps taken per day across the sample period.   

Far additional information, a rug plot has been added to the histogram.  

Note: I've set warn.conflicts to FALSE when loading the dplry library to tidy up the output html file.


```r
library(dplyr, warn.conflicts = FALSE)
byDate <- group_by(activities, date)
stepsPerDay <- summarise(byDate, sum(steps))
hist(stepsPerDay$"sum(steps)", col="blue", 
     main ="Histogram of Steps per Day",
     xlab = "Number of steps per day",
     breaks = 20)
rug(stepsPerDay$"sum(steps)")
```

![](PA1_template_files/figure-html/mean-1.png) 

```r
mean(stepsPerDay$"sum(steps)", na.rm = TRUE)
```

```
## [1] 10766.19
```

```r
median(stepsPerDay$"sum(steps)", na.rm = TRUE)
```

```
## [1] 10765
```


## What is the average daily activity pattern?

1. Make a time series plot (i.e. `type = "l"`) of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)  
  
To achieve this, we create an array object using the tapply() function that groups the data by the slot in the day defined by the interval value and finding the mean number of steps taken in that slot. The array is named dailyActivitiy.  
This is then plotted as a time series graph using the base graphics plot() function.


```r
dailyActivity <- tapply(activities$steps, activities$interval, mean, na.rm=TRUE)
plot(dailyActivity, type="l", col="red", 
     main = "Time Series Plot for Average Number of Steps",
     xlab = "Daily Time Slot (5 minute intervals)",
     ylab = "Average number of Steps")
```

![](PA1_template_files/figure-html/daily activity-1.png) 
  
2. Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?    

To answer this question we find the time slot with the highest mean in the dailyActivity array.  
The index of the array is the time of day that the mean occurs in. In this case, 08:35 with a mean value of 206.1698 steps.



```r
dailyActivity[dailyActivity ==max(dailyActivity)]
```

```
##      835 
## 206.1698
```



## Imputing missing values

Within the data there are a number of days/intervals where there are missing
values (coded as `NA`). The presence of missing days may introduce
bias into some calculations or summaries of the data. Assocaited with this the task is to work out a strategy for removing the NA values to understand the effect of them. This task is divided into 5 actions 

1. Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with `NA`s)


```r
colSums(is.na(activities))
```

```
##    steps     date interval 
##     2304        0        0
```
  
From the output of the colSums() function we can see that there are a total of 2304 missing values and they are all in the Steps variable.  

2. Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.  





## Are there differences in activity patterns between weekdays and weekends?

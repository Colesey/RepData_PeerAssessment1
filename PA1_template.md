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

The data is loaded from activities.csv and into the data frame called activities. Then convert the date variable from a factor to a date using strptime() function:


```r
activities <- read.csv(file= "activity.csv", header=TRUE)
activities$date <- strptime(activities$date, format = "%Y-%m-%d")
```

The summary function output is displayed here (only because I can). I've used the kable() function instead of the xtable becasue the later refused to format properly into the HTML document. The kable() snippet was found on Stack Oerflow (and is better than xtable but still not perfect).

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
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> Min.   :  0.00 </td>
   <td style="text-align:left;"> Min.   :2012-10-01 00:00:00 </td>
   <td style="text-align:left;"> Min.   :   0.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2 </td>
   <td style="text-align:left;"> 1st Qu.:  0.00 </td>
   <td style="text-align:left;"> 1st Qu.:2012-10-16 00:00:00 </td>
   <td style="text-align:left;"> 1st Qu.: 588.8 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 3 </td>
   <td style="text-align:left;"> Median :  0.00 </td>
   <td style="text-align:left;"> Median :2012-10-31 00:00:00 </td>
   <td style="text-align:left;"> Median :1177.5 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 4 </td>
   <td style="text-align:left;"> Mean   : 37.38 </td>
   <td style="text-align:left;"> Mean   :2012-10-30 23:32:27 </td>
   <td style="text-align:left;"> Mean   :1177.5 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 5 </td>
   <td style="text-align:left;"> 3rd Qu.: 12.00 </td>
   <td style="text-align:left;"> 3rd Qu.:2012-11-15 00:00:00 </td>
   <td style="text-align:left;"> 3rd Qu.:1766.2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 6 </td>
   <td style="text-align:left;"> Max.   :806.00 </td>
   <td style="text-align:left;"> Max.   :2012-11-30 00:00:00 </td>
   <td style="text-align:left;"> Max.   :2355.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 7 </td>
   <td style="text-align:left;"> NA's   :2304 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> NA </td>
  </tr>
</tbody>
</table>


## What is mean total number of steps taken per day?





## What is the average daily activity pattern?



## Imputing missing values
# Not working - so leave it for now.
# isNAs <- colSums(is.na(activities$steps))




## Are there differences in activity patterns between weekdays and weekends?

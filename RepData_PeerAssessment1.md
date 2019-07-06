---
title: "RepData_PeerAssessment1"
author: "joedinoto"
date: "7/6/2019"
output: 
  html_document: 
    keep_md: yes
---



## Loading and preprocessing the data

Load the data into a dataframe.

```r
unzip("activity.zip")
DF<- read.csv("activity.csv")
```
Invoke the powers of the tidyverse to make the date column read as dates, the interval column as factors, and turn the whole dataframe into a tibble.

```r
library(tidyverse)
```

```
## Warning: package 'tidyverse' was built under R version 3.5.3
```

```
## -- Attaching packages -------------------------------------------------------------------- tidyverse 1.2.1 --
```

```
## v ggplot2 3.1.1       v purrr   0.3.2  
## v tibble  2.1.1       v dplyr   0.8.0.1
## v tidyr   0.8.3       v stringr 1.4.0  
## v readr   1.3.1       v forcats 0.4.0
```

```
## Warning: package 'ggplot2' was built under R version 3.5.3
```

```
## Warning: package 'tibble' was built under R version 3.5.3
```

```
## Warning: package 'tidyr' was built under R version 3.5.3
```

```
## Warning: package 'purrr' was built under R version 3.5.3
```

```
## Warning: package 'dplyr' was built under R version 3.5.3
```

```
## Warning: package 'forcats' was built under R version 3.5.3
```

```
## -- Conflicts ----------------------------------------------------------------------- tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(lubridate)
```

```
## Warning: package 'lubridate' was built under R version 3.5.3
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following object is masked from 'package:base':
## 
##     date
```

```r
DF$date <- ymd(DF$date)
DF$interval <- as.factor(DF$interval)
DF<- as_tibble(DF)
```


## What is mean total number of steps taken per day?

For this part of the assignment, you can ignore the missing values in the dataset.

 1. Make a histogram of the total number of steps taken each day
 
First, figure out the total number of steps taken each day using dplyr. 

```r
newDF <- na.omit(DF)
by_date<- newDF %>%
  group_by(date,add=TRUE) %>%
  summarise(total_steps=sum(steps))
by_date
```

```
## # A tibble: 53 x 2
##    date       total_steps
##    <date>           <int>
##  1 2012-10-02         126
##  2 2012-10-03       11352
##  3 2012-10-04       12116
##  4 2012-10-05       13294
##  5 2012-10-06       15420
##  6 2012-10-07       11015
##  7 2012-10-09       12811
##  8 2012-10-10        9900
##  9 2012-10-11       10304
## 10 2012-10-12       17382
## # ... with 43 more rows
```

Now create the histogram.

```r
with(by_date,hist(total_steps))
```

![](RepData_PeerAssessment1_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

 2. Calculate and report the mean and median total number of steps taken per day

```r
by_date_mean <- newDF %>%
  group_by(date) %>%
  summarise(mean(steps))
by_date_mean
```

```
## # A tibble: 53 x 2
##    date       `mean(steps)`
##    <date>             <dbl>
##  1 2012-10-02         0.438
##  2 2012-10-03        39.4  
##  3 2012-10-04        42.1  
##  4 2012-10-05        46.2  
##  5 2012-10-06        53.5  
##  6 2012-10-07        38.2  
##  7 2012-10-09        44.5  
##  8 2012-10-10        34.4  
##  9 2012-10-11        35.8  
## 10 2012-10-12        60.4  
## # ... with 43 more rows
```
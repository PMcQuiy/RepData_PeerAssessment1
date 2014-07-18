---
title: "Reproducible Reaseach Peer Assessment 1"
author: "Parmutia Makui"
date: "Friday, July 18, 2014"
output: html_document
---
##Loading and preprocessing data
```{r}
setwd("D:/Coursera/Reproducible Research/PA1")
#load the activity data
Adata <- read.csv("activity.csv")
head(Adata,5)
```

##What is the total number of steps per day?
## 1. Drawing a histogram of the data
```{r}
library(ggplot2)
Adata$DateOrder <- reorder(Adata$date, Adata$steps)
ggplot(Adata, aes(x=date, y=steps)) + geom_bar(aes(DateOrder), stat="identity")
```

## 2. Mean and median of the steps taken
```{r}
MeanSteps <- mean(Adata$steps, na.rm=TRUE)
MeanSteps
MedianSteps <- median(Adata$steps, na.rm=TRUE)
MedianSteps
```

##What is the average daily activity patern


##Imputing missing values
## 1. Reporting number of missing values
```{r}
sum(is.na(Adata$steps))
sapply(Adata, function(x) sum(is.na(x)))
```

##2. Filling in missing values
```{r}
library(plyr)
my.mean <- function(x) replace(x, is.na(x), mean(x, na.rm = TRUE))
Adata.imputed <- plyr::ddply(Adata[1:3], .(interval), transform,
                                 steps = my.mean(steps),
                                 date = date,
                                 interval = interval)
```

## 3. Histogram of newdataset
```{r}
Adata.imputed$DateOrder <- reorder(Adata.imputed$date, Adata.imputed$steps)
ggplot(Adata.imputed, aes(x=date, y=steps)) + geom_bar(aes(DateOrder), stat="identity")
MeanStepsNew <- mean(Adata.imputed$steps, na.rm=TRUE)
MeanStepsNew
MedianStepsNew <- median(Adata.imputed$steps, na.rm=TRUE)
MedianStepsNew

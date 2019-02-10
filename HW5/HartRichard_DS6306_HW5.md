---
title: "Homework 5"
author: "Richard Hart"
date: "2/7/2019"
output: 
  html_document:
    keep_md: true
---

# Homework Assignment 5

### Question 1 Data Munging

Loading Libraries

```r
library(tidyr)
```

a. Importing, opening and creating a data frame from yob2016.txt file.

```r
df <- read.csv(file = "/Users/rhart/Dropbox/SMU Classes/6306 Doing DataScience/Homework/HW 5/yob2016.txt", header = FALSE, sep = ";")
```

b. Summary and Structure of df

```r
summary(df)
```

```
##        V1        V2              V3         
##  Aalijah:    2   F:18758   Min.   :    5.0  
##  Aaliyan:    2   M:14111   1st Qu.:    7.0  
##  Aamari :    2             Median :   12.0  
##  Aarian :    2             Mean   :  110.7  
##  Aarin  :    2             3rd Qu.:   30.0  
##  Aaris  :    2             Max.   :19414.0  
##  (Other):32857
```

```r
str(df)
```

```
## 'data.frame':	32869 obs. of  3 variables:
##  $ V1: Factor w/ 30295 levels "Aaban","Aabha",..: 9317 22546 3770 26409 12019 20596 6185 339 9298 11222 ...
##  $ V2: Factor w/ 2 levels "F","M": 1 1 1 1 1 1 1 1 1 1 ...
##  $ V3: int  19414 19246 16237 16070 14722 14366 13030 11699 10926 10733 ...
```

c. Find the duplicate name that contains 3 y's

```r
grep('yyy', df$V1)
```

```
## [1] 212
```

d. Remove the row that contains the duplicate name found in c.

```r
y2016 <- df[-grep('yyy', df$V1), ]
```

### Question 2 Data Merging

a. Importing, opening and creating a data frame from yob2015.txt file.

```r
y2015 <- read.csv(file = "/Users/rhart/Dropbox/SMU Classes/6306 Doing DataScience/Homework/HW 5/yob2015.txt", header = FALSE, sep = ",")
```

b. Display the 10 rows.

```r
tail(y2015, 10)
```

```
##           V1 V2 V3
## 33054   Ziyu  M  5
## 33055   Zoel  M  5
## 33056  Zohar  M  5
## 33057 Zolton  M  5
## 33058   Zyah  M  5
## 33059 Zykell  M  5
## 33060 Zyking  M  5
## 33061  Zykir  M  5
## 33062  Zyrus  M  5
## 33063   Zyus  M  5
```

What's interesting about these last 10 rows is that they're all very similar to each other in spelling. For instance, the names Zykir, Xyrus, and Zyus are only a few letters different from each other.

c. Merge both y2015 and y2016 together by the name column.

```r
final <- merge(y2015, y2016, by = "V1")
```

### Question 3 Data Summary

a. Add a new "Total" column and the number of children for each year together

```r
final$Total <- final$V3.x + final$V3.y
```

b. Sort final by Total

```r
tail(final[order(final$Total), ], 10)
```

```
##             V1 V2.x  V3.x V2.y  V3.y Total
## 12698 Isabella    F 15574    F 14722 30296
## 13054    Jacob    M 15914    M 14416 30330
## 30128  William    M 15863    M 15668 31531
## 21102    Mason    M 16591    M 15192 31783
## 3725       Ava    F 16340    F 16237 32577
## 27782   Sophia    F 17381    F 16070 33451
## 19277     Liam    M 18330    M 18138 36468
## 23258     Noah    M 19594    M 19015 38609
## 23607   Olivia    F 19638    F 19246 38884
## 9820      Emma    F 20415    F 19414 39829
```

c. Top 10 Most Popular Girls' Names

```r
girls <- final[-grep('M', final$V2.x), ]
tail(girls[order(girls$Total), ], 10)
```

```
##              V1 V2.x  V3.x V2.y  V3.y Total
## 11838    Harper    F 10283    F 10733 21016
## 9799      Emily    F 11766    F 10926 22692
## 302     Abigail    F 12371    F 11699 24070
## 6509  Charlotte    F 11381    F 13030 24411
## 21722       Mia    F 14871    F 14366 29237
## 12698  Isabella    F 15574    F 14722 30296
## 3725        Ava    F 16340    F 16237 32577
## 27782    Sophia    F 17381    F 16070 33451
## 23607    Olivia    F 19638    F 19246 38884
## 9820       Emma    F 20415    F 19414 39829
```

d. Write only the girls' names to CSV

```r
write.csv(girls[,c("V1", "Total")], file="MostPopularGirlsNames.csv",row.names=FALSE)
```

### Question 4 GitHub Location

```r
https://github.com/rhart211/DS6306_HW/tree/master/HW5
```

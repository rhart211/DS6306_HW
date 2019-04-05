---
title: "Homework 12 -- Time Series"
author: "Richard Hart"
date: "April 5, 2019"
output: 
  html_document:
    keep_md: true
---

# Homework Assignment 12 -- Time Series

Loading Libraries

```r
library(dygraphs)
library(forecast)
library(fpp2)
```

```
## Loading required package: ggplot2
```

```
## Loading required package: fma
```

```
## Loading required package: expsmooth
```

### 1. Warm Up: Brief Financial Data, European Stock Market DataSets

a. European Stock Market Data Information

```r
help("EuStockMarkets")
```


```r
str(EuStockMarkets)
```

```
##  Time-Series [1:1860, 1:4] from 1991 to 1999: 1629 1614 1607 1621 1618 ...
##  - attr(*, "dimnames")=List of 2
##   ..$ : NULL
##   ..$ : chr [1:4] "DAX" "SMI" "CAC" "FTSE"
```

Create dataset from the DAX Index

```r
Eu_DAX <- EuStockMarkets[, "DAX"]
```

b. Plot of European Stock Market Data with an Indication at 1997

```r
plot(Eu_DAX, col="blue", main="Daily Closing Prices of the European Stock Index, DAX (1991-1998)", ylab="European Stock Market Index DAX", xlab="Year")
abline(v=1997, col="red")
```

![](HartRichard_DS6306_HW12_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

c. Decomposition of DAX Index Time Series into its components (multiplicative model)

```r
EuDaxComp <- decompose(Eu_DAX, type = "multiplicative")
plot(EuDaxComp, col="blue", ylab="Component", xlab="Year" )
abline(v=1997, col="red")
```

![](HartRichard_DS6306_HW12_files/figure-html/unnamed-chunk-6-1.png)<!-- -->


### 2. Temperature Data (maxTemp)

a. Information about the maxtemp dataset

```r
help("maxtemp")
```


```r
str(maxtemp)
```

```
##  Time-Series [1:46] from 1971 to 2016: 34.6 39.3 40.5 36.8 39.7 40.5 41.5 38.2 41.4 41.5 ...
##  - attr(*, "names")= chr [1:46] "1971" "1972" "1973" "1974" ...
```

```r
plot(maxtemp, col="blue", main="Maximum Annual Temperature (Celsius) for Moorabbin Airport, 1971-2016", xlab="Year", ylab="Temp (Degress Ceslsius)")
abline(v=1990, col="red")
```

![](HartRichard_DS6306_HW12_files/figure-html/unnamed-chunk-8-1.png)<!-- -->


b. Eliminating data before the year 1990

```r
temp_1990 <- window(maxtemp, start=1990)
```

c. Maximum Temperature Predictions for the Next 5 Years in Melbourne, Simple Exponential Smoothing.  

```r
# After careful analysis it was found that setting the alpha to 0.6 produced the best 
# predicted value line
fit1 <- ses(temp_1990, alpha=0.6, initial = "optimal", h=5)
plot(fit1, PI=FALSE, ylab="Temp (Degress Ceslsius)", xlab="Year", main="Max Annual Temperature Predictions at Moorabbin, 1990-2021", fcol = "white", type = "o")
lines(fitted(fit1), col="blue", type="o")
lines(fit1$mean, col="blue", type="o")
```

![](HartRichard_DS6306_HW12_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

d. Maximum Temperature Predictions for the Next 5 Years in Melbourne, Holt's Linear Trend.

```r
fit2 <- holt(temp_1990, alpha = 0.8, beta = 0.2, initial = "optimal", damped = TRUE, h = 5)
plot(fit2, PI=FALSE, ylab="Temp (Degress Ceslsius)", xlab="Year", main="Max Annual Temperature Predictions at Moorabbin, 1990-2021 (Holt)", fcol = "white", type = "o")
lines(fitted(fit2), col="blue", type="o")
lines(fit2$mean, col="blue", type="o")
```

![](HartRichard_DS6306_HW12_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

 * AICc for the fitted Model, fit2

```r
fit2$model$aicc
```

```
## [1] 157.9802
```

e. AICc Comparison
 * AICc for the fitted Model, fit1

```r
fit1$model$aicc
```

```
## [1] 144.2461
```

 * AICc for the fitted Model, fit2

```r
fit2$model$aicc
```

```
## [1] 157.9802
```

 * The AICc value for the model that utilizes, Simple Expotential Smoothing (fit1), is 144.2461, and the AICc for the model that utilizes Holt's Linear Trend method (fit2), is 157.9802. The accepted convention is to choose the model that has the lowest AICc value. Based on that convention, the best model is fit1 because it's AICc value is the lowest, at 147.2461.

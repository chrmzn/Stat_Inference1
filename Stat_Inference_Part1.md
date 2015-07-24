# Exponential Distribution Analysis
Chris O'Brien  
13 June 2015  

## Overview

In this analysis we set out to investigate the Exponential Distribution ([Further Info][1]) and how samples pulled from the distribution can help us understand the theoretical mean and variance. 

We also investigate how the means of our samples are distributed, whether they are normally distibuted and how this relates to the Central Limit Theorum. 

## Simulations


## Sample Mean versus Theoretical Mean

We know that the theorectical mean of the exponential distribution is $\lambda^{-1}$, as our simulations the choice of lambda was 0.2 the theorectical mean should be 5.
$$0.2^{-1} = 5$$
From Figure 1 (Mean of 40 samples from Exponential Distribution) we have plotted the frequency of the means of our one thousand size 40 samples. The vertical dotted line represents the theoretical mean we discuessed earlier.

Applying a density function to the graph in Figure 2 

It is clear from the figure that the central tendancy of our means is around the theoretical mean of 5. While each sample mean can vary wildly, in aggregate we get more means that are near to our theoretical. 

Finally taking the mean of our means we get the result 4.999702

## Sample Mean versus Theoretical Mean

Similar to our analysis of the theoretical mean we know that the variance can be calculated by $\lambda^{-2}$, again, as our choice of lambda in this instance was 0.2 the theorectical variance should be 25
$$0.2^{-2} = 25$$

From Figure 3 (Variance of 40 samples from Exponential Distribution) we have plotted the frequency of the variances of our one thousand size 40 samples. The vertical dotted line represents the theoretical variance we discussed earlier.

Applying a density function to the graph in Figure 4

It is clear from the figure that the central tendancy of our means is around the theoretical mean of 5. While each sample mean can vary wildly, in aggregate we get more means that are near to our theoretical. 

Finally taking the mean of our means we get the result 4.999702

## Distribution

How can we tell that this is in line with the central limit theorem. We plot the standard normal distribution against our own density function. While it's not a perfect replica of the standard normal it represents a very similar shape


```r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
## 
## The following object is masked from 'package:stats':
## 
##     filter
## 
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
library(ggplot2)
library(data.table)
```

```
## 
## Attaching package: 'data.table'
## 
## The following objects are masked from 'package:dplyr':
## 
##     between, last
```

```r
source("http://peterhaschke.com/Code/multiplot.R")
```


```r
# Start by setting the seed. This is important for reproducibility
set.seed(100)
lambda <- 0.2
expMatrix <- matrix(rexp(40000, lambda), ncol = 40)
expMeans <- data.table(rowMeans(expMatrix))
expVars <- data.table(apply(expMatrix,FUN=var,MARGIN=1))
```


```r
ggplot(expMeans,aes(x=V1)) + geom_histogram(aes(fill=..count..)) + geom_vline(xintercept=lambda^-1, linetype='dotted', color='red') + labs(x='Mean') + ggtitle('Mean of 40 samples from Exponential Distribution')
```

![](Stat_Inference_Part1_files/figure-html/figure_1-1.png) 


```r
ggplot(expMeans,aes(x=V1)) + geom_histogram(aes(y=..density..)) + geom_vline(xintercept=lambda^-1, linetype='dotted', color='red') + labs(x='Mean') + ggtitle('Mean of 40 samples from Exponential Distribution') + geom_density(color='blue')
```

![](Stat_Inference_Part1_files/figure-html/figure_2-1.png) 


```r
ggplot(expVars,aes(x=V1)) + geom_histogram(aes(fill=..count..)) + geom_vline(xintercept=lambda^-2, linetype='dotted', color='red') + labs(x='Mean') + ggtitle('Mean of 40 samples from Exponential Distribution')
```

![](Stat_Inference_Part1_files/figure-html/figure_3-1.png) 


```r
ggplot(expVars,aes(x=V1)) + geom_histogram(aes(y=..density..)) + geom_vline(xintercept=lambda^-2, linetype='dotted', color='red') + labs(x='Mean') + ggtitle('Mean of 40 samples from Exponential Distribution') + geom_density(color='blue')
```

![](Stat_Inference_Part1_files/figure-html/figure_4-1.png) 


```r
snValues <- expMeans - mean(expMeans$V1)
ggplot(snValues, aes(x=V1)) + geom_density(aes(y=..density.., position="stack"), color='red')  + stat_function(fun = dnorm, args = list(mean = 0, sd = 1))
```

![](Stat_Inference_Part1_files/figure-html/figure_5-1.png) 

[1]: https://en.wikipedia.org/wiki/Exponential_distribution

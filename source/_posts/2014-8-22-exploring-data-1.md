---
title: 机器学习与R(6)-数据探索-1
date: 2014-08-22 19:30:56
modified: 2014-08-22 19:30:56
category: 数据科学
tags: [数据科学,机器学习,R,机器学习与R]
---



探索数据是对数据进行初步研究,以便更好的理解数据，帮助选择合适的数据预处理和数据分析技术。通过数据探索我们将识别原始数据中的问题，缺失值，异常，分布等等。通常我们会考察：

+ 频数，均值，方差，偏度...
+ 对数据可视化，以快速找到数据的特征


## 数据读入


```r
contribution=read.csv("contribution.csv",stringsAsFactors=F)
str(contribution)
```

```
## 'data.frame':	1230 obs. of  11 variables:
##  $ Gender         : chr  "M" "M" "F" "M" ...
##  $ Class.Year     : int  1957 1957 1957 1957 1957 1957 1957 1957 1957 1957 ...
##  $ Marital.Status : chr  "M" "M" "M" "M" ...
##  $ Major          : chr  "History" "Physics" "Music" "History" ...
##  $ Next.Degree    : chr  "LLB" "MS" "NONE" "NONE" ...
##  $ FY04Giving     : num  2500 5000 5000 0 1000 0 0 100 100 0 ...
##  $ FY03Giving     : num  2500 5000 5000 5100 1000 0 0 100 100 0 ...
##  $ FY02Giving     : num  1400 5000 5000 200 1000 0 0 100 100 0 ...
##  $ FY01Giving     : num  12060 5000 5000 200 1005 ...
##  $ FY00Giving     : num  12000 10000 10000 0 1000 0 0 100 100 0 ...
##  $ AttendenceEvent: int  1 1 1 1 1 0 0 0 0 1 ...
```

1230条观察记录，11个变量，其中的文本变量都是`char`类型，没有转换成factor，这里可以根据需求来转换。

# 数字变量探索


```r
summary(contribution$FY00Giving)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##       0       0       0     169      60   21000
```

```r
summary(contribution$FY01Giving)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##       0       0       0     277      75  161000
```

```r
summary(contribution[c("FY00Giving","FY01Giving")])
```

```
##    FY00Giving      FY01Giving    
##  Min.   :    0   Min.   :     0  
##  1st Qu.:    0   1st Qu.:     0  
##  Median :    0   Median :     0  
##  Mean   :  169   Mean   :   277  
##  3rd Qu.:   60   3rd Qu.:    75  
##  Max.   :21000   Max.   :161370
```

`summary`列出了最小，最大，第1，3个四分位值以及mean，median。通过这些数据我们大致对数据的分布区间有了了解。而其它的来自`Hmisc`包的`describe`提供了更详细的数据描述。


```r
describe(contribution$FY01Giving)
```

```
## Error: could not find function "describe"
```

## 数据的中心趋势

对于一组数据的描述，我们常用均值来进行描述，而均值有`mean`和`median`两种，其中`mean`很容易受到异常值的影响(outlier)


```r
x=c(10:20,80,100,120)
mean(x)
```

```
## [1] 33.21
```

```r
median(x)
```

```
## [1] 16.5
```

显然在这是用`median`是更好描述方法，通过`summary`，我们可以同时获得`mean,median`，通过比较二者，我们也可以知道数据的分布情况。比如这里`mean`远大于`median`，虽然数据中大部分点位于10到20，但是少数几个较大的outlier使得mean为33.2，而此时median则为16.5



```r
range(contribution$FY00Giving)
```

```
## [1]     0 21000
```

```r
diff(range(contribution$FY00Giving))
```

```
## [1] 21000
```

利用`range`与`diff`我们可以获得数据的分布区间，而


```r
IQR(contribution$FY00Giving)
```

```
## [1] 60
```

则直接给出中间50%的数据的区间即Q1与Q3间的差。直接使用


```r
quantile(contribution$FY00Giving)
```

```
##    0%   25%   50%   75%  100% 
##     0     0     0    60 21000
```

```r
quantile(contribution$FY00Giving,prob=c(0.75,0.85,0.95))
```

```
## 75% 85% 95% 
##  60 154 350
```

```r
#试试这个
#quantile(contribution$FY00Giving,prob=seq(from=0,to=1,by=0.1))
```

可以给出更详细的值，比如我们可以由此知道85%的人的捐款都少于154$。通过这些数据你应该可以理解前面的`summary(contribution$FY00Giving)`为什么`median`为0了，数据分布的非正态。

title: 机器学习与R(7)-数据探索-2
date: 2014-08-22 19:31:01
category: 数据科学
tags: [数据科学,机器学习,R,机器学习与R]
---


## 盒子图

boxplot是用图形表示Q1,Q3,median,min,max的方法


```r
boxplot(contribution$Class.Year,main="Boxplot for Class Year",ylab="Class Year")
```

![](/img/rmachine/rmachine-7-21.png) 

```r
boxplot(contribution$FY00Giving,main="Boxplot for donation in FY00",ylab="donation $")
```

![](/img/rmachine/rmachine-7-22.png) 

通过图形，很容易我们可以看出给学校捐款的学生大多是85年前毕业的，而大部分人没有捐款，少数人在贡献。

## 直方图

histogram是另一种描述数据分布的方式


```r
hist(contribution$Class.Year)
```

![](/img/rmachine/rmachine-7-31.png) 

```r
hist(contribution$FY00Giving)
```

![](/img/rmachine/rmachine-7-32.png) 

```r
hist(subset(contribution,FY00Giving>0&FY00Giving<500)$FY00Giving)
```

![](/img/rmachine/rmachine-7-33.png) 

从`Class.Year`我们看出数据集中1997毕业的最多，而FY00Giving大部分为0，进一步还看出大部分人捐款都在100以下。同时我们还了解到了数据的偏度(skew)。

偏度：偏度（Skewness）是描述某变量取值分布对称性的统计量。http://www.r-tutor.com/elementary-statistics/numerical-measures/skewness
 
+ Skewness=0 分布形态与正态分布偏度相同
+ Skewness>0 正偏差数值较大，为正偏或右偏。长尾巴拖在右边。
+ Skewness<0 负偏差数值较大，为负偏或左偏。长尾巴拖在左边。 

利用来自`e1071`包的函数`skewness`我们可以验证到Class.Year分布是左偏的，FY00Giving是严重右偏的。


```r
require(e1071)
```

```
## Loading required package: e1071
```

```r
skewness(contribution$Class.Year)
```

```
## [1] -0.3364
```

```r
skewness(contribution$FY00Giving)
```

```
## [1] 13.38
```

## 标准差与方差


```r
sd(contribution$FY00Giving)
```

```
## [1] 1171
```

```r
var(sd(contribution$FY00Giving))
```

```
## [1] NA
```

标准差是描述数据离mean的距离。对正态分布68%的数据在1个标准差内，95%的在2个标准差内，99.7在3个标准差内，这就是我们常说的$/sigma$

## 分类（categorical） 变量

此时我们可以通过频数表来统计

```
table(contribution$Major)
major_table=table(contribution$Major)
prop.table(major_table)
round(prop.table(major_table)*100)
```

`table`统计频数，`prop.table`则可以计算比例。

## CrossTable


```r
require(gmodels)
```

```
## Loading required package: gmodels
```

```
## Warning: there is no package called 'gmodels'
```

```r
?CrossTable
```

```
## No documentation for 'CrossTable' in specified packages and libraries:
## you could try '??CrossTable'
```

`gmodels`包的`CrossTable`函数提供了更丰富的功能，可以通过`?CrossTable`查看

## 散点图



```r
data(trees)
plot(trees$Height,trees$Volume)
```

![](/img/rmachine/rmachine-7-7.png) 


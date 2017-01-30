---
title: 机器学习与R(4)-数据准备
date: 2014-08-16 15:36:40
modified: 2014-08-16 15:36:40
category: 数据科学
tags: [数据科学,机器学习,R,机器学习与R]
---

尽管数据准备与整理没有建立模型有趣，但却是极为重要的一步。因为数据的质量决定了模型质量。而多数情况下，输入数据都很复杂，散乱，或者是来自多个数据源，或者是不同格式。正是由于这一复杂性，机器学习大量的工作时实际在于数据准备与探索阶段。

对于应用R来完成这一任务，则需要我们：

+ 掌握R的基本数据结构，利用它们来存储，抽取数据
+ 利用R从不同数据源获取数据
+ 利用R来理解并将复杂数据可视化

# R数据结构

因为R是大量用于统计数据分析的编程语言，因此R中数据结构的设计也是为了更好的操作这些数据。R中常用数据结构有

+ vectors
+ factors
+ lists
+ arrays
+ data frames

## vectors

vector是R中基础数据结构，实际就是一个有序排列的元素，这些元素既可以是数字，也可以是factor，逻辑值(TRUE/FALSE)，也可以是字符。我们通常用`c()`来建立vector，而赋值则可以使用`=`或者`=`。关于`=`还是`=`的争论好比之前的咸豆浆还是甜豆浆的争论，大家可以看看[大神谢益辉](http://yihui.name/cn/2012/09/equal-and-arrow/)。

以下代码分别建立了三个不同的vector(向量)：

```r
subject_name <- c("John Doe", "Jane Doe", "Steve Graves")
temperature <- c(98.1, 98.6, 101.4)
flu_status <- c(FALSE, FALSE, TRUE)
```

要获取某个向量的值也很简单，用`[]`直接取下标：


```r
temperature[2]
```

```
## [1] 98.6
```

不过要注意**R里面下标都是从1开始，而不是像其他程序从0开始**。

如果我想取某几个值则可以同时包含对应下标


```r
temperature[2:3] #取vector的2，3下标对应值
```

```
## [1]  98.6 101.4
```

```r
temperature[c(1,3)] #取vector的1，3下标对应值
```

```
## [1]  98.1 101.4
```
如果前面加一个`-`就表示，不要包含该值了


```r
temperature[-2]
```

```
## [1]  98.1 101.4
```

```r
temperature[c(-1,-3)]
```

```
## [1] 98.6
```
除了这种方式，采用逻辑变量来取子集也是一样，比如


```r
temperature[c(TRUE, TRUE, FALSE)]
```

```
## [1] 98.1 98.6
```

## Factors

Factors通常用于**nomial**变量，比如性别：male,female。也许你会问为什么不直接用character vector呢？很简单采用fator更高效，计算机会把factor：male,male,female保存为1,1,2，更加高效。另外一个原因就在于机器学习有特殊的处理分类变量的方式。

建立factor相当简单


```r
gender = factor(c("MALE", "FEMALE", "MALE"))
gender
```

```
## [1] MALE   FEMALE MALE  
## Levels: FEMALE MALE
```

上面是采用了缺省顺序，你也可以指明factor的顺序，例如：


```r
blood = factor(c("O", "AB", "A"),levels = c("A", "B", "AB", "O"))
blood
```

```
## [1] O  AB A 
## Levels: A B AB O
```

## Lists

list是一种特殊类型向量，通常向量只能保存相同类型数据，而list则可以保存不同类型数据。正是由于这一灵活性，list通常用来保存输入输出数据或者机器学习中的配置参数。

以前面的数据微冷，如果我们要看某一病人的信息，我们需要执行如下操作：


```r
subject_name[1]
```

```
## [1] "John Doe"
```

```r
temperature[1]
```

```
## [1] 98.1
```

```r
flu_status[1]
```

```
## [1] FALSE
```

```r
gender[1]
```

```
## [1] MALE
## Levels: FEMALE MALE
```

```r
blood[1]
```

```
## [1] O
## Levels: A B AB O
```

实在麻烦，那我可以建立一个list：


```r
subject = list(fullname = subject_name,
  temperature = temperature,
  flu_status = flu_status,
	gender = gender,
	blood = blood)
```

这时输入:


```r
subject
```

```
## $fullname
## [1] "John Doe"     "Jane Doe"     "Steve Graves"
## 
## $temperature
## [1]  98.1  98.6 101.4
## 
## $flu_status
## [1] FALSE FALSE  TRUE
## 
## $gender
## [1] MALE   FEMALE MALE  
## Levels: FEMALE MALE
## 
## $blood
## [1] O  AB A 
## Levels: A B AB O
```

全部信息同时都有了，如果我只想看某个变量那么：


```r
subject[1][1] #代表查看list中第一个元素的第1列变量值
```

```
## $fullname
## [1] "John Doe"     "Jane Doe"     "Steve Graves"
```

```r
subject$temperature #代表查看list中temperature列
```

```
## [1]  98.1  98.6 101.4
```

```r
subject[c("temperature", "flu_status")]  #代表查看list中temperature列以及flu_status列
```

```
## $temperature
## [1]  98.1  98.6 101.4
## 
## $flu_status
## [1] FALSE FALSE  TRUE
```





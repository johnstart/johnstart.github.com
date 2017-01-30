---
title: 机器学习与R(5)-数据准备续
date: 2014-08-16 15:36:54
modified: 2014-08-16 15:36:54
category: 数据科学
tags: [数据科学,机器学习,R,机器学习与R]
---



## Data Frames

data frame应该是R中最常用的数据结构了，你可以把它类比于excel中的表，有行有列。


```r
pt_data <- data.frame(subject_name, temperature, flu_status,
  gender, blood, stringsAsFactors = FALSE)
pt_data
```

```
##   subject_name temperature flu_status gender blood
## 1     John Doe        98.1      FALSE   MALE     O
## 2     Jane Doe        98.6      FALSE FEMALE    AB
## 3 Steve Graves       101.4       TRUE   MALE     A
```

以上代码创建了一个data frame，其中的`stringsAsFactors`是指出不要把字符变量转换为factor（缺省会转换），所以如果你用


```r
str(pt_data)
```

```
## 'data.frame':	3 obs. of  5 variables:
##  $ subject_name: chr  "John Doe" "Jane Doe" "Steve Graves"
##  $ temperature : num  98.1 98.6 101.4
##  $ flu_status  : logi  FALSE FALSE TRUE
##  $ gender      : Factor w/ 2 levels "FEMALE","MALE": 2 1 2
##  $ blood       : Factor w/ 4 levels "A","B","AB","O": 4 3 1
```

查看数据结构，`subject_name`是字符类型。

有了前面基础，你应该很容易理解下面的代码:


```r
pt_data$subject_name
```

```
## [1] "John Doe"     "Jane Doe"     "Steve Graves"
```

```r
pt_data[c("temperature", "flu_status")]
```

```
##   temperature flu_status
## 1        98.1      FALSE
## 2        98.6      FALSE
## 3       101.4       TRUE
```

不过data frame还有些新用法


```r
# 取第1行，第2列
pt_data[1, 2]
```

```
## [1] 98.1
```

```r
# 取第1行
pt_data[1, ]
```

```
##   subject_name temperature flu_status gender blood
## 1     John Doe        98.1      FALSE   MALE     O
```

```r
# 取第1列
pt_data[,1 ]
```

```
## [1] "John Doe"     "Jane Doe"     "Steve Graves"
```

```r
#更复杂的取1，3行的2，4列
pt_data[c(1, 3), c(2, 4)]
```

```
##   temperature gender
## 1        98.1   MALE
## 3       101.4   MALE
```

```r
#取整个data frame
pt_data[ , ]
```

```
##   subject_name temperature flu_status gender blood
## 1     John Doe        98.1      FALSE   MALE     O
## 2     Jane Doe        98.6      FALSE FEMALE    AB
## 3 Steve Graves       101.4       TRUE   MALE     A
```

```r
#这2个等价，想想...
pt_data[c(1, 3), c("temperature", "gender")]
```

```
##   temperature gender
## 1        98.1   MALE
## 3       101.4   MALE
```

```r
pt_data[-2, c(-1, -3, -5)]
```

```
##   temperature gender
## 1        98.1   MALE
## 3       101.4   MALE
```

## matrixes 与 arrays

除了data frame，matrix和array也可以用来保存表格数据，只是要求数据类型相同。比如


```r
m <- matrix(c('a', 'b', 'c', 'd'), nrow = 2)
m
```

```
##      [,1] [,2]
## [1,] "a"  "c" 
## [2,] "b"  "d"
```

这里`nrow`代表行数，matrix缺省按行排列，但你也可以指定：


```r
m <- matrix(c('a', 'b', 'c', 'd'), nrow = 2)
m
```

```
##      [,1] [,2]
## [1,] "a"  "c" 
## [2,] "b"  "d"
```

对比一下：


```r
m <- matrix(c('a', 'b', 'c', 'd'), nrow = 2,byrow=FALSE)
m
```

```
##      [,1] [,2]
## [1,] "a"  "c" 
## [2,] "b"  "d"
```

类似还有


```r
m <- matrix(c('a', 'b', 'c', 'd', 'e', 'f'), ncol = 2)
m
```

```
##      [,1] [,2]
## [1,] "a"  "d" 
## [2,] "b"  "e" 
## [3,] "c"  "f"
```

```r
m[1, ]
```

```
## [1] "a" "d"
```

```r
m[, 1]
```

```
## [1] "a" "b" "c"
```

Array与matrix类似，只是可以有更多维。

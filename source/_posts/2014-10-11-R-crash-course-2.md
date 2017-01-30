title: R Your Ready? 100分钟R入门教程(下)
date: 2014-10-11 22:28:30
modified: 2014-10-11 22:28:30
category: 数据科学
tags: [数据科学,R,R培训]
---

# Read in data

## 工作环境相关


```r
ls()                 # 列出已创建变量
remove( object.name )           # 删除变量

dir()                           
getwd() 
setwd("d:/project/datascience/rtraining/") 
```

## read.table

read.table是R里面常用的读取ASCII文件数据的方式，通过制定参数sep,header可以对读入方式进行更细致的控制。

`?read.table`


```r
setwd("d:/project/datascience/rtraining/")
test.txt <- read.table("./data/test.txt", header=T)
print(test.txt)
```

```
##    make   model mpg weight price
## 1   AMC Concord  22   2930  4099
## 2   AMC   Pacer  17   3350  4749
## 3   AMC  Spirit  22   2640  3799
## 4 Buick Century  20   3250  4816
## 5 Buick Electra  15   4080  7827
```

## read.csv

read.csv是read.table的特列，专门针对csv文件格式


```r
test.csv <- read.csv("./data/test.csv", header=T)
test.csv
```

```
##    make   model mpg weight price
## 1   amc concord  22   2930  4099
## 2   amc   oacer  17   3350  4749
## 3   amc  spirit  22   2640  3799
## 4 buick century  20   3250  4816
## 5 buick electra  15   4080  7827
```

```r
test.csv1 <- read.table("./data/test.csv", header=T, sep=",")
test.csv1
```

```
##    make   model mpg weight price
## 1   amc concord  22   2930  4099
## 2   amc   oacer  17   3350  4749
## 3   amc  spirit  22   2640  3799
## 4 buick century  20   3250  4816
## 5 buick electra  15   4080  7827
```

## read.table读入其它格式文件


```r
test.semi = read.table("./data/testsemicolon.txt", header=T, sep=";")
test.semi
```

```
##    make   model mpg weight price
## 1   AMC Concord  22   2930  4099
## 2   AMC   Pacer  17   3350  4749
## 3   AMC  Spirit  22   2640  3799
## 4 Buick Century  20   3250  4816
## 5 Buick Electra  15   4080  7827
```

```r
test.z = read.table("./data/testz.txt", header=T, sep="z")
test.z
```

```
##    make   model mpg weight price
## 1   AMC Concord  22   2930  4099
## 2   AMC   Pacer  17   3350  4749
## 3   AMC  Spirit  22   2640  3799
## 4 Buick Century  20   3250  4816
## 5 Buick Electra  15   4080  7827
```

## scan

scan是极为灵活的输入数据方式，可以读入几乎任何类型数据，数字，字符...。同时通过scan你可以直接从控制台输入。缺省条件下scan的参数what接收数字，制定what=""接受字符。不过需要注意与read.table不同，scan返回的是向量。


```r
x=scan()
x
```

```
## numeric(0)
```

```r
name.x=scan(,what="")
name.x
```

```
## character(0)
```

```r
x = scan("./data/scan.txt", what=list(age=0, name=""))
x
```

```
## $age
## [1] 12 24 35 20
## 
## $name
## [1] "bobby"   "kate"    "david"   "michael"
```

## read.fwf

read.fwf读入固定宽度格式保存的文件。


```r
names = scan("./data/names.txt", what=character() )
names
```

```
## [1] "model"  "make"   "mph"    "weight" "price"
```

```r
test.fixed = read.fwf("./data/testfixed.txt", 
                      col.names=names, width = c(5, 7, 2, 4, 4))
```

```
## Warning: incomplete final line found on './data/testfixed.txt'
```

```r
# 有一个警告出来是什么原因呢？猜猜
test.fixed
```

```
##   model    make mph weight price
## 1 AMC   Concord  22   2930  4099
## 2 AMC   Pacer    17   3350  4749
## 3 AMC   Spirit   22   2640  3799
## 4 Buick Century  20   3250  4816
## 5 Buick Electra  15   4080  7827
```

## 内部数据


```r
data( USArrests )
```

## 读取远程数据

前面的read系列函数也支持读取远程数据


```r
pew_data = read.csv("http://bit.ly/11I3iuU")
```

```
## Warning: cannot open: HTTP status was '0 (nil)'
```

```
## Error: cannot open the connection
```

```r
pew_data = read.csv("http://www.pewinternet.org/files/old-media/Files/Data%20Sets/2013/Omnibus_Jan_2013_csv.csv")
head(pew_data)
```

```
##   psraid sample state cregion usr pial1a pial1b pial1c pial1d pial1e pial2
## 1 100002      1    18       2   S      2      2      2      2      2    NA
## 2 100005      1    18       2   U      1      2      1      1      2     2
## 3 100008      1    39       2   R      1      2      2      2      1     2
## 4 100010      1    24       3   S      1      2      2      1      2     1
## 5 100012      1    42       1   S      1      2      2      2      2     2
## 6 100014      1    36       1   S      2      2      2      2      2    NA
##   pial3a pial3b pial3c pial4 employ par sex age educ2 hisp race inc weight
## 1     NA     NA     NA    NA      3   2   1  62     4    2    1   2  1.463
## 2      2      2      2    NA      3   2   2  85     8    2    2   1  1.122
## 3      2      2      2    NA      1   1   1  41     3    2    1   4  2.268
## 4      2      2      2    NA      3   2   1  19     3    2    1   7  2.415
## 5      2      2      2    NA      3   2   2  84     2    2    2   2  1.683
## 6     NA     NA     NA    NA      3   2   2  84     4    2    1   7  1.976
##   standwt
## 1  0.4879
## 2  0.3741
## 3  0.7563
## 4  0.8051
## 5  0.5611
## 6  0.6587
```

```r
#http://www.pewinternet.org/files/old-media/Files/Data%20Sets/2013/Omnibus_Jan_2013_csv.csv
```

## 其它格式数据

与老外通话肯定得讲外语了，所以我们需要`foreign`包


```r
library(foreign)
help(package=foreign)
```

data.restore   Read an S3 Binary File  
lookup.xport   Lookup Information on a SAS XPORT Format Library  
read.dbf       Read a DBF File  
read.dta       Read Stata binary files  
read.epiinfo   Read Epi Info data files  
read.mtp       Read a Minitab Portable Worksheet  
read.octave    Read Octave Text Data Files  
read.spss      Read an SPSS data file  
read.ssd       Obtain a Data Frame from a SAS Permanent Dataset, via read.xport  
read.systat    Obtain a Data Frame from a Systat File  
read.xport     Read a SAS XPORT Format Library  
write.dbf      Write a DBF File  
write.dta      Write Files in Stata Binary Format  
write.foreign  Write text files and code to read them  

## 访问数据库

[RPostgreSQL](http://cran.r-project.org/web/packages/RPostgreSQL/index.html)  
[RMySQL](http://cran.r-project.org/web/packages/RMySQL/index.html)  
[RMongo](http://cran.r-project.org/web/packages/RMongo/index.html)  
[RSQLite](http://cran.r-project.org/web/packages/RSQLite/index.html)  
[RODBC](http://cran.r-project.org/web/packages/RODBC/index.html)  

最后重点推出

[sqldf](http://cran.r-project.org/web/packages/RODBC/index.html)


## 数据保存与加载


```r
save( the.data, file="thedata.rda" )
load( file = "thedata.rda" )    # rda == R Data
write.table( USArrests, file = "arrests.txt", sep = "," )
```


# R的绘图功能

R 中的绘图系统:

- *base* graphics
- *lattice* graphics
- *ggplot2* 


```r
earnings = read.table("./data/heights.csv",sep=" ")
names(earnings)
```

```
## [1] "earn"    "height1" "height2" "sex"     "race"    "hisp"    "ed"     
## [8] "yearbn"  "height"
```

```r
hist(earnings$earn)
```

![](/images/project/training/crashcourse/basic_plots1.png) 

```r
plot(earnings$earn ~ earnings$height)
```

![](/images/project/training/crashcourse/basic_plots2.png) 

```r
boxplot(earnings$earn ~ earnings$height)
```

![](/images/project/training/crashcourse/basic_plots3.png) 

```r
boxplot(earnings$earn ~ earnings$sex)
```

![](/images/project/training/crashcourse/basic_plots4.png) 


# 赐予我力量吧-`packages`

- `packages` 是R的动力源泉
- > 5,000 packages ，提供各种功能
- 一些例子
  * `ggplot2` package
  * 高级回归包`lme4`
  * 3d 图形 `scatterplot3d` package
  * GIS 分析与地图 `sp`
  * 文本挖掘 `tm`
  * 预测建模 `caret`
  * 与 Python, Java,  C , C++的接口
  * 服务器版本: `Rserve`
  * 甚至扫雷都有- `fun` package
  

## 如何获取


```r
install.packages('foo') # Name must be in quotes
install.packages(c('foo','foo1','foo2'))
install.packages(file_name_and_path, repos = NULL, type="source")
# Packages 更新
update.packages() # update all
installed.packages()
remove.packages()
available.packages()
old.packages()
require("ggplot2")
library()
```

## 查找 Packages

- [packages on CRAN](http://www.cran.r-project.org/web/packages/). 
- [CRAN Task Views](http://www.cran.r-project.org/web/views/).
- 非官方网站 [RForge](http://r-forge.r-project.org/) , [GitHub](http://www.github.com)

## 利用某包完成某功能

- 放狗google
  * Google "doing X in R package"
  * 或者CRAN taskviews
- [CRAN taskviews](http://cran.r-project.org/web/views/) 

## 一些常用包

`plyr` `ggplot2` `caret` `rattle` `lme4` `reshape2` `knitr` `dplyr`

- 包里面包含了函数，代码，数据
- `help(package="ggplot2")`
- `vignettes` 


## 使用包


1. 安装
2. 加载

RStudio, `Packages->Install Packages`.


```r
install.packages('fields')
#指定目录
install.packages('fields', lib = '~/R')
```

##


```r
library(help="ggplot2")
help(package="ggplot2")
```


# R编程

## `if`-`else` 


```r
val = rnorm(1)
val
```

```
## [1] 0.6169
```

```r
if (val < 0) {
  "val is negative!"
} else {
  "val is positive"
}
```

```
## [1] "val is positive"
```

```r
x=rnorm(1)
y = if ( x < 0 ) -1 else 0 
y
```

```
## [1] -1
```

```r
x
```

```
## [1] -0.681
```

## 嵌套if-else


```r
# multiline if - else statement 
if ( x < 0 ) { 
    x = x+1
    print("Add one") 
} else if ( x == 0 ) {
    print("Zero")
} else {
    print("Positive value")
}
```

```
## [1] "Add one"
```

## ifelse

ifelse(test_condition, true_value, false_value)



```r
y = ifelse ( x < 0, -1, 0 )
y
```

```
## [1] 0
```

```r
# nested ifelse statement 
y = ifelse ( x < 0, -1, ifelse (x > 0, 1, 0) )

# ifelse statement on a vector
digits = 0 : 9
(odd = ifelse( digits %% 2 > 0, TRUE, FALSE ))
```

```
##  [1] FALSE  TRUE FALSE  TRUE FALSE  TRUE FALSE  TRUE FALSE  TRUE
```

## 练习

定义一个向量，包含的10个数，数值在-10～10间均匀分布
  `x= as.integer( runif( 10, -10, 10 ))`

利用ifelse 来求x的绝对值。（通常用abs()可以解决）

##


```r
x= as.integer( runif( 10, -10, 10 ))
ifelse(x<0,-x,x)
```

```
##  [1] 0 6 7 2 4 7 9 4 6 9
```

## 循环

+ 重复执行
+ 避免使用循环

`for,repeat,while`


## `for` 

抽象表达：


```r
for (variable in sequence) {
  statement
}
```

## `for`


```r
myseq = seq(5,20,by=5)
for (i in myseq) {
  print(i)
}
```

```
## [1] 5
## [1] 10
## [1] 15
## [1] 20
```


```r
for (i in seq(2,8, by=2)) {
  print(i)
}
```

```
## [1] 2
## [1] 4
## [1] 6
## [1] 8
```

## `while` 

抽象表达：


```r
while (condition) {
  statements
}
```

## `while`实现


```r
i = 0
while (i < 10) {
  i = i + 1
  print(i)
}
```

```
## [1] 1
## [1] 2
## [1] 3
## [1] 4
## [1] 5
## [1] 6
## [1] 7
## [1] 8
## [1] 9
## [1] 10
```

注意不要出现无限循环！

## Fibonacci 


```r
myseq[1] = 0
myseq[2] = 1
for (i in seq(3,12)) {
  myseq[i] = myseq[i-2] + myseq[i-1]
}
myseq
```

```
##  [1]  0  1  1  2  3  5  8 13 21 34 55 89
```



```r
myseq[1] = 0
myseq[2] = 1
i = 2
currentVal = 1
while (currentVal < 500) {
  myseq[i+1] = currentVal
  currentVal = myseq[i] + myseq[i+1]
  i = i+1
}
myseq
```

```
##  [1]   0   1   1   2   3   5   8  13  21  34  55  89 144 233 377
```

## `next` 与 `break` 

`next` 跳过当前的循环语句


```r
for (i in seq(1,10)) {
  if (i == 5) {
    next
  }
  print(i)
}
```

```
## [1] 1
## [1] 2
## [1] 3
## [1] 4
## [1] 6
## [1] 7
## [1] 8
## [1] 9
## [1] 10
```

`break` 直接跳出循环：


```r
val = 0
i = 0
while(TRUE) {
  i = i + 1
  val = val + rnorm(1)
  if (abs(val) > 3) {
    break
  }
}
val
```

```
## [1] -3.359
```

```r
i
```

```
## [1] 4
```

## 循环通常不是最佳方案



```r
vals = rnorm(100)
maxVal = vals[1]
for (val in vals) {
  if (val > maxVal) {
    maxVal = val
  }
}
maxVal
```

```
## [1] 3.047
```

使用内部函数：


```r
max(vals)
```

```
## [1] 3.047
```

向量化运算而不是循环:


```r
myseq = seq(2,20, by =2)
for(i in seq(1,10)) {
  myseq[i] = myseq[i] + 2
}
myseq
```

```
##  [1]  4  6  8 10 12 14 16 18 20 22
```

```r
seq(2,20, by =2)+2
```

```
##  [1]  4  6  8 10 12 14 16 18 20 22
```

## apply

apply(), sapply(), tapply(), lapply()

## 函数

函数: **arguments** 输入，**return** 输出

为什么要函数：

1. 重用
2. 清晰


## invisible return


```r
histNormal = function(N) {
  vals = rnorm(N)
  hist(vals)
  invisible(max(vals))
}

histNormal(1000)
```

![](/images/project/training/crashcourse/unnamed-chunk-741.png) 

```r
max = histNormal(1000)
```

![](/images/project/training/crashcourse/unnamed-chunk-742.png) 

```r
max
```

```
## [1] 2.989
```

## 多参数以及缺省值User-defined functions V: multiple arguments, default values


```r
newFunction = function(num, threshold=0, modifier=2) {
  if (num < threshold) {
    return(num / modifier)
  } else {
    return(num * modifier)
  }
}
newFunction(2.6)
```

```
## [1] 5.2
```

R缺省从左到有匹配参数:


```r
newFunction(2.6, 3)
```

```
## [1] 1.3
```

```r
newFunction(2.6, 3, 1.3)
```

```
## [1] 2
```

当然你可以明确指定


```r
newFunction(2.6, modifier=1.3, threshold=3)
```

```
## [1] 2
```



```r
hist(sapply(rnorm(10000), newFunction), breaks=60, freq=FALSE)
```

![](/images/project/training/crashcourse/unnamed-chunk-781.png) 

```r
hist(sapply(rnorm(10000), newFunction, modifier=1), breaks=60, freq=FALSE)
```

![](/images/project/training/crashcourse/unnamed-chunk-782.png) 

## `...` 


```r
args(hist)
```

```
## function (x, ...) 
## NULL
```

```r
histNormalWrapper = function(N, ...) {
  vals = rnorm(N)
  hist(vals, ...)
}

histNormalWrapper(1000)
```

![](/images/project/training/crashcourse/unnamed-chunk-791.png) 

```r
histNormalWrapper(1000, breaks=50)
```

![](/images/project/training/crashcourse/unnamed-chunk-792.png) 

# table


```r
data( UCBAdmissions )
ls()
```

```
##  [1] "bins"              "colors"            "currentVal"       
##  [4] "digits"            "earnings"          "f"                
##  [7] "freqs"             "hair.color"        "histNormal"       
## [10] "histNormalWrapper" "i"                 "max"              
## [13] "maxVal"            "my.data"           "myfac"            
## [16] "myfac_o"           "mylist1"           "mylist2"          
## [19] "mylist3"           "myseq"             "name.x"           
## [22] "names"             "newFunction"       "odd"              
## [25] "pew_data"          "q"                 "SO"               
## [28] "test.csv"          "test.csv1"         "test.fixed"       
## [31] "test.semi"         "test.txt"          "test.z"           
## [34] "the.data"          "theDF"             "UCBAdmissions"    
## [37] "USArrests"         "val"               "vals"             
## [40] "x"                 "X"                 "y"
```

```r
UCBAdmissions
```

```
## , , Dept = A
## 
##           Gender
## Admit      Male Female
##   Admitted  512     89
##   Rejected  313     19
## 
## , , Dept = B
## 
##           Gender
## Admit      Male Female
##   Admitted  353     17
##   Rejected  207      8
## 
## , , Dept = C
## 
##           Gender
## Admit      Male Female
##   Admitted  120    202
##   Rejected  205    391
## 
## , , Dept = D
## 
##           Gender
## Admit      Male Female
##   Admitted  138    131
##   Rejected  279    244
## 
## , , Dept = E
## 
##           Gender
## Admit      Male Female
##   Admitted   53     94
##   Rejected  138    299
## 
## , , Dept = F
## 
##           Gender
## Admit      Male Female
##   Admitted   22     24
##   Rejected  351    317
```

一个三维数组，伯克利6个不同系的男女录取情况。


```r
ftable( UCBAdmissions )
```

```
##                 Dept   A   B   C   D   E   F
## Admit    Gender                             
## Admitted Male        512 353 120 138  53  22
##          Female       89  17 202 131  94  24
## Rejected Male        313 207 205 279 138 351
##          Female       19   8 391 244 299 317
```

转换成data frame


```r
UCB = as.data.frame( UCBAdmissions )
UCB
```

```
##       Admit Gender Dept Freq
## 1  Admitted   Male    A  512
## 2  Rejected   Male    A  313
## 3  Admitted Female    A   89
## 4  Rejected Female    A   19
## 5  Admitted   Male    B  353
## 6  Rejected   Male    B  207
## 7  Admitted Female    B   17
## 8  Rejected Female    B    8
## 9  Admitted   Male    C  120
## 10 Rejected   Male    C  205
## 11 Admitted Female    C  202
## 12 Rejected Female    C  391
## 13 Admitted   Male    D  138
## 14 Rejected   Male    D  279
## 15 Admitted Female    D  131
## 16 Rejected Female    D  244
## 17 Admitted   Male    E   53
## 18 Rejected   Male    E  138
## 19 Admitted Female    E   94
## 20 Rejected Female    E  299
## 21 Admitted   Male    F   22
## 22 Rejected   Male    F  351
## 23 Admitted Female    F   24
## 24 Rejected Female    F  317
```

## 2-way tables



```r
library( MASS )
data( caith )
mode(caith)  #似乎是个list
```

```
## [1] "list"
```

```r
caith = as.matrix( caith )      # 变成一个矩阵

# 加个row,colname
dimnames(caith) = list(
        Eyes = c( "blue", "light", "medium", "dark" ),
        Hair = c( "fair", "red", "medium", "dark", "black" ))
caith
```

```
##         Hair
## Eyes     fair red medium dark black
##   blue    326  38    241  110     3
##   light   688 116    584  188     4
##   medium  343  84    909  412    26
##   dark     98  48    403  681    85
```

```r
# addmargin
addmargins( caith )
```

```
##         Hair
## Eyes     fair red medium dark black  Sum
##   blue    326  38    241  110     3  718
##   light   688 116    584  188     4 1580
##   medium  343  84    909  412    26 1774
##   dark     98  48    403  681    85 1315
##   Sum    1455 286   2137 1391   118 5387
```

```r
margin.table( caith )           # 全部汇总
```

```
## [1] 5387
```

```r
margin.table( caith, 1 )        # the row marginal sums (index 1)
```

```
## Eyes
##   blue  light medium   dark 
##    718   1580   1774   1315
```

```r
margin.table( caith, 2 )        # column marginal sums (index 2)
```

```
## Hair
##   fair    red medium   dark  black 
##   1455    286   2137   1391    118
```

```r
#比例，行用1，列用2
prop.table( caith, 1 )
```

```
##         Hair
## Eyes        fair     red medium   dark    black
##   blue   0.45404 0.05292 0.3357 0.1532 0.004178
##   light  0.43544 0.07342 0.3696 0.1190 0.002532
##   medium 0.19335 0.04735 0.5124 0.2322 0.014656
##   dark   0.07452 0.03650 0.3065 0.5179 0.064639
```

```r
prop.table( caith, 1 ) * 100
```

```
##         Hair
## Eyes       fair   red medium  dark  black
##   blue   45.404 5.292  33.57 15.32 0.4178
##   light  43.544 7.342  36.96 11.90 0.2532
##   medium 19.335 4.735  51.24 23.22 1.4656
##   dark    7.452 3.650  30.65 51.79 6.4639
```

```r
#可视化
mosaicplot( caith, shade=TRUE )     
```

![nk-83](/images/project/training/crashcourse/unnamed-chunk-831.png) 

```r
barplot( caith, beside=TRUE, legend=TRUE )     # not shown
```

![nk-83](/images/project/training/crashcourse/unnamed-chunk-832.png) 

##  Multiway tables


```r
HairEyeColor                              #contingency table
```

```
## , , Sex = Male
## 
##        Eye
## Hair    Brown Blue Hazel Green
##   Black    32   11    10     3
##   Brown    53   50    25    15
##   Red      10   10     7     7
##   Blond     3   30     5     8
## 
## , , Sex = Female
## 
##        Eye
## Hair    Brown Blue Hazel Green
##   Black    36    9     5     2
##   Brown    66   34    29    14
##   Red      16    7     7     7
##   Blond     4   64     5     8
```

```r
as.data.frame( HairEyeColor )             # dataframe 
```

```
##     Hair   Eye    Sex Freq
## 1  Black Brown   Male   32
## 2  Brown Brown   Male   53
## 3    Red Brown   Male   10
## 4  Blond Brown   Male    3
## 5  Black  Blue   Male   11
## 6  Brown  Blue   Male   50
## 7    Red  Blue   Male   10
## 8  Blond  Blue   Male   30
## 9  Black Hazel   Male   10
## 10 Brown Hazel   Male   25
## 11   Red Hazel   Male    7
## 12 Blond Hazel   Male    5
## 13 Black Green   Male    3
## 14 Brown Green   Male   15
## 15   Red Green   Male    7
## 16 Blond Green   Male    8
## 17 Black Brown Female   36
## 18 Brown Brown Female   66
## 19   Red Brown Female   16
## 20 Blond Brown Female    4
## 21 Black  Blue Female    9
## 22 Brown  Blue Female   34
## 23   Red  Blue Female    7
## 24 Blond  Blue Female   64
## 25 Black Hazel Female    5
## 26 Brown Hazel Female   29
## 27   Red Hazel Female    7
## 28 Blond Hazel Female    5
## 29 Black Green Female    2
## 30 Brown Green Female   14
## 31   Red Green Female    7
## 32 Blond Green Female    8
```

```r
ftable( HairEyeColor )                    # flat contingency table
```

```
##             Sex Male Female
## Hair  Eye                  
## Black Brown       32     36
##       Blue        11      9
##       Hazel       10      5
##       Green        3      2
## Brown Brown       53     66
##       Blue        50     34
##       Hazel       25     29
##       Green       15     14
## Red   Brown       10     16
##       Blue        10      7
##       Hazel        7      7
##       Green        7      7
## Blond Brown        3      4
##       Blue        30     64
##       Hazel        5      5
##       Green        8      8
```

```r
ftable( HairEyeColor, col.vars="Hair" )   # 换一个不同列变量
```

```
##              Hair Black Brown Red Blond
## Eye   Sex                              
## Brown Male           32    53  10     3
##       Female         36    66  16     4
## Blue  Male           11    50  10    30
##       Female          9    34   7    64
## Hazel Male           10    25   7     5
##       Female          5    29   7     5
## Green Male            3    15   7     8
##       Female          2    14   7     8
```

```r
summary( HairEyeColor )                   # this works only for 3-way and up
```

```
## Number of cases in table: 592 
## Number of factors: 3 
## Test for independence of all factors:
## 	Chisq = 165, df = 24, p-value = 5e-23
## 	Chi-squared approximation may be incorrect
```

```r
hec = as.data.frame( HairEyeColor )
head( hec )                                # first six rows
```

```
##    Hair   Eye  Sex Freq
## 1 Black Brown Male   32
## 2 Brown Brown Male   53
## 3   Red Brown Male   10
## 4 Blond Brown Male    3
## 5 Black  Blue Male   11
## 6 Brown  Blue Male   50
```

Hair-行，Eye-列，统计频数


```r
xtabs( Freq ~ Hair + Eye, data=hec )
```

```
##        Eye
## Hair    Brown Blue Hazel Green
##   Black    68   20    15     5
##   Brown   119   84    54    29
##   Red      26   17    14    14
##   Blond     7   94    10    16
```

只去"Male"


```r
xtabs( Freq ~ Hair + Eye, data=hec, subset=Sex=="Male" )
```

```
##        Eye
## Hair    Brown Blue Hazel Green
##   Black    32   11    10     3
##   Brown    53   50    25    15
##   Red      10   10     7     7
##   Blond     3   30     5     8
```
   
更多列的统计


```r
ti = as.data.frame( Titanic )
head( ti )
```

```
##   Class    Sex   Age Survived Freq
## 1   1st   Male Child       No    0
## 2   2nd   Male Child       No    0
## 3   3rd   Male Child       No   35
## 4  Crew   Male Child       No    0
## 5   1st Female Child       No    0
## 6   2nd Female Child       No    0
```

```r
xtabs( Freq ~ Class + Age + Sex, data=ti )
```

```
## , , Sex = Male
## 
##       Age
## Class  Child Adult
##   1st      5   175
##   2nd     11   168
##   3rd     48   462
##   Crew     0   862
## 
## , , Sex = Female
## 
##       Age
## Class  Child Adult
##   1st      1   144
##   2nd     13    93
##   3rd     31   165
##   Crew     0    23
```


```r
HEC.Male = xtabs( Freq ~ Hair + Eye, data=hec, subset=Sex=="Male" )

prop.table( HEC.Male, 1 ) * 100
```

```
##        Eye
## Hair     Brown   Blue  Hazel  Green
##   Black 57.143 19.643 17.857  5.357
##   Brown 37.063 34.965 17.483 10.490
##   Red   29.412 29.412 20.588 20.588
##   Blond  6.522 65.217 10.870 17.391
```

```r
results=chisq.test( HEC.Male ) 
```

```
## Warning: Chi-squared approximation may be incorrect
```

```r
results
```

```
## 
## 	Pearson's Chi-squared test
## 
## data:  HEC.Male
## X-squared = 41.28, df = 9, p-value = 4.447e-06
```

```r
results$expected
```

```
##        Eye
## Hair    Brown  Blue  Hazel  Green
##   Black 19.67 20.27  9.434  6.624
##   Brown 50.23 51.77 24.090 16.914
##   Red   11.94 12.31  5.728  4.022
##   Blond 16.16 16.65  7.749  5.441
```

```r
results$residuals
```

```
##        Eye
## Hair      Brown    Blue   Hazel   Green
##   Black  2.7800 -2.0594  0.1844 -1.4080
##   Brown  0.3909 -0.2456  0.1855 -0.4654
##   Red   -0.5621 -0.6579  0.5317  1.4853
##   Blond -3.2733  3.2709 -0.9876  1.0971
```

# R之统计假设

# Single sample t-test.


```r
the.data
```

```
##  [1] 7 7 7 5 5 4 4 4 4 4 4 4 3 3 3 3 3 2 1
```

```r
# 双尾
t.test( the.data, mu=5 )        # H0 says mu=5, two-tailed
```

```
## 
## 	One Sample t-test
## 
## data:  the.data
## t = -2.557, df = 18, p-value = 0.01981
## alternative hypothesis: true mean is not equal to 5
## 95 percent confidence interval:
##  3.274 4.831
## sample estimates:
## mean of x 
##     4.053
```

```r
# 单侧 greater    
t.test( the.data, mu=5, alternative="greater" )
```

```
## 
## 	One Sample t-test
## 
## data:  the.data
## t = -2.557, df = 18, p-value = 0.9901
## alternative hypothesis: true mean is greater than 5
## 95 percent confidence interval:
##  3.41  Inf
## sample estimates:
## mean of x 
##     4.053
```

```r
# less
t.test( the.data, mu=5, alternative="less" )
```

```
## 
## 	One Sample t-test
## 
## data:  the.data
## t = -2.557, df = 18, p-value = 0.009904
## alternative hypothesis: true mean is less than 5
## 95 percent confidence interval:
##   -Inf 4.695
## sample estimates:
## mean of x 
##     4.053
```

## 独立 t-test.


```r
x1 = c( 18, 25, 17, 20, 23 )
x2 = c( 20, 30, 22, 25, 28, 30 )

#pooled varianc
t.test( x1, x2, var.equal=TRUE )
```

```
## 
## 	Two Sample t-test
## 
## data:  x1 and x2
## t = -2.24, df = 9, p-value = 0.05188
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -10.51953   0.05287
## sample estimates:
## mean of x mean of y 
##     20.60     25.83
```

```r
# unpooled variance(缺省)

t.test( x1, x2 )
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  x1 and x2
## t = -2.29, df = 8.995, p-value = 0.04776
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -10.40273  -0.06393
## sample estimates:
## mean of x mean of y 
##     20.60     25.83
```

```r
# one-tailed (R 里面是 group 1 - group 2)

t.test( x1, x2, var.equal=T, alternative="less" )
```

```
## 
## 	Two Sample t-test
## 
## data:  x1 and x2
## t = -2.24, df = 9, p-value = 0.02594
## alternative hypothesis: true difference in means is less than 0
## 95 percent confidence interval:
##     -Inf -0.9497
## sample estimates:
## mean of x mean of y 
##     20.60     25.83
```

```r
# 实际场景

all.scores = c( x1, x2 )             # combines x1 and x2 into one vector
all.scores
```

```
##  [1] 18 25 17 20 23 20 30 22 25 28 30
```

```r
grp = c( "x1", "x2" )                # the group names
n = c( 5, 6 )                        # the groups sizes
groups = rep( grp, n )               # create the labels
groups
```

```
##  [1] "x1" "x1" "x1" "x1" "x1" "x2" "x2" "x2" "x2" "x2" "x2"
```

```r
t.test( all.scores ~ groups )
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  all.scores by groups
## t = -2.29, df = 8.995, p-value = 0.04776
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -10.40273  -0.06393
## sample estimates:
## mean in group x1 mean in group x2 
##            20.60            25.83
```


title: R Your Ready? 100分钟R入门教程(上)
date: 2014-10-11 22:28:30
category: 数据科学
tags: [数据科学,R,R培训]
---

# 内容

+ 安装
+ R使用初步
+ vector,list,data frame
+ read data
+ packages
+ R编程
+ table (选讲)
+ t检验(选讲)

# 安装下载

Windows下分别安装：
+ [R](http://www.r-project.org)  
+ [Rstudio](http://www.rstudio.com)  

Linux下安装:

`sudo apt-get install r-base`

# R 初体验

## 输入命令

- R命令大小写敏感
- R的命令实际就是一个个的函数  

` function.name( arguments, options )`

`quit()  #退出R`

`rm( myData )  #删除变量myData`
- #代表注释

## 简单数据录入

简单的小数据集通常可以用 c() 函数输入来获得一个数据向量


```r
my.data = c( 22, 38, 12, 23, 29, 18, 16, 24 )
my.data                         # 隐式调用print(my.data)
```

```
## [1] 22 38 12 23 29 18 16 24
```

修改向量中的值my.data[i]，注意R里面的索引是从1开始


```r
my.data[3] = 19
my.data
```

```
## [1] 22 38 19 23 29 18 16 24
```


## 理解R控制台里面的输出

R不仅打印变量的值，同时还会输出索引.例如系统数据rivers给出了北美141条主要河流的长度（miles）


```r
rivers
```

```
##   [1]  735  320  325  392  524  450 1459  135  465  600  330  336  280  315
##  [15]  870  906  202  329  290 1000  600  505 1450  840 1243  890  350  407
##  [29]  286  280  525  720  390  250  327  230  265  850  210  630  260  230
##  [43]  360  730  600  306  390  420  291  710  340  217  281  352  259  250
##  [57]  470  680  570  350  300  560  900  625  332 2348 1171 3710 2315 2533
##  [71]  780  280  410  460  260  255  431  350  760  618  338  981 1306  500
##  [85]  696  605  250  411 1054  735  233  435  490  310  460  383  375 1270
##  [99]  545  445 1885  380  300  380  377  425  276  210  800  420  350  360
## [113]  538 1100 1205  314  237  610  360  540 1038  424  310  300  444  301
## [127]  268  620  215  652  900  525  246  360  529  500  720  270  430  671
## [141] 1770
```

除了用以下方式访问


```r
rivers[121]
```

```
## [1] 1038
```

还可以用类似python里面的slice的方式访问


```r
rivers[ 10:20 ]
```

```
##  [1]  600  330  336  280  315  870  906  202  329  290 1000
```

```r
rivers[1:5]
```

```
## [1] 735 320 325 392 524
```
 
## 建立一个频数表


假设我们想输入这样的一个数据：

      X     f
    -----------
      7     3
      6     0
      5     2
      4     7
      3     5
      2     1
      1     1


```r
X = 7:1   # X = c( 7, 6, 5, 4, 3, 2, 1 )
f = c( 3, 0, 2, 7, 5, 1, 1 )
the.data = rep( X, f )          # rep() == repeat function
the.data
```

```
##  [1] 7 7 7 5 5 4 4 4 4 4 4 4 3 3 3 3 3 2 1
```


## 对频数表进行统计


```r
table( the.data ) 
```

```
## the.data
## 1 2 3 4 5 7 
## 1 1 5 7 2 3
```

```r
the.data
```

```
##  [1] 7 7 7 5 5 4 4 4 4 4 4 4 3 3 3 3 3 2 1
```

这里的输出第一行是代表了X列，而第二行则代表了f列。

## 对数字变量分组



```r
my.data=c(1:10,15:19,24:30)
my.data
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10 15 16 17 18 19 24 25 26 27 28 29 30
```

```r
bins = seq( from=10, to=30, by=5)
# bins = c( 10, 15, 20, 25, 30 ) 
table( cut( my.data, bins ))
```

```
## 
## (10,15] (15,20] (20,25] (25,30] 
##       1       4       2       5
```

`cut`缺省不包含左侧，包含右侧



```r
bins = c(10, 15, 20, 25, 30)
temp.table = cut( my.data, bins, right=FALSE )
temp.table
```

```
##  [1] <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>   
##  [9] <NA>    [10,15) [15,20) [15,20) [15,20) [15,20) [15,20) [20,25)
## [17] [25,30) [25,30) [25,30) [25,30) [25,30) <NA>   
## Levels: [10,15) [15,20) [20,25) [25,30)
```

```r
table(temp.table)
```

```
## temp.table
## [10,15) [15,20) [20,25) [25,30) 
##       1       5       1       5
```

## 一维向量的茎叶图表示


```r
stem( my.data ) 
```

```
## 
##   The decimal point is 1 digit(s) to the right of the |
## 
##   0 | 123456789
##   1 | 056789
##   2 | 456789
##   3 | 0
```
  
## 直方图


```r
hist( the.data )
```

![](/img/project/training/crashcourse/unnamed-chunk-11.png) 

```r
the.data
```

```
##  [1] 7 7 7 5 5 4 4 4 4 4 4 4 3 3 3 3 3 2 1
```

R的直方图缺省是以包含右侧为界，所以这里1，2是归入1～2


```r
hist( the.data, right=F ) 
```

![](/img/project/training/crashcourse/unnamed-chunk-12.png) 

这个更清楚：


```r
bins = seq( .5, 7.5, 1 )        
hist( the.data, breaks=bins )
```

![](/img/project/training/crashcourse/unnamed-chunk-13.png) 


## Categorical (nominal) 数据.


```r
colors = c( "red", "black", "brown", "blonde" )
freqs  = c(   8  ,    22  ,    30  ,    18    ) 
hair.color = rep( colors, freqs )
table( hair.color )
```

```
## hair.color
##  black blonde  brown    red 
##     22     18     30      8
```

```r
hair.color
```

```
##  [1] "red"    "red"    "red"    "red"    "red"    "red"    "red"   
##  [8] "red"    "black"  "black"  "black"  "black"  "black"  "black" 
## [15] "black"  "black"  "black"  "black"  "black"  "black"  "black" 
## [22] "black"  "black"  "black"  "black"  "black"  "black"  "black" 
## [29] "black"  "black"  "brown"  "brown"  "brown"  "brown"  "brown" 
## [36] "brown"  "brown"  "brown"  "brown"  "brown"  "brown"  "brown" 
## [43] "brown"  "brown"  "brown"  "brown"  "brown"  "brown"  "brown" 
## [50] "brown"  "brown"  "brown"  "brown"  "brown"  "brown"  "brown" 
## [57] "brown"  "brown"  "brown"  "brown"  "blonde" "blonde" "blonde"
## [64] "blonde" "blonde" "blonde" "blonde" "blonde" "blonde" "blonde"
## [71] "blonde" "blonde" "blonde" "blonde" "blonde" "blonde" "blonde"
## [78] "blonde"
```

```r
barplot( table( hair.color ))  #一个函数的输出是另一个函数的输入
```

![](/img/project/training/crashcourse/unnamed-chunk-141.png) 

```r
barplot( table( hair.color ), col=c( "black","yellow","brown","red" ))
```

![](/img/project/training/crashcourse/unnamed-chunk-142.png) 

这里用到了一个函数的输出是另一个函数的输入


```r
temp.table = table( hair.color )
barplot( temp.table )
```

![](/img/project/training/crashcourse/unnamed-chunk-15.png) 

```r
rm( temp.table)
```


## 计算


```r
sum( the.data ) 
```

```
## [1] 77
```

```r
sum( the.data ) ^ 2 
```

```
## [1] 5929
```

```r
sum( the.data ^ 2 )   
```

```
## [1] 359
```

```r
the.data ^ 2
```

```
##  [1] 49 49 49 25 25 16 16 16 16 16 16 16  9  9  9  9  9  4  1
```

```r
x = c( 2, 3, 5 )
y = c( 3, 8, 9 )
sum( x * y )                    # product of x,y
```

```
## [1] 75
```

```r
sum(x) * sum(y)                 
```

```
## [1] 200
```

## 数学函数


```r
sqrt()
log()                           
log10()                         
sin()                           
abs()
```


## 描述性统计


```r
length( the.data )
```

```
## [1] 19
```

```r
length( hair.color )
```

```
## [1] 78
```

```r
median( the.data )
```

```
## [1] 4
```

```r
mean( the.data )
```

```
## [1] 4.053
```

```r
range( the.data )           # 区间
```

```
## [1] 1 7
```

```r
IQR( the.data )  #R有9中计算Q1,Q3的方法,sigh
```

```
## [1] 1.5
```


方差


```r
var( the.data )     
```

```
## [1] 2.608
```

标准差


```r
sd( the.data )
```

```
## [1] 1.615
```

使用summary


```r
summary( the.data )
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    1.00    3.00    4.00    4.05    4.50    7.00
```

不同的方法计算Q1，Q3


```r
quantile( the.data, c( .25,.75 ), type=2 )     # 教科书的方法
```

```
## 25% 75% 
##   3   5
```

## 是自己寻求答案的时候了-help


```r
help( t.test )                  # ?t.test
```

```
## starting httpd help server ... done
```

```r
help.search( "distribution" )   # 搜索
```

# vector,data frame 与 list,factor

## vector 取子集


```r
x=seq(2,12)
x[]
x[0]
x[2]
x[-2]
x[2:4]
x[-2:-5]
names(x)
#length(x)==11
names(x)=LETTERS[1:11]
names(x)
x[c("A","B")]
x[c(T,F)]
```


## vector操作


```r
x=1:10
x*3
x+3
x/3
x^2
sqrt(x)
10:1
-1:10
5:-7
```

## vector 操作


```r
x=1:10
y=1:5
x+y
x-y
x*y
x/y
```

## vector 操作


```r
x=1:10
y=1:3
x+y
```

```
## Warning: longer object length is not a multiple of shorter object length
```

```r
x<5
x<y
```

```
## Warning: longer object length is not a multiple of shorter object length
```

```r
any(x<y) #?
```

```
## Warning: longer object length is not a multiple of shorter object length
```

```r
all(x<y)
```

```
## Warning: longer object length is not a multiple of shorter object length
```

```r
q = c("Hockey", "Football", "Baseball", "Curling", "Rugby",
        "Lacrosse", "Basketball", "Tennis", "Cricket", "Soccer")
nchar(q)
```

## Factor

factor是R中的一类特殊类型


```r
myfac=factor(c("basic","proficient","advanced","minimal"))
class(myfac)
```

```
## [1] "factor"
```

```r
myfac 
```

```
## [1] basic      proficient advanced   minimal   
## Levels: advanced basic minimal proficient
```

如果我们想改变factor顺序怎么办？


## Factor排序


```r
myfac_o=ordered(myfac,levels=c("minimal","basic","proficient","advanced")) #?ordered
myfac_o
```

```
## [1] basic      proficient advanced   minimal   
## Levels: minimal < basic < proficient < advanced
```

```r
summary(myfac_o) #?summary
```

```
##    minimal      basic proficient   advanced 
##          1          1          1          1
```


## 将Factor转换为其他类型


```r
as.character(myfac_o)
```

```
## [1] "basic"      "proficient" "advanced"   "minimal"
```

## data frame

`data.frame`函数创建数据框


```r
x = 10:1
y = -4:5
q = c("Hockey", "Football", "Baseball", "Curling", "Rugby",
        "Lacrosse", "Basketball", "Tennis", "Cricket", "Soccer")
theDF = data.frame(x, y, q)
theDF
```

如果我想每一列的名字为预先我设定的值，而不是x,y,q，应该怎么办？


## data frame 常用操作

+ 行数，列数
+ 每列的数据类型
+ 数据框的维度
+ 每列，每行的名称


```r
theDF = data.frame(First = x, Second = y, Sport = q)
theDF
nrow(theDF)
ncol(theDF)
str(theDF)
dim(theDF)
names(theDF)
names(theDF)[1]
names(theDF)=c("First","Second","Sport")
rownames(theDF)
rownames(theDF) = c("One", "Two", "Three", "Four", "Five", "Six",
                      "Seven", "Eight", "Nine", "Ten")
rownames(theDF)
theDF
rownames(theDF)=NULL
```


## 查看data frame


```r
head(theDF,10)
tail(theDF)
?head
```

## data frame操作


```r
theDF$Sport
theDF[2,1]
theDF[3,2:3]
theDF[c(3, 5), 2]
theDF[c(3, 5), 2:3]
theDF[, 3]
theDF[, 2:3]
theDF[2, ]
theDF[, c("First", "Sport")]
#subset(theDF,theDF$Sport=="Hockey")
?subset
```

## data frame操作


```r
theDF["Sport"]
class(theDF["Sport"])
theDF[["Sport"]]
class(theDF[["Sport"]])
#[]与[[]]区别？
theDF[, "Sport", drop = FALSE]
class(theDF[, "Sport", drop = FALSE])
```

##


```r
myData = matrix( c( 2, 5, 3, 4, 6, 7, 3, 2, 18, 22, 17, 10 ), ncol=3 )
myData = as.data.frame( myData )
myData
```

```
##   V1 V2 V3
## 1  2  6 18
## 2  5  7 22
## 3  3  3 17
## 4  4  2 10
```

```r
colnames( myData ) = c( "Quiz1", "Quiz2", "Exam" )
rownames( myData ) = c( "Jeff", "Barbara", "Rachel", "Bob" )
myData
```

```
##         Quiz1 Quiz2 Exam
## Jeff        2     6   18
## Barbara     5     7   22
## Rachel      3     3   17
## Bob         4     2   10
```

```r
rownames( myData )[4] = "Robb"
myData
```

```
##         Quiz1 Quiz2 Exam
## Jeff        2     6   18
## Barbara     5     7   22
## Rachel      3     3   17
## Robb        4     2   10
```

```r
myData[4] = c( 24, 28, 30, 20 )
myData
```

```
##         Quiz1 Quiz2 Exam V4
## Jeff        2     6   18 24
## Barbara     5     7   22 28
## Rachel      3     3   17 30
## Robb        4     2   10 20
```

```r
colnames( myData )[4] = "Paper"
myData
```

```
##         Quiz1 Quiz2 Exam Paper
## Jeff        2     6   18    24
## Barbara     5     7   22    28
## Rachel      3     3   17    30
## Robb        4     2   10    20
```

```r
myData$Major = c( "Psych", "Biology", "Math", "Business" ) # $
myData
```

```
##         Quiz1 Quiz2 Exam Paper    Major
## Jeff        2     6   18    24    Psych
## Barbara     5     7   22    28  Biology
## Rachel      3     3   17    30     Math
## Robb        4     2   10    20 Business
```

```r
myData$Exam
```

```
## [1] 18 22 17 10
```

```r
rm(myData)  
ls()
```

```
##  [1] "bins"       "colors"     "f"          "freqs"      "hair.color"
##  [6] "my.data"    "myfac"      "myfac_o"    "q"          "the.data"  
## [11] "theDF"      "x"          "X"          "y"
```

## list介绍

vector里面每个变量的类型都是一样，data frame每列变量类型都一样，同时每列的长度一致。而list则可以长度不一致

Hadley Wickham's [Data structures](http://adv-r.had.co.nz/Data-structures.html)


```r
list(1,2,3)
list(c(1,2,3))
mylist1 = list(c(1, 2, 3), 3:7)
mylist1
```


## list操作


```r
list(theDF,1:10)
mylist2 = list(theDF, 1:10, mylist1)
names(mylist2)
names(mylist2) = c("data.frame", "vector", "list")
names(mylist2)
mylist2
mylist3 = list(TheDataFrame = theDF, TheVector = 1:10, TheList = mylist2)
names(mylist3)
```


## list操作


```r
mylist3[[1]]
mylist3[["TheDataFrame"]]
mylist3[[1]]$Sport
length(mylist3)
```


# 总结

我们已经学习了


+ vector,list,data frame
+ 列出工作空间中的变量: ls()
+ 了解data frame的维度: dim()
+ 列名称: colnames(), names(), dimnames()
+ 行: rownames() , dimnames()
+ 查看前几行: head()/tail()
+ data frame的变量名称
+ 利用索引取子集dataframe.name[ row.number, col.number ]
+ 利用行名，列名取子集: dataframe.name[ "row.name", "col.name" ]
+ 修改列名: colnames() = ...
+ 修改行名: rownames() = ...
+ 添加一列到data frame: 2种方法，索引以及直接使用变量名
+ $ 

## 练习


```r
USArrests[ "Iowa", "UrbanPop" ]
```

```
## [1] 57
```

```r
USArrests[ "Iowa", "UrbanPop" ] = 33
USArrests[ "Iowa", "UrbanPop" ]
```

```
## [1] 33
```

```r
USArrests[ "Iowa", "UrbanPop" ] = 57
USArrests[ "Iowa", "UrbanPop" ]
```

```
## [1] 57
```

```r
USArrests[ "Pennsylvania", ]    # 注意那个,
```

```
##              Murder Assault UrbanPop Rape
## Pennsylvania    6.3     106       72 14.9
```

```r
USArrests[ 38, ]     
```

```
##              Murder Assault UrbanPop Rape
## Pennsylvania    6.3     106       72 14.9
```

```r
#如果是访问某列

USArrests[ "UrbanPop" ]
```

```
##                UrbanPop
## Alabama              58
## Alaska               48
## Arizona              80
## Arkansas             50
## California           91
## Colorado             78
## Connecticut          77
## Delaware             72
## Florida              80
## Georgia              60
## Hawaii               83
## Idaho                54
## Illinois             83
## Indiana              65
## Iowa                 57
## Kansas               66
## Kentucky             52
## Louisiana            66
## Maine                51
## Maryland             67
## Massachusetts        85
## Michigan             74
## Minnesota            66
## Mississippi          44
## Missouri             70
## Montana              53
## Nebraska             62
## Nevada               81
## New Hampshire        56
## New Jersey           89
## New Mexico           70
## New York             86
## North Carolina       45
## North Dakota         44
## Ohio                 75
## Oklahoma             68
## Oregon               67
## Pennsylvania         72
## Rhode Island         87
## South Carolina       48
## South Dakota         45
## Tennessee            59
## Texas                80
## Utah                 80
## Vermont              32
## Virginia             63
## Washington           73
## West Virginia        39
## Wisconsin            66
## Wyoming              60
```

```r
USArrests[3]
```

```
##                UrbanPop
## Alabama              58
## Alaska               48
## Arizona              80
## Arkansas             50
## California           91
## Colorado             78
## Connecticut          77
## Delaware             72
## Florida              80
## Georgia              60
## Hawaii               83
## Idaho                54
## Illinois             83
## Indiana              65
## Iowa                 57
## Kansas               66
## Kentucky             52
## Louisiana            66
## Maine                51
## Maryland             67
## Massachusetts        85
## Michigan             74
## Minnesota            66
## Mississippi          44
## Missouri             70
## Montana              53
## Nebraska             62
## Nevada               81
## New Hampshire        56
## New Jersey           89
## New Mexico           70
## New York             86
## North Carolina       45
## North Dakota         44
## Ohio                 75
## Oklahoma             68
## Oregon               67
## Pennsylvania         72
## Rhode Island         87
## South Carolina       48
## South Dakota         45
## Tennessee            59
## Texas                80
## Utah                 80
## Vermont              32
## Virginia             63
## Washington           73
## West Virginia        39
## Wisconsin            66
## Wyoming              60
```

```r
USArrests[ ,"UrbanPop" ]
```

```
##  [1] 58 48 80 50 91 78 77 72 80 60 83 54 83 65 57 66 52 66 51 67 85 74 66
## [24] 44 70 53 62 81 56 89 70 86 45 44 75 68 67 72 87 48 45 59 80 80 32 63
## [47] 73 39 66 60
```

```r
USArrests[ ,3]
```

```
##  [1] 58 48 80 50 91 78 77 72 80 60 83 54 83 65 57 66 52 66 51 67 85 74 66
## [24] 44 70 53 62 81 56 89 70 86 45 44 75 68 67 72 87 48 45 59 80 80 32 63
## [47] 73 39 66 60
```

```r
# 这里需要注意，如果在取列是没有用那个`,`，那么结果是一个data frame，如果用了那么结果就是一个向量

# 区别在这里

USArrests[ "Murder" ]
```

```
##                Murder
## Alabama          13.2
## Alaska           10.0
## Arizona           8.1
## Arkansas          8.8
## California        9.0
## Colorado          7.9
## Connecticut       3.3
## Delaware          5.9
## Florida          15.4
## Georgia          17.4
## Hawaii            5.3
## Idaho             2.6
## Illinois         10.4
## Indiana           7.2
## Iowa              2.2
## Kansas            6.0
## Kentucky          9.7
## Louisiana        15.4
## Maine             2.1
## Maryland         11.3
## Massachusetts     4.4
## Michigan         12.1
## Minnesota         2.7
## Mississippi      16.1
## Missouri          9.0
## Montana           6.0
## Nebraska          4.3
## Nevada           12.2
## New Hampshire     2.1
## New Jersey        7.4
## New Mexico       11.4
## New York         11.1
## North Carolina   13.0
## North Dakota      0.8
## Ohio              7.3
## Oklahoma          6.6
## Oregon            4.9
## Pennsylvania      6.3
## Rhode Island      3.4
## South Carolina   14.4
## South Dakota      3.8
## Tennessee        13.2
## Texas            12.7
## Utah              3.2
## Vermont           2.2
## Virginia          8.5
## Washington        4.0
## West Virginia     5.7
## Wisconsin         2.6
## Wyoming           6.8
```

```r
USArrests[ ,"Murder" ]
```

```
##  [1] 13.2 10.0  8.1  8.8  9.0  7.9  3.3  5.9 15.4 17.4  5.3  2.6 10.4  7.2
## [15]  2.2  6.0  9.7 15.4  2.1 11.3  4.4 12.1  2.7 16.1  9.0  6.0  4.3 12.2
## [29]  2.1  7.4 11.4 11.1 13.0  0.8  7.3  6.6  4.9  6.3  3.4 14.4  3.8 13.2
## [43] 12.7  3.2  2.2  8.5  4.0  5.7  2.6  6.8
```

```r
USArrests[1]
```

```
##                Murder
## Alabama          13.2
## Alaska           10.0
## Arizona           8.1
## Arkansas          8.8
## California        9.0
## Colorado          7.9
## Connecticut       3.3
## Delaware          5.9
## Florida          15.4
## Georgia          17.4
## Hawaii            5.3
## Idaho             2.6
## Illinois         10.4
## Indiana           7.2
## Iowa              2.2
## Kansas            6.0
## Kentucky          9.7
## Louisiana        15.4
## Maine             2.1
## Maryland         11.3
## Massachusetts     4.4
## Michigan         12.1
## Minnesota         2.7
## Mississippi      16.1
## Missouri          9.0
## Montana           6.0
## Nebraska          4.3
## Nevada           12.2
## New Hampshire     2.1
## New Jersey        7.4
## New Mexico       11.4
## New York         11.1
## North Carolina   13.0
## North Dakota      0.8
## Ohio              7.3
## Oklahoma          6.6
## Oregon            4.9
## Pennsylvania      6.3
## Rhode Island      3.4
## South Carolina   14.4
## South Dakota      3.8
## Tennessee        13.2
## Texas            12.7
## Utah              3.2
## Vermont           2.2
## Virginia          8.5
## Washington        4.0
## West Virginia     5.7
## Wisconsin         2.6
## Wyoming           6.8
```

```r
USArrests[ ,1]
```

```
##  [1] 13.2 10.0  8.1  8.8  9.0  7.9  3.3  5.9 15.4 17.4  5.3  2.6 10.4  7.2
## [15]  2.2  6.0  9.7 15.4  2.1 11.3  4.4 12.1  2.7 16.1  9.0  6.0  4.3 12.2
## [29]  2.1  7.4 11.4 11.1 13.0  0.8  7.3  6.6  4.9  6.3  3.4 14.4  3.8 13.2
## [43] 12.7  3.2  2.2  8.5  4.0  5.7  2.6  6.8
```

```r
# 取10：20行

USArrests[ 10:20, ]
```

```
##           Murder Assault UrbanPop Rape
## Georgia     17.4     211       60 25.8
## Hawaii       5.3      46       83 20.2
## Idaho        2.6     120       54 14.2
## Illinois    10.4     249       83 24.0
## Indiana      7.2     113       65 21.0
## Iowa         2.2      56       57 11.3
## Kansas       6.0     115       66 18.0
## Kentucky     9.7     109       52 16.3
## Louisiana   15.4     249       66 22.2
## Maine        2.1      83       51  7.8
## Maryland    11.3     300       67 27.8
```

```r
USArrests[ c(10,11,12,13,14,15,16,17,18,19,20), ]
```

```
##           Murder Assault UrbanPop Rape
## Georgia     17.4     211       60 25.8
## Hawaii       5.3      46       83 20.2
## Idaho        2.6     120       54 14.2
## Illinois    10.4     249       83 24.0
## Indiana      7.2     113       65 21.0
## Iowa         2.2      56       57 11.3
## Kansas       6.0     115       66 18.0
## Kentucky     9.7     109       52 16.3
## Louisiana   15.4     249       66 22.2
## Maine        2.1      83       51  7.8
## Maryland    11.3     300       67 27.8
```

```r
USArrests[ seq( from = 10, to = 20, by = 1 ), ]
```

```
##           Murder Assault UrbanPop Rape
## Georgia     17.4     211       60 25.8
## Hawaii       5.3      46       83 20.2
## Idaho        2.6     120       54 14.2
## Illinois    10.4     249       83 24.0
## Indiana      7.2     113       65 21.0
## Iowa         2.2      56       57 11.3
## Kansas       6.0     115       66 18.0
## Kentucky     9.7     109       52 16.3
## Louisiana   15.4     249       66 22.2
## Maine        2.1      83       51  7.8
## Maryland    11.3     300       67 27.8
```

```r
# 取murder rate>12的行

USArrests[ Murder > 12, ] # 怎么不对啊?
```

```
## Error: object 'Murder' not found
```

```r
USArrests[ "Murder" > 12, ]
```

```
##                Murder Assault UrbanPop Rape
## Alabama          13.2     236       58 21.2
## Alaska           10.0     263       48 44.5
## Arizona           8.1     294       80 31.0
## Arkansas          8.8     190       50 19.5
## California        9.0     276       91 40.6
## Colorado          7.9     204       78 38.7
## Connecticut       3.3     110       77 11.1
## Delaware          5.9     238       72 15.8
## Florida          15.4     335       80 31.9
## Georgia          17.4     211       60 25.8
## Hawaii            5.3      46       83 20.2
## Idaho             2.6     120       54 14.2
## Illinois         10.4     249       83 24.0
## Indiana           7.2     113       65 21.0
## Iowa              2.2      56       57 11.3
## Kansas            6.0     115       66 18.0
## Kentucky          9.7     109       52 16.3
## Louisiana        15.4     249       66 22.2
## Maine             2.1      83       51  7.8
## Maryland         11.3     300       67 27.8
## Massachusetts     4.4     149       85 16.3
## Michigan         12.1     255       74 35.1
## Minnesota         2.7      72       66 14.9
## Mississippi      16.1     259       44 17.1
## Missouri          9.0     178       70 28.2
## Montana           6.0     109       53 16.4
## Nebraska          4.3     102       62 16.5
## Nevada           12.2     252       81 46.0
## New Hampshire     2.1      57       56  9.5
## New Jersey        7.4     159       89 18.8
## New Mexico       11.4     285       70 32.1
## New York         11.1     254       86 26.1
## North Carolina   13.0     337       45 16.1
## North Dakota      0.8      45       44  7.3
## Ohio              7.3     120       75 21.4
## Oklahoma          6.6     151       68 20.0
## Oregon            4.9     159       67 29.3
## Pennsylvania      6.3     106       72 14.9
## Rhode Island      3.4     174       87  8.3
## South Carolina   14.4     279       48 22.5
## South Dakota      3.8      86       45 12.8
## Tennessee        13.2     188       59 26.9
## Texas            12.7     201       80 25.5
## Utah              3.2     120       80 22.9
## Vermont           2.2      48       32 11.2
## Virginia          8.5     156       63 20.7
## Washington        4.0     145       73 26.2
## West Virginia     5.7      81       39  9.3
## Wisconsin         2.6      53       66 10.8
## Wyoming           6.8     161       60 15.6
```

```r
## subset
```

```r
subset( USArrests, select = UrbanPop )
```

```
##                UrbanPop
## Alabama              58
## Alaska               48
## Arizona              80
## Arkansas             50
## California           91
## Colorado             78
## Connecticut          77
## Delaware             72
## Florida              80
## Georgia              60
## Hawaii               83
## Idaho                54
## Illinois             83
## Indiana              65
## Iowa                 57
## Kansas               66
## Kentucky             52
## Louisiana            66
## Maine                51
## Maryland             67
## Massachusetts        85
## Michigan             74
## Minnesota            66
## Mississippi          44
## Missouri             70
## Montana              53
## Nebraska             62
## Nevada               81
## New Hampshire        56
## New Jersey           89
## New Mexico           70
## New York             86
## North Carolina       45
## North Dakota         44
## Ohio                 75
## Oklahoma             68
## Oregon               67
## Pennsylvania         72
## Rhode Island         87
## South Carolina       48
## South Dakota         45
## Tennessee            59
## Texas                80
## Utah                 80
## Vermont              32
## Virginia             63
## Washington           73
## West Virginia        39
## Wisconsin            66
## Wyoming              60
```

```r
subset( USArrests, subset = Murder > 12 )
```

```
##                Murder Assault UrbanPop Rape
## Alabama          13.2     236       58 21.2
## Florida          15.4     335       80 31.9
## Georgia          17.4     211       60 25.8
## Louisiana        15.4     249       66 22.2
## Michigan         12.1     255       74 35.1
## Mississippi      16.1     259       44 17.1
## Nevada           12.2     252       81 46.0
## North Carolina   13.0     337       45 16.1
## South Carolina   14.4     279       48 22.5
## Tennessee        13.2     188       59 26.9
## Texas            12.7     201       80 25.5
```

```r
subset( USArrests, subset = Murder > 12 & Assault >= 255 )
```

```
##                Murder Assault UrbanPop Rape
## Florida          15.4     335       80 31.9
## Michigan         12.1     255       74 35.1
## Mississippi      16.1     259       44 17.1
## North Carolina   13.0     337       45 16.1
## South Carolina   14.4     279       48 22.5
```

```r
subset( USArrests, subset = Murder > 12 & Assault >= 255, select = Rape )
```

```
##                Rape
## Florida        31.9
## Michigan       35.1
## Mississippi    17.1
## North Carolina 16.1
## South Carolina 22.5
```

## sort



```r
attach(USArrests)
sort( Murder )
```

```
##  [1]  0.8  2.1  2.1  2.2  2.2  2.6  2.6  2.7  3.2  3.3  3.4  3.8  4.0  4.3
## [15]  4.4  4.9  5.3  5.7  5.9  6.0  6.0  6.3  6.6  6.8  7.2  7.3  7.4  7.9
## [29]  8.1  8.5  8.8  9.0  9.0  9.7 10.0 10.4 11.1 11.3 11.4 12.1 12.2 12.7
## [43] 13.0 13.2 13.2 14.4 15.4 15.4 16.1 17.4
```

```r
sort( Murder, decreasing = TRUE )
```

```
##  [1] 17.4 16.1 15.4 15.4 14.4 13.2 13.2 13.0 12.7 12.2 12.1 11.4 11.3 11.1
## [15] 10.4 10.0  9.7  9.0  9.0  8.8  8.5  8.1  7.9  7.4  7.3  7.2  6.8  6.6
## [29]  6.3  6.0  6.0  5.9  5.7  5.3  4.9  4.4  4.3  4.0  3.8  3.4  3.3  3.2
## [43]  2.7  2.6  2.6  2.2  2.2  2.1  2.1  0.8
```

不过如果我想data frame里面的数据根据某列来排序呢？


```r
SO = order( Murder )
SO
```

```
##  [1] 34 19 29 15 45 12 49 23 44  7 39 41 47 27 21 37 11 48  8 16 26 38 36
## [24] 50 14 35 30  6  3 46  4  5 25 17  2 13 32 20 31 22 28 43 33  1 42 40
## [47]  9 18 24 10
```

```r
head(USArrests[ SO, ])
```

```
##               Murder Assault UrbanPop Rape
## North Dakota     0.8      45       44  7.3
## Maine            2.1      83       51  7.8
## New Hampshire    2.1      57       56  9.5
## Iowa             2.2      56       57 11.3
## Vermont          2.2      48       32 11.2
## Idaho            2.6     120       54 14.2
```

```r
USArrests[34,]
```

```
##              Murder Assault UrbanPop Rape
## North Dakota    0.8      45       44  7.3
```

SO 成为了我们用了排序的dummy variable


```r
head(USArrests[ order( Murder ), ])
```

```
##               Murder Assault UrbanPop Rape
## North Dakota     0.8      45       44  7.3
## Maine            2.1      83       51  7.8
## New Hampshire    2.1      57       56  9.5
## Iowa             2.2      56       57 11.3
## Vermont          2.2      48       32 11.2
## Idaho            2.6     120       54 14.2
```

```r
# 注意我们没有改变USArrests
USArrests
```

```
##                Murder Assault UrbanPop Rape
## Alabama          13.2     236       58 21.2
## Alaska           10.0     263       48 44.5
## Arizona           8.1     294       80 31.0
## Arkansas          8.8     190       50 19.5
## California        9.0     276       91 40.6
## Colorado          7.9     204       78 38.7
## Connecticut       3.3     110       77 11.1
## Delaware          5.9     238       72 15.8
## Florida          15.4     335       80 31.9
## Georgia          17.4     211       60 25.8
## Hawaii            5.3      46       83 20.2
## Idaho             2.6     120       54 14.2
## Illinois         10.4     249       83 24.0
## Indiana           7.2     113       65 21.0
## Iowa              2.2      56       57 11.3
## Kansas            6.0     115       66 18.0
## Kentucky          9.7     109       52 16.3
## Louisiana        15.4     249       66 22.2
## Maine             2.1      83       51  7.8
## Maryland         11.3     300       67 27.8
## Massachusetts     4.4     149       85 16.3
## Michigan         12.1     255       74 35.1
## Minnesota         2.7      72       66 14.9
## Mississippi      16.1     259       44 17.1
## Missouri          9.0     178       70 28.2
## Montana           6.0     109       53 16.4
## Nebraska          4.3     102       62 16.5
## Nevada           12.2     252       81 46.0
## New Hampshire     2.1      57       56  9.5
## New Jersey        7.4     159       89 18.8
## New Mexico       11.4     285       70 32.1
## New York         11.1     254       86 26.1
## North Carolina   13.0     337       45 16.1
## North Dakota      0.8      45       44  7.3
## Ohio              7.3     120       75 21.4
## Oklahoma          6.6     151       68 20.0
## Oregon            4.9     159       67 29.3
## Pennsylvania      6.3     106       72 14.9
## Rhode Island      3.4     174       87  8.3
## South Carolina   14.4     279       48 22.5
## South Dakota      3.8      86       45 12.8
## Tennessee        13.2     188       59 26.9
## Texas            12.7     201       80 25.5
## Utah              3.2     120       80 22.9
## Vermont           2.2      48       32 11.2
## Virginia          8.5     156       63 20.7
## Washington        4.0     145       73 26.2
## West Virginia     5.7      81       39  9.3
## Wisconsin         2.6      53       66 10.8
## Wyoming           6.8     161       60 15.6
```

```r
USArrests.sorted = USArrests[ SO, ]
USArrests.sorted
```

```
##                Murder Assault UrbanPop Rape
## North Dakota      0.8      45       44  7.3
## Maine             2.1      83       51  7.8
## New Hampshire     2.1      57       56  9.5
## Iowa              2.2      56       57 11.3
## Vermont           2.2      48       32 11.2
## Idaho             2.6     120       54 14.2
## Wisconsin         2.6      53       66 10.8
## Minnesota         2.7      72       66 14.9
## Utah              3.2     120       80 22.9
## Connecticut       3.3     110       77 11.1
## Rhode Island      3.4     174       87  8.3
## South Dakota      3.8      86       45 12.8
## Washington        4.0     145       73 26.2
## Nebraska          4.3     102       62 16.5
## Massachusetts     4.4     149       85 16.3
## Oregon            4.9     159       67 29.3
## Hawaii            5.3      46       83 20.2
## West Virginia     5.7      81       39  9.3
## Delaware          5.9     238       72 15.8
## Kansas            6.0     115       66 18.0
## Montana           6.0     109       53 16.4
## Pennsylvania      6.3     106       72 14.9
## Oklahoma          6.6     151       68 20.0
## Wyoming           6.8     161       60 15.6
## Indiana           7.2     113       65 21.0
## Ohio              7.3     120       75 21.4
## New Jersey        7.4     159       89 18.8
## Colorado          7.9     204       78 38.7
## Arizona           8.1     294       80 31.0
## Virginia          8.5     156       63 20.7
## Arkansas          8.8     190       50 19.5
## California        9.0     276       91 40.6
## Missouri          9.0     178       70 28.2
## Kentucky          9.7     109       52 16.3
## Alaska           10.0     263       48 44.5
## Illinois         10.4     249       83 24.0
## New York         11.1     254       86 26.1
## Maryland         11.3     300       67 27.8
## New Mexico       11.4     285       70 32.1
## Michigan         12.1     255       74 35.1
## Nevada           12.2     252       81 46.0
## Texas            12.7     201       80 25.5
## North Carolina   13.0     337       45 16.1
## Alabama          13.2     236       58 21.2
## Tennessee        13.2     188       59 26.9
## South Carolina   14.4     279       48 22.5
## Florida          15.4     335       80 31.9
## Louisiana        15.4     249       66 22.2
## Mississippi      16.1     259       44 17.1
## Georgia          17.4     211       60 25.8
```

```r
rm( USArrests.sorted )

# 从高到低排序

SO = order( Murder, decreasing = TRUE )
USArrests[ SO, ]
```

```
##                Murder Assault UrbanPop Rape
## Georgia          17.4     211       60 25.8
## Mississippi      16.1     259       44 17.1
## Florida          15.4     335       80 31.9
## Louisiana        15.4     249       66 22.2
## South Carolina   14.4     279       48 22.5
## Alabama          13.2     236       58 21.2
## Tennessee        13.2     188       59 26.9
## North Carolina   13.0     337       45 16.1
## Texas            12.7     201       80 25.5
## Nevada           12.2     252       81 46.0
## Michigan         12.1     255       74 35.1
## New Mexico       11.4     285       70 32.1
## Maryland         11.3     300       67 27.8
## New York         11.1     254       86 26.1
## Illinois         10.4     249       83 24.0
## Alaska           10.0     263       48 44.5
## Kentucky          9.7     109       52 16.3
## California        9.0     276       91 40.6
## Missouri          9.0     178       70 28.2
## Arkansas          8.8     190       50 19.5
## Virginia          8.5     156       63 20.7
## Arizona           8.1     294       80 31.0
## Colorado          7.9     204       78 38.7
## New Jersey        7.4     159       89 18.8
## Ohio              7.3     120       75 21.4
## Indiana           7.2     113       65 21.0
## Wyoming           6.8     161       60 15.6
## Oklahoma          6.6     151       68 20.0
## Pennsylvania      6.3     106       72 14.9
## Kansas            6.0     115       66 18.0
## Montana           6.0     109       53 16.4
## Delaware          5.9     238       72 15.8
## West Virginia     5.7      81       39  9.3
## Hawaii            5.3      46       83 20.2
## Oregon            4.9     159       67 29.3
## Massachusetts     4.4     149       85 16.3
## Nebraska          4.3     102       62 16.5
## Washington        4.0     145       73 26.2
## South Dakota      3.8      86       45 12.8
## Rhode Island      3.4     174       87  8.3
## Connecticut       3.3     110       77 11.1
## Utah              3.2     120       80 22.9
## Minnesota         2.7      72       66 14.9
## Idaho             2.6     120       54 14.2
## Wisconsin         2.6      53       66 10.8
## Iowa              2.2      56       57 11.3
## Vermont           2.2      48       32 11.2
## Maine             2.1      83       51  7.8
## New Hampshire     2.1      57       56  9.5
## North Dakota      0.8      45       44  7.3
```

```r
USArrests[ order( Murder, decreasing = TRUE ), ]
```

```
##                Murder Assault UrbanPop Rape
## Georgia          17.4     211       60 25.8
## Mississippi      16.1     259       44 17.1
## Florida          15.4     335       80 31.9
## Louisiana        15.4     249       66 22.2
## South Carolina   14.4     279       48 22.5
## Alabama          13.2     236       58 21.2
## Tennessee        13.2     188       59 26.9
## North Carolina   13.0     337       45 16.1
## Texas            12.7     201       80 25.5
## Nevada           12.2     252       81 46.0
## Michigan         12.1     255       74 35.1
## New Mexico       11.4     285       70 32.1
## Maryland         11.3     300       67 27.8
## New York         11.1     254       86 26.1
## Illinois         10.4     249       83 24.0
## Alaska           10.0     263       48 44.5
## Kentucky          9.7     109       52 16.3
## California        9.0     276       91 40.6
## Missouri          9.0     178       70 28.2
## Arkansas          8.8     190       50 19.5
## Virginia          8.5     156       63 20.7
## Arizona           8.1     294       80 31.0
## Colorado          7.9     204       78 38.7
## New Jersey        7.4     159       89 18.8
## Ohio              7.3     120       75 21.4
## Indiana           7.2     113       65 21.0
## Wyoming           6.8     161       60 15.6
## Oklahoma          6.6     151       68 20.0
## Pennsylvania      6.3     106       72 14.9
## Kansas            6.0     115       66 18.0
## Montana           6.0     109       53 16.4
## Delaware          5.9     238       72 15.8
## West Virginia     5.7      81       39  9.3
## Hawaii            5.3      46       83 20.2
## Oregon            4.9     159       67 29.3
## Massachusetts     4.4     149       85 16.3
## Nebraska          4.3     102       62 16.5
## Washington        4.0     145       73 26.2
## South Dakota      3.8      86       45 12.8
## Rhode Island      3.4     174       87  8.3
## Connecticut       3.3     110       77 11.1
## Utah              3.2     120       80 22.9
## Minnesota         2.7      72       66 14.9
## Idaho             2.6     120       54 14.2
## Wisconsin         2.6      53       66 10.8
## Iowa              2.2      56       57 11.3
## Vermont           2.2      48       32 11.2
## Maine             2.1      83       51  7.8
## New Hampshire     2.1      57       56  9.5
## North Dakota      0.8      45       44  7.3
```


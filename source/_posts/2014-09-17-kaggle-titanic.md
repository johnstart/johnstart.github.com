title: Kaggle上的泰坦尼克生还数据分析
date: 2014-09-17 11:28:30
category: 数据科学
tags: [数据科学,R,Kaggle]
---

# 数据准备

先根据数据的codebook来给每列命名，同时预先设定类型


```r
setwd("d:/course/kaggle/titanic/")
train.col.types <- c('integer', # PassengerId
  'factor', # Survived
  'factor', # Pclass
  'character', # Name
  'factor', # Sex
  'numeric', # Age
  'integer', # SibSp
  'integer', # Parch
  'character', # Ticket
  'numeric', # Fare
  'character', # Cabin
  'factor' # Embarked
)
test.col.types=train.col.types[-2]
train.raw=read.csv("./train.csv",colClasses=train.col.types,na.strings=c("NA",""))
test.raw=read.csv("./test.csv",colClasses=test.col.types,na.strings=c("NA",""))
dim(train.raw) 
```

```
## [1] 891  12
```

```r
dim(test.raw)
```

```
## [1] 418  11
```

这里预先设定了每列的class，之后读入数据。train数据集为891行12列大小矩阵，测试数据集418行，11列（没有了是否生还的列）

# 数据探索


```r
summary(train.raw)
```

```
##   PassengerId  Survived Pclass      Name               Sex     
##  Min.   :  1   0:549    1:216   Length:891         female:314  
##  1st Qu.:224   1:342    2:184   Class :character   male  :577  
##  Median :446            3:491   Mode  :character               
##  Mean   :446                                                   
##  3rd Qu.:668                                                   
##  Max.   :891                                                   
##                                                                
##       Age            SibSp           Parch          Ticket         
##  Min.   : 0.42   Min.   :0.000   Min.   :0.000   Length:891        
##  1st Qu.:20.12   1st Qu.:0.000   1st Qu.:0.000   Class :character  
##  Median :28.00   Median :0.000   Median :0.000   Mode  :character  
##  Mean   :29.70   Mean   :0.523   Mean   :0.382                     
##  3rd Qu.:38.00   3rd Qu.:1.000   3rd Qu.:0.000                     
##  Max.   :80.00   Max.   :8.000   Max.   :6.000                     
##  NA's   :177                                                       
##       Fare          Cabin           Embarked  
##  Min.   :  0.0   Length:891         C   :168  
##  1st Qu.:  7.9   Class :character   Q   : 77  
##  Median : 14.5   Mode  :character   S   :644  
##  Mean   : 32.2                      NA's:  2  
##  3rd Qu.: 31.0                                
##  Max.   :512.3                                
## 
```

```r
summary(test.raw)
```

```
##   PassengerId   Pclass      Name               Sex           Age       
##  Min.   : 892   1:107   Length:418         female:152   Min.   : 0.17  
##  1st Qu.: 996   2: 93   Class :character   male  :266   1st Qu.:21.00  
##  Median :1100   3:218   Mode  :character                Median :27.00  
##  Mean   :1100                                           Mean   :30.27  
##  3rd Qu.:1205                                           3rd Qu.:39.00  
##  Max.   :1309                                           Max.   :76.00  
##                                                         NA's   :86     
##      SibSp           Parch          Ticket               Fare      
##  Min.   :0.000   Min.   :0.000   Length:418         Min.   :  0.0  
##  1st Qu.:0.000   1st Qu.:0.000   Class :character   1st Qu.:  7.9  
##  Median :0.000   Median :0.000   Mode  :character   Median : 14.5  
##  Mean   :0.447   Mean   :0.392                      Mean   : 35.6  
##  3rd Qu.:1.000   3rd Qu.:0.000                      3rd Qu.: 31.5  
##  Max.   :8.000   Max.   :9.000                      Max.   :512.3  
##                                                     NA's   :1      
##     Cabin           Embarked
##  Length:418         C:102   
##  Class :character   Q: 46   
##  Mode  :character   S:270   
##                             
##                             
##                             
## 
```

这里看出Age有177个缺失，占比比较大，而对于character变量Cabin，Name等没有相关信息，一个简单的查看缺失数据的方法就是采用missamp()函数


```r
require(Amelia)
missmap(train.raw, main="Titanic缺失数据图",
        col=c("yellow", "black"), legend=FALSE)
```

![](/img/kaggle/titanic/unnamed-chunk-3.png) 

这里看出Cabin，Age有较多缺失数据，而Embark只有两个缺失数据。

接下来先简单分析一下数据，比如生还与死亡的比例，不同等级(Pclass)对生还有无影响，我们知道老人，妇女，小孩会优先所以看看性别，年龄是否有影响，此外票的价钱有关吗（fare）？当然可能还有其它的变量，这里我们就不一一列举...

## 生还与死亡对比


```r
barplot(table(train.raw$Survived),names.arg=c("死亡","生还"),
        main="生还 vs 死亡")
```

![](/img/kaggle/titanic/unnamed-chunk-4.png) 

## 不同舱位等级的影响


```r
survive.rate.class=table(train.raw$Survived,train.raw$Pclass)
barplot(survive.rate.class,names.arg=c("一等","二等","三等"),
        main="不同舱位生还 vs 死亡",
        legend.text=c("死亡","生还"),
        args.legend=list(x="topleft"))
```

![](/img/kaggle/titanic/unnamed-chunk-5.png) 

```r
round((survive.rate.class[2,]/colSums(survive.rate.class))*100,2)
```

```
##     1     2     3 
## 62.96 47.28 24.24
```

看起来一等舱，二等舱显然比三等舱要高。如果按照比例来看各舱位生还的比例依次为：62.96%,47.28,24.24%。除了舱位等级好的在越上面，跑得快，下面的先被淹外，估计这个也和当时发生事故的时间在睡觉有关系。

## 不同性别的生还率对比


```r
survive.rate.sex=table(train.raw$Survived,train.raw$Sex)
barplot(survive.rate.sex,names.arg=c("女","男"),
        main="不同性别生还 vs 死亡",
        legend.text=c("死亡","生还"),
        args.legend=list(x="topleft"))
```

![](/img/kaggle/titanic/unnamed-chunk-6.png) 

```r
round((survive.rate.sex[2,]/colSums(survive.rate.sex))*100,2)
```

```
## female   male 
##  74.20  18.89
```

看来在生与死的选择时男人们还是发扬了高风亮节！女性生还率达到了74.2%，男性只有18.89%

## 不同年龄的生还率


```r
age.breaker=c(0,18,50,100)
age.cut= cut(train.raw$Age,breaks=age.breaker,,labels=c("小孩","成年人","老人"))
train.raw$age.cut=age.cut
survive.rate.age=table(train.raw$Survived,train.raw$age.cut)
barplot(survive.rate.age,
        main="不同年龄生还 vs 死亡",
        legend.text=c("死亡","生还"),
        args.legend=list(x="topleft"))
```

![](/img/kaggle/titanic/unnamed-chunk-71.png) 

```r
round((survive.rate.age[2,]/colSums(survive.rate.age))*100,2)
```

```
##   小孩 成年人   老人 
##  50.36  38.75  34.38
```

```r
age.breaker=c(0,15,55,100)
age.cut= cut(train.raw$Age,breaks=age.breaker,,labels=c("小孩","成年人","老人"))
train.raw$age.cut=age.cut
survive.rate.age=table(train.raw$Survived,train.raw$age.cut)
barplot(survive.rate.age,
        main="不同年龄生还 vs 死亡",
        legend.text=c("死亡","生还"),
        args.legend=list(x="topleft"))
```

![](/img/kaggle/titanic/unnamed-chunk-72.png) 

```r
round((survive.rate.age[2,]/colSums(survive.rate.age))*100,2)
```

```
##   小孩 成年人   老人 
##  59.04  38.75  30.00
```

这里分别用15和18来作为小孩的判断标准，50，55分别作为老人判断标准。可以看出小孩的生还率分别为59.04%与50.36%，生还率最高。而老人的生还率却比成年人低，也许是年龄大了的影响？

## 马赛克图

还有一种更好的方法就是利用mosaicplot图


```r
mosaicplot(train.raw$Pclass ~ train.raw$Survived,
           main="不同舱位等级生还 vs 死亡 ", shade=FALSE,
           color=TRUE, xlab="舱位", ylab="生还")
```

![](/img/kaggle/titanic/unnamed-chunk-81.png) 

```r
mosaicplot(train.raw$Sex ~ train.raw$Survived,
           main="不同性别生还 vs 死亡 ", shade=FALSE,
           color=TRUE, xlab="性别", ylab="生还")
```

![](/img/kaggle/titanic/unnamed-chunk-82.png) 

## 相关性分析

前面大致可以得出性别，年龄，舱位对生还率有很大影响，那么其他的票价，上船的港口，在那个cabin等等变量对生还率有影响吗？这里用相关性分析来观察一下：


```r
train.corrgram = train.raw
# 相关性分析要求全部为数字
train.corrgram$Survived <- as.numeric(train.corrgram$Survived)
train.corrgram$Pclass <- as.numeric(train.corrgram$Pclass)
train.corrgram$Embarked <- as.numeric(train.corrgram$Embarked)
train.corrgram$Sex <- as.numeric(train.corrgram$Sex)
train.corrgram[which(is.na(train.corrgram$Embarked)),]$Embarked=3
cor(train.corrgram[,corrgram.vars])
```

```
## Error: object 'corrgram.vars' not found
```

这里大致可以看出除了Pclass,Sex,Age以为，Fare还和生还率有关，而Embarked似乎也有一定关系（虽然理论上来讲登船地点应该与这个无关？）而Age由于有大量缺失值这里相关性为NA,下一步的目标似乎应该是在于如何处理这些缺失的数据上面。

图形分析也许更简单快捷，直接使用`corrgram`来分析。



```r
require(corrgram)
```

```
## Loading required package: corrgram
## Loading required package: seriation
```

```r
# 字符变量这里不考虑
corrgram.vars = c("Survived", "Pclass", "Sex", "Age",
                   "SibSp", "Parch", "Fare", "Embarked")
corrgram(train.corrgram[,corrgram.vars], lower.panel=panel.ellipse, 
         upper.panel=panel.pie,text.panel=panel.txt, main="泰坦尼克生还率相关性分析")
```

![0](/img/kaggle/titanic/unnamed-chunk-10.png) 

# 初次建模

有了前面的探索性分析，大致我们对数据有了一定了解，考虑先建立一个模型来进行一次初步预测。显然我们需要处理的问题是一个分类为题，首先想到的方法有逻辑回归，决策树，classification rule，更高级的可以考虑采用boost，随机森林。这里我们先考虑从简单的逻辑回归开始。

## 构建训练集与测试集


```r
require(caret)
```

```
## Loading required package: caret
## Loading required package: lattice
## 
## Attaching package: 'lattice'
## 
## The following object is masked from 'package:seriation':
## 
##     panel.lines
## 
## Loading required package: ggplot2
```

```r
set.seed(201409)
inTrain = createDataPartition(train.raw$Survived,
                                     p = 0.8, list = FALSE)
training = train.raw[inTrain, ]
test = train.raw[-inTrain, ]
```

## 第一个模型

互联网时代来临之前，人们的收入通常和年龄相关，因此对缺失数据较多的年龄我们先根据不同等级来填充一个估计值。这里直接使用median。

另外这里还计算了3等舱的价格的median，因为我们发现测试集里面有一个记录的Fare值缺失。



```r
first.class.age=median(training[training$Pclass=="1",]$Age,na.rm=T)
second.class.age=median(training[training$Pclass=="2",]$Age,na.rm=T)
third.class.age=median(training[training$Pclass=="1",]$Age,na.rm=T)
training[is.na(training$Age)&training$Pclass=="1",]$Age=first.class.age
training[is.na(training$Age)&training$Pclass=="2",]$Age=second.class.age
training[is.na(training$Age)&training$Pclass=="3",]$Age=third.class.age
third.class.fare=median(training[training$Pclass=="3",]$Fare,na.rm=T)
model.logit.1 <- train(Survived ~ Sex + Pclass + Age + Embarked + Fare,
                     data = training, method="glm")

model.logit.1
```

```
## Generalized Linear Model 
## 
## 714 samples
##  12 predictors
##   2 classes: '0', '1' 
## 
## No pre-processing
## Resampling: Bootstrapped (25 reps) 
## 
## Summary of sample sizes: 712, 712, 712, 712, 712, 712, ... 
## 
## Resampling results
## 
##   Accuracy  Kappa  Accuracy SD  Kappa SD
##   0.8       0.6    0.02         0.05    
## 
## 
```

模型accuracy为0.8,kappa 为0.6，采用了bootstrap 采样方式。作为我们的第一个模型看起来还不错


```r
first.class.age=median(test[test$Pclass=="1",]$Age,na.rm=T)
second.class.age=median(test[test$Pclass=="2",]$Age,na.rm=T)
third.class.age=median(test[test$Pclass=="1",]$Age,na.rm=T)
test[is.na(test$Age)&test$Pclass=="1",]$Age=first.class.age
test[is.na(test$Age)&test$Pclass=="2",]$Age=second.class.age
test[is.na(test$Age)&test$Pclass=="3",]$Age=third.class.age

predict.model.1=predict(model.logit.1,test)
table(test$Survived,predict.model.1)
```

```
##    predict.model.1
##      0  1
##   0 91 18
##   1 22 46
```

```r
sensitivity(test$Survived,predict.model.1)
```

```
## [1] 0.8053
```

```r
specificity(test$Survived,predict.model.1)
```

```
## [1] 0.7188
```

```r
#require(gmodels)
#CrossTable(test$Survived,predict.model.1)
```

相对我们自己构建的测试集sensitivity,specificity分别为0.8,0.71也还行。


```r
require(ROCR)
```

```
## Loading required package: ROCR
## Loading required package: gplots
## KernSmooth 2.23 loaded
## Copyright M. P. Wand 1997-2009
## 
## Attaching package: 'gplots'
## 
## The following object is masked from 'package:stats':
## 
##     lowess
```

```r
predictions.model.1=prediction(c(predict.model.1),labels=test$Survived)
perf = performance(predictions.model.1, measure = "tpr", x.measure = "fpr")
plot(perf, main = "ROC curve",col = "blue", lwd = 2)
abline(a = 0, b = 1, lwd = 2, lty = 2)
```

![4](/img/kaggle/titanic/unnamed-chunk-14.png) 


## 预测


```r
first.class.age=median(test.raw[test.raw$Pclass=="1",]$Age,na.rm=T)
second.class.age=median(test.raw[test.raw$Pclass=="2",]$Age,na.rm=T)
third.class.age=median(test.raw[test.raw$Pclass=="1",]$Age,na.rm=T)
test.raw[is.na(test.raw$Age)&test.raw$Pclass=="1",]$Age=first.class.age
test.raw[is.na(test.raw$Age)&test.raw$Pclass=="2",]$Age=second.class.age
test.raw[is.na(test.raw$Age)&test.raw$Pclass=="3",]$Age=third.class.age
test.raw[153,]$Fare=third.class.fare
predict.final.model.1=predict(model.logit.1,newdata=test.raw)
predictions=data.frame(PassengerId=test.raw$PassengerId, Survived=predict.final.model.1)
write.csv(predictions,file="Titanic_predictions_1.csv", row.names=FALSE, quote=FALSE)
```

提交到Kaggle上，模型accuracy显示为0.77左右，排在了2000名左右的位置，显然这不是一个好的结果。不过到此为止我们还有很多事情没有做：

+ 有4个特征没有使用分别是sibsp,parch,Cabin,names。
  + 也许家人的数目会影响生还率，比如需要照顾家人或者上救生船的时候会让别的家庭成员上等等
  + Cabin这个变量虽然有很多缺失但是理论上应该也是一个关键特征，因为不同舱位对逃生影响很大
  + names看似没有用但是似乎它们都满足了 xx Title, xx xx这样一种模式，而西方人的title可以反映年龄。因此可能我们需要利用names来构建一个新变量Title
+ 对Age的插值之前我们采用了直接取不同等级舱位年龄median的方式完成，这是否是最佳，可否有更好方法？比如可以考虑根据姓名的Title来判断
+ 前面的模型参数我们没有进行任何的优化，也许优化参数可以取得更好结果？
+ 除了逻辑回归，我们还可以采用决策树，随机森林，boosting方法，是否这些模型会带来更好结果？

这些就是下一步我们可以考虑的地方了。

# 模型优化

先重新读入一次数据：


```r
train.col.types <- c('integer', # PassengerId
  'factor', # Survived
  'factor', # Pclass
  'character', # Name
  'factor', # Sex
  'numeric', # Age
  'integer', # SibSp
  'integer', # Parch
  'character', # Ticket
  'numeric', # Fare
  'character', # Cabin
  'factor' # Embarked
)
test.col.types=train.col.types[-2]
train.raw=read.csv("./train.csv",colClasses=train.col.types,na.strings=c("NA",""))
test.raw=read.csv("./test.csv",colClasses=test.col.types,na.strings=c("NA",""))
```


## 数据整理与清洗

首先考虑前面提到的Names中的title.西方人的命名都有一定规则，首先关于Title：

- Mr. 更个年龄的先生
- Miss. 未婚女士（不过女权解放的现代，好像也有已婚的用？）
- Mrs. (已婚女士)
- Master. (男孩，早期使用，现在应该不用了。不过泰坦尼克灾难发生时适用)
- Rev., Col. Sir. Dr.  etc... 等大多与职业相关，应该都是男士

而对西方的名字，比如：

Baclini, Mrs. Solomon (Latifa Qurban)

+ Mrs. 已婚
+ Solomon ：丈夫的名字,比如Jane Smith与John Smith结婚后，她就是Mrs. John Smith.
+ Latifa : 他自己的名字
+ Qurban ："maiden" ，结婚前的姓
+ Baclini:她丈夫的姓，也就是他丈夫Solomon的姓.

有了这些基础那么我们可以想法把中间的那个Title给取出来：


```r
#这里格式是xx, Title . xx xx
getTitle = function(data) {
  title.start = regexpr("\\,[A-Z ]{1,20}\\.", data$Name, TRUE)
  title.end = title.start+attr(title.start, "match.length")-1
  data$Title = substr(data$Name, title.start+2, title.end-1)
  return (data$Title)
}
train.raw$Title=getTitle(train.raw)
require(dplyr)
```

```
## Loading required package: dplyr
## 
## Attaching package: 'dplyr'
## 
## The following objects are masked from 'package:stats':
## 
##     filter, lag
## 
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
head(train.raw %>% group_by(Title) %>% summarise(count=n())%>%arrange(desc(count)))
```

```
## Source: local data frame [6 x 2]
## 
##    Title count
## 1     Mr   517
## 2   Miss   182
## 3    Mrs   125
## 4 Master    40
## 5     Dr     7
## 6    Rev     6
```

主要的称呼为Mr,Miss,Mrs,Master，剩余的类型都比较少，可以归入professional一类。因此我们增加一列Title，分为Mr,Miss,Mrs,Master,Professional 5种类型。


```r
title.filter=c("Mr","Mrs","Miss","Master","Professional")
recodeTitle = function(data,title.filter) {
  if(!(data %in% title.filter))
    data = "Professional"
  return (data)
}
train.raw$Title=sapply(train.raw$Title,recodeTitle,title.filter)
```

简单利用median来对年龄进行插值,Embarked的NA直接使用“S”,fare根据class的median来计算


```r
imputeAge = function(Age,Title,title.filter) {
  for(v in title.filter) {
    Age[is.na(Age)]=median(Age[Title==v],na.rm=T)    
  }
  return(Age)
}

title.filter=c("Mr","Mrs","Miss","Master","Professional")
train.raw$Age=imputeAge(train.raw$Age,train.raw$Title,title.filter)

imputeEmbarked=function(Embarked) {
  Embarked[is.na(Embarked)]="S"
  return(Embarked)
}
train.raw$Embarked=imputeEmbarked(train.raw$Embarked)

imputeFare = function(fare,pclass,pclass.filter) {
  for(v in pclass.filter) {
    fare[is.na(fare)]=median(fare[pclass==v],na.rm=T)    
  }
  return(fare)
}

pclass.filter=c(1,2,3)
train.raw$fare=imputeFare(train.raw$Fare,train.raw$Pclass,pclass.filter)
```

## 第二个模型

现在使用了新的计算确实年龄的方法，同时加上SibSp以及Parch变量建立一个新模型


```r
require(caret)
set.seed(201409)
inTrain = createDataPartition(train.raw$Survived,
                                     p = 0.8, list = FALSE)
training = train.raw[inTrain, ]
test = train.raw[-inTrain, ]
model.logit.2 <- train(Survived ~ Sex + Pclass + Age + Embarked + Fare+Title+SibSp+Parch,data = training, method="glm")
model.logit.2
```

```
## Generalized Linear Model 
## 
## 714 samples
##  13 predictors
##   2 classes: '0', '1' 
## 
## No pre-processing
## Resampling: Bootstrapped (25 reps) 
## 
## Summary of sample sizes: 714, 714, 714, 714, 714, 714, ... 
## 
## Resampling results
## 
##   Accuracy  Kappa  Accuracy SD  Kappa SD
##   0.8       0.6    0.02         0.05    
## 
## 
```

```r
predict.model.2=predict(model.logit.2,test)
table(test$Survived,predict.model.2)
```

```
##    predict.model.2
##      0  1
##   0 93 16
##   1 17 51
```

```r
sensitivity(test$Survived,predict.model.2)
```

```
## [1] 0.8455
```

```r
specificity(test$Survived,predict.model.2)
```

```
## [1] 0.7612
```

```r
predict(model.logit.2,test)
```

```
##   [1] 0 1 1 0 1 0 0 0 0 1 0 0 1 0 1 0 0 0 1 0 0 0 0 0 0 0 1 0 1 0 1 0 1 0 0
##  [36] 1 0 0 0 1 1 0 1 0 0 1 0 0 0 0 0 0 1 0 0 1 0 0 1 1 0 1 0 1 1 1 0 1 1 0
##  [71] 0 1 0 1 0 1 1 1 0 0 1 0 0 0 1 1 0 0 0 0 1 0 0 0 1 0 0 0 1 1 1 0 0 0 1
## [106] 1 0 0 1 1 0 0 0 0 0 0 1 0 1 0 1 0 1 0 1 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0
## [141] 1 0 0 1 0 1 1 0 1 1 0 0 0 1 0 0 1 1 1 0 1 0 1 1 0 0 1 0 0 1 0 1 1 1 0
## [176] 0 0
## Levels: 0 1
```

训练集accuracy达到0.82，对于测试集sensitivity,specificity都达到了有提高

## 预测


```r
title.filter=c("Mr","Mrs","Miss","Master","Professional")
test.raw$Title=getTitle(test.raw)
test.raw$Title=sapply(test.raw$Title,recodeTitle,title.filter)
test.raw$Age=imputeAge(test.raw$Age,test.raw$Title,title.filter)
test.raw$Embarked=imputeEmbarked(test.raw$Embarked)
test.raw$Fare=imputeFare(test.raw$Fare,test.raw$Pclass,pclass.filter)
predict.final.model.2=predict(model.logit.2,newdata=test.raw)
predictions=data.frame(PassengerId=test.raw$PassengerId, Survived=predict.final.model.2)
write.csv(predictions,file="Titanic_predictions_2.csv", row.names=FALSE, quote=FALSE)
```

提交到kaggle，新的排名一下提高的了800多位，一下子比之前提高了1600多位。回到之前我们提到的可能优化方向：

+ 有4个特征没有使用分别是sibsp,parch,Cabin,names。
  + 也许家人的数目会影响生还率，比如需要照顾家人或者上救生船的时候会让别的家庭成员上等等
  + Cabin这个变量虽然有很多缺失但是理论上应该也是一个关键特征，因为不同舱位对逃生影响很大
  + names看似没有用但是似乎它们都满足了 xx Title, xx xx这样一种模式，而西方人的title可以反映年龄。因此可能我们需要利用names来构建一个新变量Title
+ 对Age的插值之前我们采用了直接取不同等级舱位年龄median的方式完成，这是否是最佳，可否有更好方法？比如可以考虑根据姓名的Title来判断
+ 前面的模型参数我们没有进行任何的优化，也许优化参数可以取得更好结果？
+ 除了逻辑回归，我们还可以采用决策树，随机森林，boosting方法，是否这些模型会带来更好结果？

现在还可以做的有:

+ Cabin这个变量虽然有很多缺失但是理论上应该也是一个关键特征，因为不同舱位对逃生影响很大
+ 前面的模型参数我们没有进行任何的优化，也许优化参数可以取得更好结果？
+ 除了逻辑回归，我们还可以采用决策树，随机森林，boosting方法，是否这些模型会带来更好结果？

此外前面的代码中的插值函数显然可以重构成一个通用函数就不用每个特征写一个函数了，不过今天先到这里下一步的优化我们就下次再写了。
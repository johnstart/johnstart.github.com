title: 机器学习与R(10)-朴素贝叶斯
date: 2014-08-22 19:49:24
category: 数据科学
tags: [数据科学,机器学习,R,机器学习与R]
---

## Naive Bayes

关于贝叶斯定理阮一峰的博客已经写得相当好了，我就不献丑了，要了解看这里：

[贝叶斯推断及其互联网应用（一）：定理简介](http://www.ruanyifeng.com/blog/2011/08/bayesian_inference_part_one.html)  
[贝叶斯推断及其互联网应用（二）：过滤垃圾邮件](http://www.ruanyifeng.com/blog/2011/08/bayesian_inference_part_two.html)  

朴素贝叶斯则是利用贝叶斯定理来进行分类的方法。它具有以下优点：

+ 简单，快速，有效
+ 能很好处理噪声，缺失数据
+ 对小和大样本都可以
+ 比较容易获得预测的概率估计

不过它也存在：

+ 要求特征间平等及重要性相同，特征相互独立（通常不会存在，这也是为什么要叫naive的原因）
+ 对主要是大量数字特征的数据集不太适合
+ 估计概率不如分类可靠（所以一般用它来分类）

## 拉普拉斯估计值

朴素贝叶斯用来进行文本分类通常存在一个问题，如果训练的数据中某个词没有出现过，那么该词的概率就变成了0，从而如公式所示，整个计算的垃圾邮件概率就变成了0，这显然不合理，于是就有了拉普拉斯估计的应用，我们给它预估一个值这样就可以避免问题的出现了。

![垃圾邮件计算示例](/img/rmachine/bayes-laplace.png)


## 数字特征的处理

贝叶斯是通过计算频数来进行学习，这显然只能用于分类数据，对于连续数据这时可以考虑把它进行分区（bin）处理，比如利用`cut`函数。不过需要注意的是这样会导致信息的丢失。


# 利用朴素贝叶斯来判断垃圾短信

这里我们以判断垃圾短信为例，数据来自[sms spam](http://www.dt.fee.unicamp.br/~tiago/smsspamcollection/)数据集

## 数据准备

把数据下载后读入：


```r
sms_raw <- read.table("SMSSpamCollection", stringsAsFactors = FALSE, sep = "\t", 
    header = F, comment = "", quote = NULL)
names(sms_raw) = c("type", "text")
str(sms_raw)
```

```
## 'data.frame':	5574 obs. of  2 variables:
##  $ type: chr  "ham" "ham" "spam" "ham" ...
##  $ text: chr  "Go until jurong point, crazy.. Available only in bugis n great world la e buffet... Cine there got amore wat..." "Ok lar... Joking wif u oni..." "Free entry in 2 a wkly comp to win FA Cup final tkts 21st May 2005. Text FA to 87121 to receive entry question(std txt rate)T&C"| __truncated__ "U dun say so early hor... U c already then say..." ...
```


注意如果在`read.table`里面不指定`quote=NULL`那么会遇到如下问题

>Warning message:In scan(file, what, nmax, sep, dec, quote, skip, nlines, na.strings,  :  EOF within quoted string

实际上你如果仔细研究一下数据，你可以发现这是因为数据里面的5082行开始有`""`导致。


接下来将`type`转换为factor变量，因为贝叶斯分类要求目标变量为factor类型。


```r
sms_raw$type = factor(sms_raw$type)
table(sms_raw$type)
```

```
## 
##  ham spam 
## 4827  747
```


数据集里面有4827条正常短信，747条垃圾短信

## 数据预处理

对于文本的分析通常我们会用到[`tm`包](http://tm.r-forge.r-project.org/)


```r
require(tm)
```

```
## Loading required package: tm
```

```r
sms_corpus <- Corpus(VectorSource(sms_raw$text))
```


这里将原始数据中的短消息都作为向量输入构建语料库


```r
print(sms_corpus)
```

```
## A corpus with 5574 text documents
```

```r
inspect(sms_corpus[1:3])
```

```
## A corpus with 3 text documents
## 
## The metadata consists of 2 tag-value pairs and a data frame
## Available tags are:
##   create_date creator 
## Available variables in the data frame are:
##   MetaID 
## 
## [[1]]
## Go until jurong point, crazy.. Available only in bugis n great world la e buffet... Cine there got amore wat...
## 
## [[2]]
## Ok lar... Joking wif u oni...
## 
## [[3]]
## Free entry in 2 a wkly comp to win FA Cup final tkts 21st May 2005. Text FA to 87121 to receive entry question(std txt rate)T&C's apply 08452810075over18's
```


这里可以看出语料库有5574个文档，实际与我们的数据集样本数一样。每个文档对应的就是一条短信。从前3条短信我们看出，文档的里面有标题，数字，还有标点符号，以及大小写，为了方便分析我们进行如下处理：


```r
corpus_clean <- tm_map(sms_corpus, tolower)
corpus_clean <- tm_map(corpus_clean, removeNumbers)
corpus_clean <- tm_map(corpus_clean, removeWords, stopwords())
corpus_clean <- tm_map(corpus_clean, removePunctuation)
corpus_clean <- tm_map(corpus_clean, stripWhitespace)
inspect(corpus_clean[1:3])
```

```
## A corpus with 3 text documents
## 
## The metadata consists of 2 tag-value pairs and a data frame
## Available tags are:
##   create_date creator 
## Available variables in the data frame are:
##   MetaID 
## 
## [[1]]
## go jurong point crazy available bugis n great world la e buffet cine got amore wat
## 
## [[2]]
## ok lar joking wif u oni
## 
## [[3]]
## free entry wkly comp win fa cup final tkts st may text fa receive entry questionstd txt ratetcs apply s
```


以上依次把所有词转换为小写，去掉数字，去掉停止词（就是类似and，or，the之类，也就是冠词、介词、副词或连词），去掉标点，最后去掉所有空格。

完成了上述步骤，我们就需要统计每个词在文档中出现的频率了，这可以通过构建document term稀疏矩阵完成，这个稀疏矩阵的行对应一个文档，列则对应了每个词。term document则反过来。


```r
sms_dtm <- DocumentTermMatrix(corpus_clean)
```


## 准备训练与测试数据

有了上面的矩阵，我们就可以开始准备训练数据与测试数据了，还是用`caret`包的`createDataPartition`来完成，可以看出训练与测试数据中的垃圾短信比例都相似。


```r
require(caret)
```

```
## Loading required package: caret
```

```
## Warning: package 'caret' was built under R version 3.0.3
```

```
## Loading required package: lattice
## Loading required package: ggplot2
```

```r
set.seed(2014)
inTrain = createDataPartition(y = sms_raw$type, p = 0.75, list = FALSE)
sms_raw_train = sms_raw[inTrain, ]
sms_raw_test = sms_raw[-inTrain, ]

sms_dtm_train = sms_dtm[inTrain, ]
sms_dtm_test = sms_dtm[-inTrain, ]

sms_corpus_train = corpus_clean[inTrain]
sms_corpus_test = corpus_clean[-inTrain]

prop.table(table(sms_raw_train$type))
```

```
## 
##    ham   spam 
## 0.8659 0.1341
```

```r
prop.table(table(sms_raw_test$type))
```

```
## 
##    ham   spam 
## 0.8664 0.1336
```


## wordcloud

最简单的文本分析方法就是市场词云了，我们用`wordcloud`包


```r
require(wordcloud)
```

```
## Loading required package: wordcloud
## Loading required package: Rcpp
```

```
## Warning: package 'Rcpp' was built under R version 3.0.3
```

```
## Loading required package: RColorBrewer
```

```r
wordcloud(sms_corpus_train, min.freq = 40, random.order = FALSE)
```

![](/img/rmachine/rmachine-10-8.png) 


这里的`min.freq`是词出现的最小频率，通常我们用语料库的10%来开始(训练语料库有4182个文档)。上面那个词云只是给出了一个总体印象，对我们的分析没有太大帮助，所有我们考虑分布看看垃圾邮件与正常邮件的区别



```r
spam <- subset(sms_raw_train, type == "spam")
ham <- subset(sms_raw_train, type == "ham")
wordcloud(spam$text, max.words = 40, scale = c(3, 0.5))
```

![](/img/rmachine/rmachine-10-91.png) 

```r
wordcloud(ham$text, max.words = 40, scale = c(3, 0.5))
```

![](/img/rmachine/rmachine-10-92.png) 


很显然可以看出垃圾邮件里面`free,now,prize,text claim`比较多。

## 词频

把所有的词都考虑进来显然不是很好的方法，我们的矩阵有7986个特征，因此我们需要考虑缩小范围，于是采用`findFreqTerms`的方法取大于5的特征（具体取多少根据数据的数据情况）：



```r
findFreqTerms(sms_dtm_train, 5)[10:20]
```

```
##  [1] "actually"  "add"       "address"   "admirer"   "advance"  
##  [6] "aft"       "afternoon" "age"       "ago"       "ahead"    
## [11] "aight"
```

```r
sms_dict <- Dictionary(findFreqTerms(sms_dtm_train, 5))
```


获得了频数大于5的词后，我们再利用它来生成一个字典，这样可以在文档矩阵中指出，我只取字典中有的词，新的矩阵就只有1252个特征了。


```r
sms_train <- DocumentTermMatrix(sms_corpus_train, list(dictionary = sms_dict))
sms_test <- DocumentTermMatrix(sms_corpus_test, list(dictionary = sms_dict))
```


我们的目标是想通过短信里面有或者是没有某个词来判断是否是垃圾短信，那么我们很显然应该使用的矩阵是标记某个词在某个短信中出现了还是没有出现。因此写个函数来完成这一个功能：


```r
convert_counts <- function(x) {
    x <- ifelse(x > 0, 1, 0)
    x <- factor(x, levels = c(0, 1), labels = c("No", "Yes"))
    return(x)
}
```


对矩阵每一列进行这样的处理：


```r
sms_train <- apply(sms_train, MARGIN = 2, convert_counts)
sms_test <- apply(sms_test, MARGIN = 2, convert_counts)
```


于是我们可以得到最终用来构建模型的数据集。

## 模型训练

在R里面有多个包都提供朴素贝叶斯分类，比如`e1071`包，还有`klaR`包的`NaiveBayes()`。这里使用`e1071`


```r
require(e1071)
```

```
## Loading required package: e1071
## Loading required package: class
```

```r
sms_classifier <- naiveBayes(sms_train, sms_raw_train$type)
```


于是我们得到了分类器`sms_classifier`

## 模型评估

有了模型就可以对测试数据进行预测:

`pred(m,test,type="class")

这里的type如果为class代表是分类，如果是raw则代表概率的计算


```r
sms_test_pred <- predict(sms_classifier, sms_test)
require(gmodels)
```

```
## Loading required package: gmodels
```

```
## Warning: package 'gmodels' was built under R version 3.0.3
```

```r
CrossTable(sms_test_pred, sms_raw_test$type, prop.chisq = FALSE, prop.t = FALSE, 
    dnn = c("predicted", "actual"))
```

```
## 
##  
##    Cell Contents
## |-------------------------|
## |                       N |
## |           N / Row Total |
## |           N / Col Total |
## |-------------------------|
## 
##  
## Total Observations in Table:  1392 
## 
##  
##              | actual 
##    predicted |       ham |      spam | Row Total | 
## -------------|-----------|-----------|-----------|
##          ham |      1202 |        29 |      1231 | 
##              |     0.976 |     0.024 |     0.884 | 
##              |     0.997 |     0.156 |           | 
## -------------|-----------|-----------|-----------|
##         spam |         4 |       157 |       161 | 
##              |     0.025 |     0.975 |     0.116 | 
##              |     0.003 |     0.844 |           | 
## -------------|-----------|-----------|-----------|
## Column Total |      1206 |       186 |      1392 | 
##              |     0.866 |     0.134 |           | 
## -------------|-----------|-----------|-----------|
## 
## 
```


我们可以看出简单的贝叶斯模型的效果却很好，97.6%的正确率，186封垃圾邮件中29封误判为了正常邮件。而1206封正常邮件中4封误判为垃圾邮件。把正常邮件误判为垃圾邮件的影响显然更大，这是需要考虑的地方。

## 模型改进

前面说过了拉普拉斯估计的问题，那么如果我们假设拉普拉斯估计会怎么样呢？


```r
sms_classifier2 <- naiveBayes(sms_train, sms_raw_train$type, laplace = 1)
sms_test_pred2 <- predict(sms_classifier2, sms_test)
CrossTable(sms_test_pred2, sms_raw_test$type, prop.chisq = FALSE, prop.t = FALSE, 
    prop.r = FALSE, dnn = c("predicted", "actual"))
```

```
## 
##  
##    Cell Contents
## |-------------------------|
## |                       N |
## |           N / Col Total |
## |-------------------------|
## 
##  
## Total Observations in Table:  1392 
## 
##  
##              | actual 
##    predicted |       ham |      spam | Row Total | 
## -------------|-----------|-----------|-----------|
##          ham |      1204 |        30 |      1234 | 
##              |     0.998 |     0.161 |           | 
## -------------|-----------|-----------|-----------|
##         spam |         2 |       156 |       158 | 
##              |     0.002 |     0.839 |           | 
## -------------|-----------|-----------|-----------|
## Column Total |      1206 |       186 |      1392 | 
##              |     0.866 |     0.134 |           | 
## -------------|-----------|-----------|-----------|
## 
## 
```


加了拉普拉斯估计后，正常邮件误判为垃圾邮件减少了2封，而垃圾邮件误判为正常邮件的增加了1封。似乎新的模型要好些。

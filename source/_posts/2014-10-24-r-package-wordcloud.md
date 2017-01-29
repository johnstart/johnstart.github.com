---
title: R语言可视化-所有R包词云
date: 2014-10-24 19:45:37
category: 数据科学
tags: [数据科学,data science,可视化,R]
---


# 获取包信息


直接访问R的网站就可以获得该信息,打开 http://cran.r-project.org/web/packages/available_packages_by_date.html 网页可以发现网站有一个html表

```
    require(XML)

    ## Loading required package: XML

    require(tm)

    ## Loading required package: tm

    require(wordcloud)

    ## Loading required package: wordcloud
    ## Loading required package: Rcpp
    ## Loading required package: RColorBrewer

    require(SnowballC)

    ## Loading required package: SnowballC

    require(RColorBrewer)
    url = "http://cran.r-project.org/web/packages/available_packages_by_date.html"
    t = readHTMLTable(url)[[1]]
 ```

# 利用tm包对文档进行处理

从网站可以发现表的第3列提供了包的描述信息，因此取第3列。依次去掉标点符号，转换为小写，去掉常用的`the,a..`等英文停止词。由于大部分包的描述都会包含`data,analysis,model,function,package...`所以这里把这些词也去掉

```
    package.corpus <- Corpus(DataframeSource(data.frame(as.character(t[,3]))))

    package.corpus <- tm_map(package.corpus, removePunctuation)
    package.corpus <- tm_map(package.corpus, tolower)
    package.corpus <- tm_map(package.corpus, removeNumbers)
    package.corpus <- tm_map(package.corpus, removeWords, stopwords("english"))
    package.corpus <- tm_map(package.corpus, stemDocument)
    package.corpus <- tm_map(package.corpus, removeWords,
                        c("analysis","model","data","function","package","packag","method"))
```

转换为文档矩阵，并按照词的频率排序

```
    package.tdm <- TermDocumentMatrix(package.corpus)
    package.m <- as.matrix(package.tdm)
    package.order <- sort(rowSums(package.m),decreasing=TRUE)
    package.data <- data.frame(word = names(package.order),freq=package.order)
    table(package.data$word,package.data$freq)
```

# 生成word cloud

```
    mypal <- brewer.pal(8,"Dark2")
    setwd("c:/baidu/rcourse/r-machine-training/img")
    png("packages_wordcloud.png", width=800,height=600)
    wordcloud(package.data$word,package.data$freq, scale=c(8,.2),min.freq=5,
    max.words=200, random.order=FALSE, rot.per=.15, colors=mypal)
    dev.off()
```

![包的词云](/img/project/packagecloud/packages_wordcloud.png)
---
layout: post
title: 数据之路
date: 2014-08-16 15:22:50
modified: 2014-08-16 15:22:50
category: 数据科学
tags: [数据科学,data science]
---


# 数据科学家自我修养——一份数据科学的开放课程清单

最近一年以来，大数据这个概念被吹嘘的天花乱坠，仿佛你要是不说大数据就落伍了。继云计算之后，大数据已然成为IT行业的热点。《哈佛商业评论》更是宣称“数据科学家”是二十一世纪最性感的职业。所谓性感，既代表着难以名状的诱惑，又说明了大家都不知道它干的是什么。这里我不想重复什么是大数据，什么是数据科学，而是想以个人过去接近2年的通过MOOC（开放课程）来学习数据科学的实践来给出一份个人建议的数据科学学习之路的课程与书籍清单。

## 数据科学家的自我修养

Drew Conway给出的数据科学的一个[文氏图](http://drewconway.com/zia/2013/3/26/the-data-science-venn-diagram)，很好的诠释的数据科学的技能要求。而这里我基于传统的道，术，用来将数据科学的课程分成三类在后面一一列出。不过还是让我们先从数据科学入门谈起。

### 数据科学入门

如果你公司的管理层，只是感觉想了解一下什么是大数据，个人建议从[big data for performance](https://www.open2study.com/courses/big-data-for-better-performance)这门课开始，课程有4个模块，很简单的内容，当然也有些不是很正确的内容;-)，但作为入门还是不错的。接下来你需要了解一下Hadoop，来自Udacity的[Intro to Hadoop and MapReduce](https://www.udacity.com/course/ud617)是个不错的选择。

最后如果你要想忽悠一下别人，看看维克托•迈尔•舍恩伯格(Viktor Mayer-Schönberger)的[大数据时代](http://book.douban.com/subject/20429677/)，这是国外大数据研究的先河之作。


### 如何利用大数据


#### 数据科学之道

要掌握数据科学，基础的数学与统计学知识不可避免，这里强烈推荐：

+ 普林斯顿大学[Statistics One](https://www.coursera.org/course/stats1) 统计学基础，假设检验，ANOVA，线性回归等等
+ 斯坦福大学[Statistics learning](https://class.stanford.edu/courses/HumanitiesScience/StatLearning/Winter2014/about) 基本的有监督学习介绍，回归，分类，聚类，树，SVM，K-means clustering等等

这两门课都是名校教授讲解，课程深入浅出，一个帮助你统计学入门，一个帮助你数据分析与机器学习入门。不过如果你听统计学初步都觉得吃力，那么可以考虑先听一下台湾大学的[概率论](https://www.coursera.org/course/prob)的前几讲，对概率有了初步知识后再学统计。

当然如果你想更深入一些你一定不能错过斯坦福大学的[Machine Learning](https://www.coursera.org/course/ml)，这是Coursera创始人的经典课程。

除了以上课程，你也可以看看约翰霍普金斯大学5月开设的3门课程：

+ [Statistical Inference](https://www.coursera.org/course/statinference)
+ [Practical Machine Learning](https://www.coursera.org/course/predmachlearn)
+ [Regression Models](https://www.coursera.org/course/regmods)

当然[Edx](http://www.edx.org),[Coursera](http://www.coursera.org)也是寻找相关课程的好地方

#### 数据科学之术

有了数据科学之道，我们下一步需要进行的就是如何实现它了，有人推荐Python，也有人推荐R，或者来自Apache的mahout，个人推荐Python+R，于是你可以看看：

+ 莱斯大学的[An Introduction to Interactive Programming in Python](https://www.coursera.org/course/interactivepython)
+ 华盛顿大学的[High Performance Scientific Computing](https://www.coursera.org/course/scicomp)
+ 约翰霍普金斯大学的[R Programming](https://www.coursera.org/course/rprog)

之前约翰霍普金斯大学还开过：

+ [Computing for data analysis](http://coursera.org)
+ [data analysis](http://coursera.org)

不过似乎在去年我学完后没有再开了。

了解了R，Python，下一步就是大名鼎鼎的Hadoop生态系统了，Udacity上的[Intro to Hadoop and MapReduce](https://www.udacity.com/course/ud617)是不错的入门选择，之后[IBM 的大数据大学](http://bigdatauniversity.com/)上的Hadoop，云计算，课程也是不错的选择。

#### 数据科学之用

一切技术最终都要回归商业的本质，掌握了数据科学之道和数据科学之术，我们还需要将其应用于商业中。这里首当其冲的就是如何把从数据中解读的智慧表达出来，说服别人，这里要推荐来自密歇根大学的[Introduction to Public Speaking](https://www.coursera.org/course/publicspeak)。这门课可以帮助你更好的组织你的讲演，演示等（目前这门课正在开课中）。

除了表达能力，很多时候我们的数据不是单纯的数据，我们需要理解数据分析与公司战略的关系，如果我们要开发数据产品，那么它是如何影响我们的运营，财务决策的，当然最终所有的一切都会受到宏观经济的影响，以下的几门课程可以帮助你更好的理解数据之用：

+ 马里兰大学 [Developing Innovative Ideas for New Companies](https://www.coursera.org/course/innovativeideas)
+ 弗吉尼亚大学[Foundations of Business Strategy](https://www.coursera.org/course/strategy101)
+ 加州大学[The Power of Macroeconomics](https://www.coursera.org/course/ucimacroeconomics)
+ 清华大学[财务分析与决策](http://www.xuetangx.com/courses/TsinghuaX/80512073_2014_1X/_2014_/about)
+ 宾夕法尼亚大学[An Introduction to Operations Management](https://www.coursera.org/course/operations)

最后我要推荐[Data Science for Business](http://www.amazon.com/Data-Science-Business-data-analytic-thinking/dp/1449361323)这本书，这本书将数据科学之道与用完美结合，亚马逊上五星评价！

## 其它资源

除了以上列出的课程，你也可以参考：

+ [coursera](www.coursera.org)上约翰霍普金斯大学的数据科学专业课程 [data sicence](https://www.coursera.org/specialization/jhudatascience/1?utm_medium=catalogSpec)。一共9门课，目前每个月都在开课。
+ 来自网络上的[数据科学硕士开发课程清单](http://datasciencemasters.org/)
+ 以及[学堂](http://www.xuetangx.com/knowledge/2/)网收集的数据科学课程



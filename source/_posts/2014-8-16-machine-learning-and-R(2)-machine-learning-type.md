---
title: 机器学习与R(2)-机器学习的类型
date: 2014-08-16 15:33:17
modified: 2014-08-16 15:33:17
category: 数据科学
tags: [数据科学,机器学习,R,机器学习与R]
---

# 有监督与无监督(supervised learning vs unsupervised learning)

机器学习可以分成两组：构造预测模型的有监督学习与构造描述性模型的无监督学习。

**预测模型**需要理由数据中的其它变量值来预测某个变量的值，需要预测那个变量就成为了**目标**变量，其它的变量则是feature变量。预测模型明确指出了什么需要学习，那么这个过程就是**有监督学习**。

常用的有监督学习方法有**分类(classification)，回归(regression)**等。比如预测：

+ 某人是否信用卡会赖帐
+ 某球队是否会赢
+ 地震是否发生
+ 收入
+ 测试成绩

**描述模型**更多的是对数据的模式的总结，因为没有学习目标，所以是**无监督学习**。例如**market basket analysis**，分析客户买的东西间的关联。描述性模型将数据集分成同类组叫**聚类(clustering)**

# 算法与数据匹配

通常Classification问题我们用：

+ Nearest Neighbor 
+ naive Bayes 
+ Decision Trees 
+ Classification Rule Learners

数字预测我们用:

+ linear regression
+ regression trees
+ model trees

而Neural Networks,Support vetor machines则两种情况都可以。

对于无监督学习，常用association rules来解决模式detection，k-means clustering来进行聚类分析。

总结起来我们的问题就变成了：分类，数字预测，模式detection，聚类。不同的问题需要不同算法。



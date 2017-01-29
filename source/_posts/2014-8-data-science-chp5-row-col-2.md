title: 数据科学(5)-行与列-2
date: 2014-08-17 11:06:44
category: 数据科学
tags: [数据科学,Introduction to data science]
---

本文为**Introduction to Data Science**一书的翻译，由网友义务完成，了解参加翻译的网友，请点击[这里](https://github.com/johnstart/data-science/blob/gh-pages/task.md)，如果要加入我们请加入qq群171546473,了解翻译规则点击[这里](https://github.com/johnstart/data-science/blob/gh-pages/index.md)

 `str() `函数展现了括号内数据对象的结构。在这个例子中，我们可以肯定myFamily是一个数据框，因为我们先前的命令建立了这个数据框。但是，未来会遇到各种各样的情况，我们无法确定R是怎样建立一个新的数据对象。因此，使用 `str() `函数随时了解数据对象的结构很重要。

在第一行输出结果中，我们可以确定myFamily是一个数据框，其中包含了5个观测值（统计师通常使用观测值而不是案例或实例）和4个变量。"$" 符号将后续的输出信息分为四个部分，分别对应4个变量。这四个部分分别描述了myFamily数据框对象的列信息。

根据R的输出格式，变量名紧跟着冒号和数据类型。

```{r,echo=FALSE,eval=FALSE} 
myFamilyNames<-c("Dad","Mom","Sis","Bro","Dog")
myFamilyAges<-c(43,42,12,8,5)
myFamilyGenders<-c("Male","Female","Female","Male","Female")
myFamilyWeights<-c(188,136,83,61,44)
myFamily<-data.frame(myFamilyNames,myFamilyAges,myFamilyGenders,myFamilyWeights)
str(myFamily)
```

```
 $ myFamilyGenders: Factor w/ 2 levels "Female","Male": 2 1 1 2 1
```

例如，myFamilyGenders显示的结果是"factor"(因子)。"因子"是R的一个专业术语，是一类特殊类型的标签，用于识别和管理观测值。R按照字母顺序排列呈现开头部分少许的观测值（由于例子中的数据框比较小，现在呈现的是数据框内所有的观测值。）变量myFamilyGenders一共有两个水平，表示这个变量一共有两个选项："female"和"male"。R会分配一个从1开始的数字给每个水平。所以，在这个例子中，1表示观测值"female"，2表示观测值"male"（因为female比male先出现在字母表，所以Female是第一个因子标签表示为1。）如果仔细思考的话，你会产生这个的疑问为什么我们输入一些字符串文本例如"male"但是R会将字符串文本转换成为数字并称之为"因子"。这是由于统计的起源。多年来，研究人员习惯将实验组称之为"Exp",对照组称之为"Ctl",不使用文本字符串以外的其他标签。因此，R假定，当你输入一些字符串的时候，例如"male"，你想表达的是一个组的标签，R会将"male"当做因子的一个水平，除非告诉R你不想将字符串转化为水平。如果你不想将字符串转化为水平，可以使用 `data.frame() `函数的一个选项： `stringsAsFactors=FALSE `。我们之后会深入研究选项和默认值。

这个真的是很复杂。相比之下，两个数值变量myFamilyAges和myFamilyWeights就简单地多了。我们可以看到冒号后面的数据类型是“num”(数值)和开头部分少许的观测值：

```{r,echo=FALSE,eval=FALSE} 
myFamilyNames<-c("Dad","Mom","Sis","Bro","Dog")
myFamilyAges<-c(43,42,12,8,5)
myFamilyGenders<-c("Male","Female","Female","Male","Female")
myFamilyWeights<-c(188,136,83,61,44)
myFamily<-data.frame(myFamilyNames,myFamilyAges,myFamilyGenders,myFamilyWeights)
str(myFamily)
```

```
$ myFamilyAges   : num  43 42 12 8 5
```

综合所有的输出结果，我们得到了数据框myFamily的详尽信息，同时可以使用这些信息做一些深入的分析。综合上述信息可知，R使用一些难懂的标签和转化规则。R是为专业人员设计的而不是为业余人员设计的，所以我们要好好学习，早日成为专业人员。

接下来，我们要研究另一个非常有用的函数 `summary() `. `summary() `函数和 `str() `所呈现的信息有共同点，但是`summary() `会提供更多的信息，特别对数值型变量。下面是我们所得的：

```{r,echo=c(6)，prompt=TRUE} 
myFamilyNames<-c("Dad","Mom","Sis","Bro","Dog")
myFamilyAges<-c(43,42,12,8,5)
myFamilyGenders<-c("Male","Female","Female","Male","Female")
myFamilyWeights<-c(188,136,83,61,44)
myFamily<-data.frame(myFamilyNames,myFamilyAges,myFamilyGenders,myFamilyWeights)
summary(myFamily)
```

为了更好地展示输出结果，这些列做了一定的调整。列/变量的名字显示了关于这列的信息，每一格所涵盖的信息都是独立于其他的。（所以是没有任何意义的。例如，"Bro:1"和"Min."在同一行输出）。值得注意的是，`str() `输出结果有很大不同，取决于我们所说的因子，例如myFamilyNames和myFamilyGenders，和数值变量如myFamilyAges和myFamilyWeights。因子所在的列列出了水平的名称和观测值出现的次数。例如，在myFamilyGenders列中有3个女性和2个男性。相反，在数值型变量，我们得到5个不同的分位数来总结这个变量。下面是这个五个分位数的介绍：

* “Min"(最小值)指的是观测值中的最小值。在这个数据框中，5是狗狗的年纪，也是家庭成员中年纪最小的成员。
* "1st Qu." (第一四分位数)指的是第一个四分之一组的上限值。如果我们观测所有的观测值，并将它们按年纪大小（或者体重轻重）排序分成四组，每一组都含有相同个数的观测值。

第一四分位数|第二四分位数|第三四分位数|第四四分位数
---|---|---|---
最小25%的观测值|小于中位数25%的观测值|大于中位数25%的观测值|大于中位数25%的观测值


就像数轴一样，最小的观测值在左边最大的观测值在最右边。我们看一下myFamilyAges 最左边这组（包含了四分之一观测值），它的下限是5（狗狗的年龄），它的上限是8（哥哥的年龄）。所以年龄或其他变量的"第一个四分位数"值是区分第一个四分之一组和其他四分之一组的值。值得注意的是，如果观测值的个数不能被四整除，这个值是一个近似值。

* "Median"(中位数)指的是将所有观测值平均分为两部分的值，即有一半的观测值大于中位数，另一半观测值小于中位数。如果在深入思考一下，中位数也是区分第二个四分之一组和第三个四分之一组的值。

* "Mean" (平均值)如之前所学的是所有观测值的平均值。例如，这个家庭的平均年龄是22岁。

* "3rd Qu." 是第三四分位数。如果你还记得第一四分位数和中位数，那第三四分位数是区分第三个四分之一组和第四个四分之一组的值。你或许会对这些分位数产生疑惑，他们为什么有用。统计学家很喜欢这些，因为他们可以初步展现数据的分布情况。大家都有排序或分割东西的经历，如分披萨，理牌，给队员分组。对大多数人来说，已知四个相等大小的组，知道需要多大年纪多少体重（或者其他变量）可以进入下一个组是很有用的信息。

* 最后，"Max" (最大值)是观测值中最大的值。例如，在这个数据框中父亲的体重是最大的，有188.看上去是个比较苗条的人。

本章结束前的最后一点：如何获取存储在新数据框内的变量。R使用向量列表存储在数据框内，我们可以使用数据框的名字和向量的名字和"$"符合将两者相关联起来，例如：

```{r,echo=c(6)，prompt=TRUE} 
myFamilyNames<-c("Dad","Mom","Sis","Bro","Dog")
myFamilyAges<-c(43,42,12,8,5)
myFamilyGenders<-c("Male","Female","Female","Male","Female")
myFamilyWeights<-c(188,136,83,61,44)
myFamily<-data.frame(myFamilyNames,myFamilyAges,myFamilyGenders,myFamilyWeights)
myFamily$myFamilyAges
```      

如果你疑惑我们为什么需要输入如此长一大串，还需要`$`符号加在两个名字中间，我们可以像先前建立数据框一样直接输入"myFamilyAges".接下来是值得指出的，当我们创建myFamily数据框时，我们从每个向量中都复制了所有信息存储到新的空间中。因此，现在我们创建了"myFamily"数据框，myFamily$myFamilyAges实际上指的是完全不同的向量值（虽然现在还是相同的）。你可以自己尝试一下，试着加上一些数据到原始的向量内，myFamilyAges：

```{r,prompt=TRUE} 
myFamilyAges<-c(myFamilyAges,11)
myFamilyAges
myFamily$myFamilyAges
```  

我们看一下上面五行信息。第一行中我们使用`c()`命令在原始的年龄数据中增加了一个数值11，存储在myFamilyAges中（我们或许领养了一只老猫到家中）。第二行，我们命令R输出myFamilyAges向量中的值。第三行，R输出了原先的五个数值和新加入的数值11。我们命令R输出myFamil$myFamilyAges,但是，R只输出了原先的五个数值。这个表明数据框内的列/向量数值已经与之前单独的向量数据完全不同了。所以我们要非常小心，如果我们建立新的数据框用于后续的分析，那在分析时避免因使用原始数据集而出错。

下面是继上面问题而引申的。我们现在有一个很好的数据框，包含了5个观测值和4个变量。这是一个矩形数据集，正如本章开头所述。如果我们在一个变量中新加入一个数值，如下：

``` {r,eval=FALSE，prompt=TRUE} 
myFamily$myFamilyAges<-c(myFamily$myFamilyAges,11)
```  

如果运行这条命令，我会有一个很诡异的情况：数据框中的myFamilyAges变量会比其他变量多一个观测值，这不在是一个完美的矩形。试一试这条命令，看看会有什么发生。这个结果阐明了R是怎样处理类似情况的。

因此我们从这一章学到了哪些新技术新知识呢？下面是本章的总结：





#### 参考资料

[http://en.wikipedia.org/wiki/Central_tendency](http://en.wikipedia.org/wiki/Central_tendency )

[http://en.wikipedia.org/wiki/Median](http://en.wikipedia.org/wiki/Median)

[http://en.wikipedia.org/wiki/Relational_model](http://en.wikipedia.org/wiki/Relational_model )

[http://msenux.redwoods.edu/math/R/dataframe.php](http://msenux.redwoods.edu/math/R/dataframe.php)

[http://stat.ethz.ch/R-manual/R-devel/library/base/html/data.frame.html](http://stat.ethz.ch/R-manual/R-devel/library/base/html/data.frame.html )

[http://www.burns-stat.com/pages/Tutor/hints_R_begin.html](http://www.burns-stat.com/pages/Tutor/hints_R_begin.html)

[http://msenux.redwoods.edu/math/R/dataframe.php](http://msenux.redwoods.edu/math/R/dataframe.php)






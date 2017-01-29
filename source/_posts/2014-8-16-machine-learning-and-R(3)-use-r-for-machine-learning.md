title: 机器学习与R(3)-利用R进行机器学习
date: 2014-08-16 15:35:20
category: 数据科学
tags: [数据科学,机器学习,R,机器学习与R]
---


# R机器学习的包

[R](http://cran.r-project.org/index.html)有着超过了4000多个包，那么哪些与机器学习相关呢？CRAN [Task View](http://cran.r-project.org/web/views/MachineLearning.html)则是你应该关注的地方。

# 安装R包

以下命令是安装weka这个机器学习的R包。

```R
install.packages("RWeka")
```

当然你也可以指定安装的目录

```R
install.packages("RWeka", lib="/path/to/library")
```

如果不懂lib参数是什么，看帮助则是最好选择

```R
?install.packages
```

安装完R包以后要使用它则可以使用

```R
library(RWeka)
#或者
require(RWeka)
```
这个`library`和`require`有什么区别？看帮助;-)



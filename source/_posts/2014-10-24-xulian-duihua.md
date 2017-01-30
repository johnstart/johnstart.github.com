title: 数据分析的视角看香港特区政府与学联对话
date: 2014-10-24 19:55:37
modified: 2014-10-24 19:55:37
category: 数据科学
tags: [数据科学,data science,可视化,R]
---

据新华社电 香港特区政府政务司司长林郑月娥等与学联进行了对话，这里不带任何政治色彩，也不对其进行解读，只是用数据分析的视角将对话内容可视化方式呈现。

+ 数据来源:网上的对话实录
+ 数据处理：将特区政府与学联的话分别分为两组，其余无修改

# 词云分析

![学联](/images/project/duihua/xl_wordcloud.png)

利用R语言的tm包，Rwordseg包对文字进行分词，去掉停止词等，得到上图所示词云。大致看出学联对话主要在于对基本法，候选人，立法会的意见，要求选举权，被选举权，民主制度的时间表，路线图，要求约束力，其它大家可以自己解读...

![特区政府](/images/project/duihua/gf_wordcloud.png)

以上是特区政府对话的词云，基本**基本法**出现频率太高，影响了词云。将**基本法**去掉后，得到如下词云

![特区政府](/images/project/duihua/gf_wordcloud_2.png)

这里大家可以自己解读

# 聚类分析

还是利用R语言对上述对话分析，得到学联的分析如下：

![学联](/images/project/duihua/xl_cluster.png)

可以与前面的词云对比参照。类似特区政府的分析如下：

![特区政府](/images/project/duihua/gf_cluster.png)



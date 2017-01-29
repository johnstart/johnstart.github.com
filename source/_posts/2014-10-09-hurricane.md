title: 2010~2014 飓风可视化尝试
date: 2014-10-09 22:35:37
category: 数据科学
tags: [数据科学,data science,可视化,R]
---

最近对数据可视化进行了一些研究学习，下面是参考[Gaston Sanchez](http://www.gastonsanchez.com)之前对2009～2010年飓风的可视化方法对2010~2014发生的飓风进行了一个可视化尝试，采用R实现。

# 数据获取

[International Best Track Archive for Climate Stewardship](http://www.ncdc.noaa.gov/oa/ibtracs/index.php?name=wmo-data)网站会收集飓风信息数据，我们这里选择下载csv格式数据。


```r
setwd("d:/project/datascience/hurricane/")
row.names=c("Serial_Num","Season","Num","Basin","Sub_basin","Name",
            "ISO_time","Nature","Latitude","Longitude","Wind.WMO","Pres.WMO",
            "Center","Wind.WMO.Percentile","Pres.WMO.Percentile","Track_type")
hurricane=read.csv("Allstorms.ibtracs_wmo.v03r06.csv",skip=3,
                   header=F,stringsAsFactors=F)
names(hurricane)=row.names
head(hurricane)
```

```
##      Serial_Num Season Num Basin Sub_basin       Name            ISO_time
## 1 1848011S09080   1848   2    SI        MM XXXX848003 1848-01-11 06:00:00
## 2 1848011S09080   1848   2    SI        MM XXXX848003 1848-01-12 06:00:00
## 3 1848011S09080   1848   2    SI        MM XXXX848003 1848-01-13 06:00:00
## 4 1848011S09080   1848   2    SI        MM XXXX848003 1848-01-14 06:00:00
## 5 1848011S09080   1848   2    SI        MM XXXX848003 1848-01-15 06:00:00
## 6 1848011S09080   1848   2    SI        MM XXXX848003 1848-01-16 06:00:00
##   Nature Latitude Longitude Wind.WMO Pres.WMO  Center Wind.WMO.Percentile
## 1     NR     -8.6      79.8        0        0 reunion                -100
## 2     NR     -9.0      78.9        0        0 reunion                -100
## 3     NR    -10.4      73.2        0        0 reunion                -100
## 4     NR    -12.8      69.9        0        0 reunion                -100
## 5     NR    -13.9      68.9        0        0 reunion                -100
## 6     NR    -15.3      67.7        0        0 reunion                -100
##   Pres.WMO.Percentile Track_type
## 1                -100       main
## 2                -100       main
## 3                -100       main
## 4                -100       main
## 5                -100       main
## 6                -100       main
```

```r
str(hurricane)
```

```
## 'data.frame':	189855 obs. of  16 variables:
##  $ Serial_Num         : chr  "1848011S09080" "1848011S09080" "1848011S09080" "1848011S09080" ...
##  $ Season             : int  1848 1848 1848 1848 1848 1848 1848 1848 1848 1848 ...
##  $ Num                : int  2 2 2 2 2 2 2 2 2 2 ...
##  $ Basin              : chr  " SI" " SI" " SI" " SI" ...
##  $ Sub_basin          : chr  " MM" " MM" " MM" " MM" ...
##  $ Name               : chr  "XXXX848003" "XXXX848003" "XXXX848003" "XXXX848003" ...
##  $ ISO_time           : chr  "1848-01-11 06:00:00" "1848-01-12 06:00:00" "1848-01-13 06:00:00" "1848-01-14 06:00:00" ...
##  $ Nature             : chr  " NR" " NR" " NR" " NR" ...
##  $ Latitude           : num  -8.6 -9 -10.4 -12.8 -13.9 -15.3 -16.5 -18 -20.6 -22.8 ...
##  $ Longitude          : num  79.8 78.9 73.2 69.9 68.9 67.7 67 67.4 69.8 72 ...
##  $ Wind.WMO           : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ Pres.WMO           : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ Center             : chr  "reunion" "reunion" "reunion" "reunion" ...
##  $ Wind.WMO.Percentile: num  -100 -100 -100 -100 -100 -100 -100 -100 -100 -100 ...
##  $ Pres.WMO.Percentile: num  -100 -100 -100 -100 -100 -100 -100 -100 -100 -100 ...
##  $ Track_type         : chr  "main" "main" "main" "main" ...
```

# 预处理数据

通过对数据大致了解，我们知道Season实际也对应于年，即Season为1848就代表了该行对应1848。这里取2010年以后数据进行分析


```r
require(dplyr)
```

```r
hurricane.df=tbl_df(hurricane)
hurricane.after.2010=hurricane.df%>%filter(Season>=2010)
hurricane.after.2010
```

```
## Source: local data frame [12,324 x 16]
## 
##       Serial_Num Season Num Basin Sub_basin Name            ISO_time
## 1  2009317S10073   2010   1    SI        MM ANJA 2009-11-13 06:00:00
## 2  2009317S10073   2010   1    SI        MM ANJA 2009-11-13 12:00:00
## 3  2009317S10073   2010   1    SI        MM ANJA 2009-11-13 18:00:00
## 4  2009317S10073   2010   1    SI        MM ANJA 2009-11-14 00:00:00
## 5  2009317S10073   2010   1    SI        MM ANJA 2009-11-14 06:00:00
## 6  2009317S10073   2010   1    SI        MM ANJA 2009-11-14 12:00:00
## 7  2009317S10073   2010   1    SI        MM ANJA 2009-11-14 18:00:00
## 8  2009317S10073   2010   1    SI        MM ANJA 2009-11-15 00:00:00
## 9  2009317S10073   2010   1    SI        MM ANJA 2009-11-15 06:00:00
## 10 2009317S10073   2010   1    SI        MM ANJA 2009-11-15 12:00:00
## ..           ...    ... ...   ...       ...  ...                 ...
## Variables not shown: Nature (chr), Latitude (dbl), Longitude (dbl),
##   Wind.WMO (dbl), Pres.WMO (dbl), Center (chr), Wind.WMO.Percentile (dbl),
##   Pres.WMO.Percentile (dbl), Track_type (chr)
```

```r
dim(hurricane.after.2010)
```

```
## [1] 12324    16
```

这里的ISO_time同时包含了年月日以及时间，我们这里先把月份给单独取出来。


```r
#拆分
date.time=strsplit(hurricane.after.2010$ISO_time," ")
#只取日期
iso.date=unlist(lapply(date.time,function(x) x[1]))
#取月份
iso.month=substr(iso.date,6,7)
hurricane.after.2010$Month=iso.month
```

对于没有命名的飓风一般都不是特别厉害，这里就不考虑了


```r
hurricane.after.2010=hurricane.after.2010%>%filter(Name!="NAMED" & Name!="NOT NAMED")
```

# 准备绘图


```r
require(ggplot2)
```

```
## Loading required package: ggplot2
```

```r
require(ggthemes)
```

```
## Loading required package: ggthemes
```

```
## Warning: package 'ggthemes' was built under R version 3.1.1
```

```r
# 我们再给飓风加上一个ID，以飓风的Name+Season来命名
# 这样后面我们可以获得飓风的路径用来绘图
hurricane.after.2010$ID = as.factor(paste(hurricane.after.2010$Name, 
                                          hurricane.after.2010$Season, sep = "."))

# 将飓风Name转换为factor
hurricane.after.2010$Name = as.factor(hurricane.after.2010$Name)

# 经过试验发现其他地区数据有些错误，所以只使用NA与EP区域
hurricane.after.2010$Basin=gsub("^ ", "", hurricane.after.2010$Basin)
hurricane.after.2010=hurricane.after.2010%>%filter(Basin=="NA"|Basin=="EP")
```

现在一切就绪开始绘图


```r
x.range=range(hurricane.after.2010$Longitude)
y.range=range(hurricane.after.2010$Latitude)
hurricane.map = ggplot(hurricane.after.2010, 
                       aes(x = Longitude, y = Latitude, group = ID)) + 
  geom_polygon(data = map_data("world"), aes(x = long, y = lat, group = group), 
               fill = "gray25", colour = "gray10", size = 0.2) + 
  geom_path(data = hurricane.after.2010, aes(group = ID, colour = Wind.WMO), 
            alpha = 0.5, size = 0.8) + 
  xlim(x.range) + 
  ylim(y.range) + 
  labs(x = "", y = "")+
  theme(panel.background = element_rect(fill = "gray10", colour = "gray30"),      
       axis.text.x = element_blank(), 
       axis.text.y = element_blank(),
       axis.ticks = element_blank(), 
       panel.grid.major = element_blank(), 
       panel.grid.minor = element_blank())+
  ggtitle(label = "飓风 2010 - 2014")


hurricane.map
```

![飓风2010～2014](/img/project/hurricane/hurricane.png)

按月绘制


```r
hurricane.map.month = ggplot(hurricane.after.2010, 
                       aes(x = Longitude, y = Latitude, group = ID)) + 
  geom_polygon(data = map_data("world"), aes(x = long, y = lat, group = group), 
               fill = "gray25", colour = "gray10", size = 0.2) + 
  geom_path(data = hurricane.after.2010, aes(group = ID, colour = Wind.WMO), 
            alpha = 0.5, size = 0.8) + 
  xlim(x.range) + 
  ylim(y.range) + 
  labs(x = "", y = "")+
  facet_wrap(~Month)+
  theme(panel.background = element_rect(fill = "gray10", colour = "gray30"),      
       axis.text.x = element_blank(), 
       axis.text.y = element_blank(),
       axis.ticks = element_blank(), 
       panel.grid.major = element_blank(), 
       panel.grid.minor = element_blank())+
  ggtitle(label = "飓风 2010 - 2014/月")

hurricane.map.month
```
![飓风2010～2014 月度趋势图](/img/project/hurricane/hurricane-month.png)

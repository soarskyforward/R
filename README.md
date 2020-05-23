# R
基本语法练习

> 获取帮助 `help.start()` `?foo` `example(foo)`  

#### 包的安装和使用
```
library() # 显示库中的包
install.library("gclus") #安装新的包
update.package("gclus") #更新包
library("gclus") #使用包
```

#### 工作空间
```
save.image("myfile") #保存工作空间到文件myfile中
load("myfile") #读取工作空间
```

#### 输入与输出
```
source("filename") #在当前对话执行一个脚本
sink("filename")  #将输出重定向到文件filename中
pdf("filename.pdf") #图形输出
jpeg("filename.jpeg")
```

#### 创建数据集
> 不同的行业对于数据集的行和列叫法不同。统计学家称它们为观测（observation）和变量
（variable），数据库分析师则称其为记录（record）和字段（field），数据挖掘和机器学习学科的研
究者则把它们叫作示例（example）和属性（attribute）。

##### 数据结构

R中的数据结构一般为标量、向量、矩阵、数组、数据框和列表。
1.向量
```
x <- c(1,2,3) #数值型向量
y <- c("one", "two")   #字符型向量
z <- c(TRUE)  #逻辑性向量，此例为标量
```
2.矩阵
```
x <- matrix(1:15, nrow = 3, ncol = 5, byrow = TRUE) #按行填充
y <- matrix(1:15, nrow = 3, ncol = 5, byrow = FALSE) #按列填充
```
3.数组
>与矩阵类似，但是维度可以大于2  

` z <- array(1:24, c(2, 3, 4)`

4.数据框
```
patientID <- c(1, 2, 3, 4)
age <- c(25, 34, 28, 52)
diabetes <- c("Type1", "Type2", "Type1", "Type1")
patientdata <- data.frame(patientID, age, diabetes)
```

使用函数attach()和detach()或单独使用函数with()来简化代码。
```
summary(mtcars$mpg)
plot(mtcars$mpg, mtcars$disp)
plot(mtcars$mpg, mtcars$wt)

#函数attach()可将数据框添加到R的搜索路径中
attach(mtcars)
 summary(mpg)
 plot(mpg, disp)
 plot(mpg, wt)
detach(mtcars)
#函数detach()将数据框从搜索路径中移除

with(mtcars, {
 print(summary(mpg))
 plot(mpg, disp)
 plot(mpg, wt)
})
```
5.因子
>变量可归结为名义型、有序型或连续型变量, 而类别（名义型）变量和有序类别（有序型）变量在R中称为因子（factor）
```
status <- c("Poor", "Improved", "Excellent", "Poor")
status <- factor(status, order = TRUE,
          levels = c(c("Poor", "Improved", "Excellent"))
```

6.列表
>一般来说，列表就是一些对象（或成分，
component）的有序集合。列表允许你整合若干（可能无关的）对象到单个对象名下。
```
g <- "My First List"
h <- c(25, 26, 18, 39)
j <- matrix(1:10, nrow=5)
k <- c("one", "two", "three")
mylist <- list(title=g, ages=h, j, k)
```

#### 数据的输入
```
fix(mydata) #键盘输入数据
mydata <- edit(mydata)

#从csv文件中导入数据
grades <- read.table("studentgrades.csv", header=TRUE,
 row.names="StudentID", sep=",")

#导入SPSS数据
library("Hmisc")
mydataframe <- spss.get("mydata.sav", use.value.labels=TRUE)  #use.value.labels=TRUE表示让函数将带有值标签的变量导入为R中水平对应相同的因子

```


#### 创建新变量
```
mydata<-data.frame(x1 = c(2, 2, 6, 4),
x2 = c(3, 4, 2, 8))
mydata <- transform(mydata, sumx = x1 + x2,
          meanx = (x1 + x2)/2)
#OR
attach(mydata)
mydata$sumx <- x1 + x2
mydata$meanx <- (x1 + x2)/2
detach(mydata)
```

#### 处理缺失值
```
leadership$age[leadership$age == 99] <- NA
```

#### 日期处理   
%d 日期 %m 月份 %Y 年份 %a 星期
```
as.Date(x,"input_format")
as.Character(dates)
```
#### 判别数据类型
`is.datatype()`

#### 数据排序
`newdata <-leadership[order(gender, -age),] `

#### 数据集合并
```
total <- merge(dataframeA, dataframeB, by = "ID")   #横向
total <- rbind(dataframeA, dataframeB) 	#纵向
```
#### 选入观测
```
newdata <- subset(leadership, age >= 35 | age < 24,)
select=c(q1, q2, q3, q4))
 ```
#### 使用SQL语句操纵数据框
```
library("sqldf")

#参数row.names=TRUE将原始数据框中的行名延续到了新数据框中
newdf <- sqldf("select * from mtcars where carb = 1 order by mpg", row.name = TRUE)
```

#### 基本绘图
```
dose <- c(20, 30, 40, 45, 60)
drugA <- c(16, 20, 27, 40, 60)

#在图形上添加标题（main）、副标题（sub）、坐标轴标
签（xlab、ylab）并指定了坐标轴范围（xlim、ylim）。
plot(dose, drugA, type="b",
 col="red", lty=2, pch=2, lwd=2,
 main="Clinical Trials for Drug A",
 sub="This is hypothetical data",
 xlab="Dosage", ylab="Drug Response",
 xlim=c(0, 60), ylim=c(0, 70))

 #图例
 legend("topleft", title="Drug Type", c("A","B")
 lty=c(1, 2), pch=c(15, 17), col=c("red", "blue"))
 ```

 简单的条形图
 ```
library(vcd)
counts <- table(Arthritis$Improved)
#普通条形图
 barplot(counts, main = "Simple Bar Plot",xlab="Improvement", ylab="Frequency")
#水平条形图
 barplot(counts, main="Horizontal Bar Plot",
 xlab="Frequency", ylab="Improvement",
 horiz=TRUE)

 counts<-table(Arthritis$Improved,Arthritis$Treatment)
#分组条形图
 barplot(counts,main="Grouped Bar Plot",
 xlab="Treatment", ylab="Frequency",
 col=c("red", "yellow", "green"),
 legend=rownames(counts), beside=TRUE)
 ```

 饼图
 ```
 slices <- c(10, 12,4, 16, 8)
lbls <- c("US", "UK", "Australia", "Germany", "France")
pie(slices, labels = lbls, main="Simple Pie Chart")
 ```

 直方图
 ```
 #参数freq=FALSE表示根据概率密度而不是频数绘制图形。参数breaks用于控制组的数量。
 hist(mtcars$mpg,
 freq=FALSE,
 breaks=12,
 col="red",
 xlab="Miles Per Gallon",
 main="Histogram, rug plot, density curve")
#添加轴须图
 lines(density(mtcars$mpg), col="blue", lwd=2)
 ```

 箱线图
 ```
 boxplot(mtcars$mpg, main="Box plot", ylab="Miles per Gallon")
 ```

 点图
 ```
 #cex控制标签的大小
 dotchart(mtcars$mpg, labels=row.names(mtcars), cex=.7,
 main="Gas Mileage for Car Models",
 xlab="Miles Per Gallon")
 ```


## 基本统计分析

#### 描述性统计
```
#通过summary()计算描述性统计量
myvars <- c("mpg", "hp", "wt")
summary(mtcars[myvars])


#通过sapply()计算描述性统计量
#自建函数mystats()
mystats <- function(x, na.omit=FALSE){
if (na.omit)
x <- x[!is.na(x)]
m <- mean(x)
n <- length(x)
s <- sd(x)
skew <- sum((x-m)^3/s^3)/n
kurt <- sum((x-m)^4/s^4)/n - 3
return(c(n=n, mean=m, stdev=s, skew=skew, kurtosis=kurt))
}
sapply(mtcars[myvars], mystats)


#通过Hmisc包中的describe()函数计算描述性统计量
library(Hmisc)
describe(mtcars[myvars])
```

使用aggregate()函数来分组获取描述性统计量
```
myvars <- c("mpg", "hp", "wt")
#计算每组的均值
aggregate(mtcars[myvars], by=list(am=mtcars$am), mean)
#计算每组的标准差
aggregate(mtcars[myvars], by=list(am=mtcars$am), sd)
```
使用psych包中的describeBy()分组计算概述统计量
```
library(psych)
myvars <- c("mpg", "hp", "wt")
describeBy(mtcars[myvars], list(am = mtcars$am))
```

#### 频数表和列联表

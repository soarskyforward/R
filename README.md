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
> 不同的行业对于数据集的行和列叫法不同。统计学家称它们为观测（observation）和变量（variable），数据库分析师则称其为记录（record）和字段（field），数据挖掘和机器学习学科的研究者则把它们叫作示例（example）和属性（attribute）。

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

散点图
```
plot(wt, mpg,
 main="Basic Scatter plot of MPG vs. Weight",
 xlab="Car Weight (lbs/1000)",
 ylab="Miles Per Gallon ", pch=19)
 #abline()函数用来添加最佳拟合的线性直线，而lowess()函数则用来添加一条平滑曲线
abline(lm(mpg~wt), col="red", lwd=2, lty=1)
lines(lowess(wt,mpg), col="blue", lwd=2, lty=2)

#car包中的scatterplot()函数增强了散点图的许多功能，它可以很方便地绘制散点图
#表达式mpg ~ wt | cyl表示按条件绘图（即按cyl的水平分别绘制mpg和wt的关系图）
library(car)
scatterplot(mpg ~ wt | cyl, data=mtcars, lwd=2, span=0.75,
 main="Scatter Plot of MPG vs. Weight by # Cylinders",
 xlab="Weight of Car (lbs/1000)",
 ylab="Miles Per Gallon",
 legend.plot=TRUE,
 id.method="identify",
 labels=row.names(mtcars),
 boxplots="xy"
)
```

折线图
>p 只有点
l 只有线
o 实心点和线（即线覆盖在点上）
b、c 线连接点（c 时不绘制点）
s、S 阶梯线
h 直方图式的垂直线
n 不生成任何点和线（通常用来为后面的命令创建坐标轴）

```
opar <- par(no.readonly=TRUE)
par(mfrow=c(1,2))
t1 <- subset(Orange, Tree==1)
plot(t1$age, t1$circumference,
 xlab="Age (days)",
 ylab="Circumference (mm)",
 main="Orange Tree 1 Growth")
plot(t1$age, t1$circumference,
 xlab="Age (days)",
 ylab="Circumference (mm)",
 main="Orange Tree 1 Growth",
 type="b")
par(opar)

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
一维列联表
```
options(digits = 3)
library(vcd)
mytable <- with(Arthritis, table(Improved))

#prop.table()将这些频数转化为比例值
prop.table(mytable)
```

二维列联表
```
#A是行变量，B是列变量
mytable <- table(A, B)

#xtabs()函数还可使用公式风格的输入创建列联表,要进行交叉分类的变量应出现在公式的右侧（即
~符号的右方)
mytable <- xtabs(~ Treatment + Improved, data = Arthritis)

#使用margin.table()和prop.table()函数分别生成边际频数和比例
#下标1指代table()语句中的第一个变量
prop.table(mytable, 1)
margin.table(mytable, 2)

#可以使用addmargins()函数为这些表格添加边际和
addmargins(mytable)
```

使用CrossTable生成二维列联表
```
library(gmodels)
CrossTable(Arthritis$Treatment, Arthritis$Improved)
```

#### 独立性检验
```
#使用chisq.test()函数对二维表的行变量和列变量进行卡方独立性检验
library(vcd)
mytable <- xtabs(~Treatment + Improved, data = Arthritis)
chisq.test(mytable)
#在结果中，患者接受的治疗和改善的水平看上去存在着某种关系（p<0.01）,所以拒绝了治疗类型和治疗结果相互独立的原假
设。

#使用fisher.test()函数进行Fisher精确检验
 mytable <- xtabs(~Treatment+Improved, data=Arthritis)
 fisher.test(mytable)

#mantelhaen.test()函数可用来进行Cochran-Mantel-Haenszel卡方检验
 mytable <- xtabs(~Treatment+Improved+Sex, data=Arthritis)
mantelhaen.test(mytable)

#拒绝变量间相互独立的原假设后进行相关性的度量
library(vcd)
 mytable <- xtabs(~Treatment+Improved, data=Arthritis)
assocstats(mytable)
```
#### 相关
 Pearson、Spearman和Kendall相关
 >Pearson积差相关系数衡量了两个定量变量之间的线性相关程度。Spearman等级相关系数则衡量分级定序变量之间的相关程度。Kendall’s Tau相关系数也是一种非参数的等级相关度量。

```
#cor()函数可以计算这三种相关系数,默认参数为use="everything"和method="pearson";而cov()函数可用来计算协方差
 states<- state.x77[,1:6]
 cor(states)
 cov(states)


 #在默认情况下得到的结果是一个方阵（所有变量之间两两计算相关）。你同样可以计算非方形的相关矩阵。
x <- states[,c("Population", "Income", "Illiteracy", "HS Grad")]
y <- states[,c("Life Exp", "Murder")]
cor(x,y)
```
相关性的显著性检验
```
#alternative则用来指定进行双侧检验或单侧检验（取值为"two.side"、"less"或"greater"），而method用以指定要计算的相关类型（"pearson"、
"kendall" 或 "spearman" ）
cor.test(x, y, alternative = , method = )
```

#### t 检验
```
#独立样本的 t 检验
t.test(y ~ x, data)

#非独立样本的 t 检验
t.test(y ~ x, paired = TRUE)
```

### 回归
>从许多方面来看，回归分析都是统计学的核心。它其实是一个广的概念，通指那些用一个或多个预测变量（也称自变量或解释变量）来预测响应变量（也称因变量、效标变量或结果变量）的方法。

用lm()拟合回归模型
```
#formula指要拟合的模型形式，data是一个数据框，包含了用于拟合模型的数据
#表达式（formula）形式如下：Y ~ X1 + X2 + ... + Xk
myfit <- lm(formula, data)
```
- summary() 展示拟合模型的详细结果
- coefficients() 列出拟合模型的模型参数（截距项和斜率）
- confint() 提供模型参数的置信区间（默认 95%）
- fitted() 列出拟合模型的预测值
- residuals() 列出拟合模型的残差值
- anova() 生成一个拟合模型的方差分析表，或者比较两个或更多拟合模型的方差分析表
- vcov() 列出模型参数的协方差矩阵
- AIC() 输出赤池信息统计量
- plot() 生成评价拟合模型的诊断图
- predict() 用拟合模型对新的数据集预测响应变量值

>当回归模型包含一个因变量和一个自变量时，我们称为简单线性回归。当只有一个预测变量，但同时包含变量的幂（比如，X、X2、X3）时，我们称为多项式回归。当有不止一个预测变量时，则称为多元线性回归。

##### 简单线性回归
```
fit <- lm(weight ~ height, data = women)
summary(fit)

plot(women$height, women$weight, xlab ="Height (in inches)",ylab="Weight (in pounds)")
abline(fit)
```
##### 多项式回归
>I函数将括号的内容看作R的一个常规表达式。因为^符号在表达式中有特殊的含义，会调用你并不需要的东西，所以此处须要用这个函数。

```
fit2 <- lm(weight ~ height + I(height ^ 2), data = women)
summary(fit2)

plot(women$height,women$weight,
 xlab="Height (in inches)",
 ylab="Weight (in lbs)")
lines(women$height, fitted(fit2))
```
```
#car包的使用,spread=FALSE选项删除了残差正负均方根在平滑
曲线上的展开和非对称信息。smoother.args=list(lty=2)选项设置loess拟合曲线为虚线。
pch=19选项设置点为实心圆（默认为空心圆）。
library(car)
scatterplot(weight ~ height, data=women,
 spread=FALSE, smoother.args=list(lty=2), pch=19,
 main="Women Age 30-39",
 xlab="Height (inches)",
 ylab="Weight (lbs.)")
```
#### 多元线性回归

```
states <- as.data.frame(state.x77[,c("Murder", "Population","Illiteracy", "Income", "Frost")])

#cor()函数提供了二变量之间的相关系数，car包中scatterplotMatrix()函数则会生成散点图矩阵
cor(states)
scatterplotMatrix(states, spread=FALSE, smoother.args=list(lty=2),
main="Scatter Plot Matrix")

fit <- lm(Murder ~ Population + Illiteracy + Income +Frost, data=states)
summary(fit)
lm(formula=Murder ~ Population + Illiteracy + Income + Frost, data=states)
```
有交互项的多元线性回归
```
 fit <- lm(mpg ~ hp + wt + hp:wt, data=mtcars)
 summary(fit)
 lm(formula = mpg ~ hp + wt + hp:wt, data=mtcars)
```


#### 回归诊断
```
states <- as.data.frame(state.x77[,c("Murder", "Population","Illiteracy", "Income", "Frost")])
fit <- lm(Murder ~ Population + Illiteracy + Income +Frost, data=states)
confint(fit)
```
##### 标准方法
>R基础安装中提供了大量检验回归分析中统计假设的方法。最常见的方法就是对lm()函数
返回的对象使用plot()函数，可以生成评价模型拟合情况的四幅图形。
```
fit <- lm(weight ~ height, data = women)
par(mfrow = c(2,2))
plot(fit)
```
 正态性 当预测变量值固定时，因变量成正态分布，则残差值也应该是一个均值为0的正
态分布。“正态Q-Q图”（Normal Q-Q，右上）是在正态分布对应的值下，标准化残差的概
率图。若满足正态假设，那么图上的点应该落在呈45度角的直线上；若不是如此，那么
就违反了正态性的假设。
 独立性 你无法从这些图中分辨出因变量值是否相互独立，只能从收集的数据中来验证。
上面的例子中，没有任何先验的理由去相信一位女性的体重会影响另外一位女性的体重。
假若你发现数据是从一个家庭抽样得来的，那么可能必须要调整模型独立性的假设。
 线性 若因变量与自变量线性相关，那么残差值与预测（拟合）值就没有任何系统关联。
换句话说，除了白噪声，模型应该包含数据中所有的系统方差。在“残差图与拟合图”
（Residuals vs Fitted，左上）中可以清楚地看到一个曲线关系，这暗示着你可能需要对回
归模型加上一个二次项。
 同方差性 若满足不变方差假设，那么在“位置尺度图”（Scale-Location Graph，左下）
中，水平线周围的点应该随机分布。该图似乎满足此假设。

##### 改进的方法
>car包提供了大量函数，大大增强了拟合和评价回归模型的能力

```
#正态性
library(car)
states <- as.data.frame(state.x77[,c("Murder", "Population",
 "Illiteracy", "Income", "Frost")])
fit <- lm(Murder ~ Population + Illiteracy + Income + Frost, data=states)

#method = "identify"选项能够交互式绘图——待图形绘制后，用鼠标单击图形内的点，将会标注函数中labels选项的设定值
#当simulate=TRUE时，95%的置信区间将会用参数自助法生成。
qqPlot(fit, labels=row.names(states), id.method="identify",
 simulate=TRUE, main="Q-Q Plot")

 #误差的独立性
 #car包提供了一个可做Durbin-Watson检验的函数，能够检测误差的序列相关性
 durbinWatsonTest(fit)

 # 线性
  crPlots(fit)

# 同方差性
 ncvTest(fit)
spreadLevelPlot(fit)
```

#### 方差分析
ANOVA模型拟合
```
#单因素方差分析,以multcomp包中的cholesterol数据集为例
library(multcomp)
attach(cholesterol)
table(trt)
aggregate(response, by = list(trt), FUN = mean)
aggregate(response, by = list(trt), FUN = sd)

#检验组间差异（ANOVA）
fit <- aov(response ~ trt)
summary(fit)

#绘制各组均值及其置信区间的图形
library(gplots)
plotmeans(response ~ trt, xlab="Treatment", ylab="Response",
main="Mean Plot\nwith 95% CI")
```
多重比较
>虽然ANOVA对各疗法的F检验表明五种药物疗法效果不同，但是并没有告诉你哪种疗法与其
他疗法不同。多重比较可以解决这个问题。
```
TukeyHSD(fit)

#第一个par语句用来旋转轴标签，第二个用来增大左边界的面积，
par(las = 2)
par(mar = c(5,8,4,2))
plot(TukeyHSD(fit))
```

单因素协方差分析
>单因素协方差分析（ANCOVA）扩展了单因素方差分析（ANOVA），包含一个或多个定量的
协变量。
```
#例子来自于multcomp包中的litter数据集
attach(litter)
table(dose)
aggregate(weight, by=list(dose), FUN=mean)

fit <- aov(weight ~ gesttime + dose)
summary(fit)
```

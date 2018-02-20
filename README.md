# BP-
本文包括了R语言下的BP网络的实现方式
```R
library(nnet)
data("iris")
set.seed(2)
ind = sample(2,nrow(iris),replace = TRUE,prob = c(0.7,0.3))
trainset = iris[ind == 1,]
testset = iris[ind == 2,]
iris.nn = nnet(Species ~ .,data = trainset,size = 2,rang = 0.1,decay = 5e-4,maxit = 200)
#rang是初始权重的选取范围[-rang,rang]；decay是学习步长；maxit为迭代速度。

#输出训练好的神经网络
summary(iris.nn)

#用训练好的模型预测测试集
iris.predict = predict(iris.nn,testset,type = "class")
 #type参数为class,因此输出的是预测的类标号而非概率矩阵

nn.table = table(testset$Species,iris.predict)
 #用table函数根据预测结果和testset的实际类标号生成分类表

nn.table

#基于分类表得到混淆矩阵/误差矩阵，并对分类结果进行分析
#需要先安装包caret，foreach,car,e1071
library(caret)
confusionMatrix(nn.table)
```

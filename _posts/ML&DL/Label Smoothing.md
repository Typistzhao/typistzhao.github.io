---
layout: post
comments: false
categories: ML&DL
--- 

# Label Smoothing

# Where
> 论文《Rethinking the Inception Architecture for Computer Vision》

# Why
标签平滑，这是一种防止模型过拟合的正则化方法

# How
输出预测结果的时候，对每个类别得到的logits值进行softmax计算，预测属于各个类别的概率  
[0, 0.7, 0.2, 0.1, 0]

计算loss，通过上述的概率值（分布）与真实值（分布）计算交叉熵  
[0, 1, 0, 0, 0]

最小化预测概率与真实概率的交叉熵，可以得到最优的预测概率分布  


预测的结果中，softmax之前，正确的标签值会越来越大（趋于无穷），错误的标签值越来越小（趋于0）   
[0, ∞, 0, 0, 0] 

在训练集中这些情况当然很好，但是这样容易过拟合啊  
因为训练集中受限于数据集大小，不一定就包含了所有情况，这样的话test中出现的新情况，预测值又太绝对，可能模型就没办法预测  
举个不恰当的例子，之前一直吃的肉包子，太绝对的预测会导致模型认为包子只能是肉馅的

label smoothing就是为了解决这样的问题，属于对label进行软化的操作  
[0, 1, 0, 0, 0]-->[0.025, 0.9, 0.025, 0.025, 0.025] 

在真实概率分布之上+噪声分布，噪声分布一般是均匀分布（1/K，K是类别个数）  
除此之外还有一个平滑指数，可信度=1-平滑指数  

平滑指数\*噪声分布 + 可信度\*真实概率分布  

对应的loss也要发生变化，H(q',p)=-E(1...k)(log(p(k)\*q'(k)))=(1-e)\*H(q,p)+e*H(u,p)

# Ref
> [深度学习 | 训练网络trick——label smoothing(附代码)
](https://blog.csdn.net/qiu931110/article/details/86684241)

> [优化策略5 Label Smoothing Regularization_LSR原理分析
](https://blog.csdn.net/lqfarmer/article/details/74276680)
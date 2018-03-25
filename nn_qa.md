神经网络学习算法的Q&A
========================
## 代价函数是如何计算出整个网络的代价的
例如，一张图片作为输入层，28*28个像素点，结果对应0～9个数字，这是如何计算出代价的？  
计算出来的结果是[0.1,0.05,0.5,0.1,0,0.03,0.02,0,0.1,0.1]， 预计出来是[1,0,0,0,0,0,0,0,0,0],也就是0,然后两个向量相减，然后对结果向量做Euclidean norm,也就是求向量的`长度`的平方。  
最终通过统计出样本集合的平均误差  

## 为什么是平方后求均值
绝对值不可微？

## 神经网络的代价函数是如何进行梯度计算的
其实就是如何对一个求和公式做偏导。 

## 网络中所有的权重和阀值的矩阵是什么样的结构

## CrossEntropy
代价函数。弥补学习速率变慢(sigmoid作为激活函数的情况下)。

## Softmax

## 为什么初始化权重的时候一般用正态分布的方式

## MSE为什么要除以2
求导后消除平方

## 参数
https://www.quora.com/How-are-the-cost-functions-for-Neural-Networks-derived

export DYLD_FALLBACK_LIBRARY_PATH=/usr/local/Cellar/opencv/3.3.0_3/lib:$DYLD_FALLBACK_LIBRARY_PATH
export PYTHONPATH=/usr/local/Cellar/opencv/3.3.0_3/lib/python3.6/site-packages:$PYTHONPATH
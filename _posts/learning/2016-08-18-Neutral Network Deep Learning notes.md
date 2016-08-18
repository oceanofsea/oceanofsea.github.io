---
layout: post
title:  "Neutral Network Deep Learning Notes"
categories: Learning
excerpt: 
tags: NeutralNetwork Learning
---

* content
{:toc}

最近看了一些关于这方面的资料，觉得还是蛮感兴趣的。

# Prerequisite

主要是微积分求导，Differentiation is to get the rate of change of one variable respect to another variable.
```dx/dt```, for example, the speed is the rate of change of position respect to time.

从物理的角度来看， 是图上某一点的slope。

## Partial Derivative
If ```a``` directly affects ```c```, then we want to know how it affects ```c```. 
If ```a``` changes a little bit, how does ```c``` change? 
We call this the partial derivative of ```c``` with respect to ```a```.

# Start with Linear Regression

[linear model](/images/lr.png)

在这个图上，我们需要把输出的y转换成概率的形式，原则是正确分类的概率大，错误的分类概率小。
而这个转换成概率的过程叫 **SoftMax**.

```
S(yi) = exp(yi) / sum(exp(yi))
```

还有一个有意思的特点就是当所有的y都乘以10， 这个Softmax的区分能力越强，
而当所有的y都缩小10倍，那Softmax的区分能力越小。

根据这个特点，我们在初始化nn的参数的时候，通常采用高斯分布，然后取峰值小的高斯系数。
因为这样在一开始的时候模型的判断比较uncertain，然后通过不断地学习来调整参数。

而且很多时候我们的分类结果是一个个类别，那么需要做一个转换，这里通常采用的是one-hot encoding，
也就是把结果转换成向量的形式，用1表示某一类，0表示其他类。

那么怎么把Softmax和1-hot label联系起来呢？
cross entropy就解决了这个问题。
```
D(S, L) = -sum(Li(log(Si))
```

注意这里的函数是不对称的，也就是不能把L和S的位置互换。因为在label里很多的0，我们不希望看到log0的情况。

# Hyperparameters

## # of neurons in each layer
Too few neurons will reduce the expression power of the network, but too many will substantially increase the running time and return noisy estimates.

## Learning rate
If it is too high, the neural network will only focus on the last few samples seen and disregard all the experience accumulated before. If it is too low, it will take too long to reach a good state.


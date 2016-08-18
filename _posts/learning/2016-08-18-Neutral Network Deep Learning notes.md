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

# Hyperparameters

## # of neurons in each layer
Too few neurons will reduce the expression power of the network, but too many will substantially increase the running time and return noisy estimates.
## Learning rate
If it is too high, the neural network will only focus on the last few samples seen and disregard all the experience accumulated before. If it is too low, it will take too long to reach a good state.
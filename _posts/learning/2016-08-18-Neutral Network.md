---
layout: post
title:  "Neutral Network"
categories: Learning
excerpt: 
tags: NeutralNetwork Learning
---

* content
{:toc}

# Perceptrons

A perceptron takes several binary inputs, $$ x_1, x_2,…, $$  and produces a single binary output:


<center> <img src = "http://neuralnetworksanddeeplearning.com/images/tikz0.png">
<br> Figure from <a href="http://neuralnetworksanddeeplearning.com/chap1.html
">here</a>
</center>



To make a decision, each input has a weight, and the weighted sum is compared with a threshold, also called bias **b**.

$$output=\begin{cases}
0 & if~\sum_j w_j*x_j \le threshold \\[2ex] 
1 & if~\sum_j w_j*x_j > threshold\end{cases}
$$

To simplify the representation, we can rewrite the above fomula as

$$output=\begin{cases}
0 & if~w*x+b\le 0 \\[2ex] 
1 & if~w*x+b>0\end{cases}
$$

## An example of more complex network
This network is composed of many layers of perceptrons. 

<center> <img src = "http://neuralnetworksanddeeplearning.com/images/tikz1.png" width="500">
<br> Figure from <a href="http://neuralnetworksanddeeplearning.com/chap1.html
">here</a>
</center>

# Sigmoid neurons

Changing the weight or bias a little may flip the output, so it's hard for us to adjust the parameter and to get closer to our expectation.

Sigmoid neurons are similar to perceptrons, but modified so that small changes in their weights and bias cause only a small change in their output. That's the crucial fact which will allow a network of sigmoid neurons to learn.

* Input: $$ x_1, x_2, ... $$, can be any value from 0 to 1
* Output: $$ \sigma(w*x+b) $$, $$ \sigma $$ is called the sigmoid function, and is defined by:

$$
\sigma(z)\equiv \frac{1}{1+e^{-z}}
$$

## Why to use this

The sigmoid function shape is smooth while the step function is not. So that changing the parameter can help influence the training results.

<img src="/images/posts/sigmoid.png" height="200"> <img src="/images/posts/step.png" height="200">

# The architecture of neural networks

For example, suppose we're trying to determine whether a handwritten image depicts a "9" or not. Suppose the image is a 64 by 64 greyscale image.

* Input layer: 64×64 input neurons, with the intensities scaled appropriately between 0 and 1. 
* Output layer: a single neuron, with output values of less than 0.5 indicating "input image is not a 9", and values greater than 0.50.5 indicating "input image is a 9 ".
* Hidden layer: there are many heuristics for different applications.



<center> <img src = "http://neuralnetworksanddeeplearning.com/images/tikz11.png" width="500">
<br> Figure from <a href="http://neuralnetworksanddeeplearning.com/chap1.html
">here</a>
</center>

# Learning with gradient descent

## Cost Function
Assume x is the input, y(x) is the output of neural network, 

$$
C(w,b)\equiv \frac{1}{2n}\sum_{x}||y(x)-a||^2
$$

Here, w denotes the collection of all weights in the network, b all the biases, n is the total number of training inputs, a is the vector of outputs from the network when x is input, and the sum is over all training inputs, x. Of course, the output a depends on x, w and b.

The task is to find a set of weights and biases which make the cost as small as possible. We'll do that using an algorithm known as **gradient descent**.

## why use quadratic cost instead of accuracy?

The number of images correctly classified is not a smooth function of the weights and biases in the network. For the most part, making small changes to the weights and biases won't cause any change at all in the number of training images classified correctly. That makes it difficult to figure out how to change the weights and biases to get improved performance. 

If we instead use a smooth cost function like the quadratic cost it turns out to be easy to figure out how to make small changes in the weights and biases so as to get an improvement in the cost. 

That's why we focus first on minimizing the quadratic cost, and only after that will we examine the classification accuracy.

# Reference

* http://neuralnetworksanddeeplearning.com/chap1.html




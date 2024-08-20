---
layout:     post
title:      Deep Learning for Computer Vision Lecture 2
subtitle:   DLCV
date:       2024-8-20 10:57:52 +0800
author:     czx
toc:        true
math:       true
mermaid:    true
catalog:    true
categories: ['Course Notes', 'Deep Learning for Computer Vision ( COMME5052 )']
comments:   true
tags:
  - Deep Learning
  - Lecture Note
  - Computer Science
---

# 2022 Deep Learning for Computer Vision Lecture2

In this lecture, we will take a glance to Neural Networks & CNN. Let's get started.

## Recap: Linear Regression

Firstly we can recap the linear regression part. Let’s take the input image as $x$, and the linear classifier as $W$.
We need$ y = Wx + b$ as a 10-dimensional output vector, indicating the score for each class.

Our computational Graph looks like:

![image-20240820103514749](https://s2.loli.net/2024/08/20/HXpMCaKFtNGke2D.png)

We have mentioned before, final result of $y$ is a dot product of a row in $W$ and a column in $x$. This is a kind of "average". The $W$ has 10 rows, project to 10 types of final result. In this case, we can see the row as the average of a class.

![image-20240820103851896](https://s2.loli.net/2024/08/20/dIFt9CTQ1B5mbkJ.png)

However, when using linear regression on image classification, we find problems. When the \# of class increase, comes:

* intra-class variation $\uparrow$
* inter-class variation $\downarrow$

So how to deal with this problem?

## Biological neuron and Perceptrons

![image-20240820104044471](https://s2.loli.net/2024/08/20/iEAd8qztcxUrPkv.png)

According to the concept of biological neuron, we may think that we can feed the input to a "neuron" in our computer. This neuron is, maybe, a function. We call it **activate function**.

And, choose what function as our activate function? we just use `sigmiod` function as it. This is a non-linear activate function. The form of sigmoid function is:


$$
\sigma(t) = \frac {1} {1 + e^{-t}}
$$


Mind that when $t \rightarrow \infty$, $\sigma(t) \rightarrow 1$, when $t \rightarrow -\infty$, $\sigma(t) \rightarrow 0$. It looks like a kind of *probability*!

Our model now can be written as:


$$
y = \sigma(W^Tx + b)
$$


and we just need to learn $W$.

## Not fast, but deep

![image-20240820105141512](https://s2.loli.net/2024/08/20/JAY34268exkXv7B.png)

We also want to learn again and again. This means that we need lots of layers! `Multi-Layer Perception/Neural Network` comes...

We use input $x$, and $x$ goes through hidden layer $1, 2, ... n$, till final output layer $y$.

This is so called **deep**.

## Hierarchical Representation Learning

> Successive model layers learn deeper intermediate representations.

For example, we want to learn faces. Use 3 layers:

![image-20240820105335621](https://s2.loli.net/2024/08/20/OK4qZ5RxGcA8QrP.png)

* Layer 1 is the nearest to our input. We can visualize it below. It seems that the layer 1 have already learnt some "direction feature". Maybe, the eyes direction.
* Layer 2 use layer 1 as its input. Now we can find that it learnt some `local information`! Eyes, noses, and so on.
* Layer 3 has the hole faces information.

It is clearly that, when our model goes to deep, every layer can learn some feature. And to be more concise, the feature becomes more and more complex. From `direction` to `faces`.

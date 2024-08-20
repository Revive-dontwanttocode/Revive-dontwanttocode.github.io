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

In this lecture, we will glance at Neural Networks & CNN. Let's get started.

## Recap: Linear Regression

Firstly we can recap the linear regression part. Let’s take the input image as $x$, and the linear classifier as $W$.
We need$ y = Wx + b$ as a 10-dimensional output vector, indicating the score for each class.

Our computational Graph looks like this:

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

## Multi-layer Perceptron: A Nonlinear Classifier

Now let's see an example of Multi-layer Perceptron.

![image-20240820163006403](https://s2.loli.net/2024/08/20/bNXCKBqUPWGsTta.png)

We have input left and output right, with hidden units. When learning, we need to learn two sets of parameters:

* $w_{MD}^{(1)} ... w_{10}^{(1)}$: inputs $x_1, ... ,x_D$ to hidden layer.
* $w_{KM}^{(2)}, ..., w_{10}^(2)$: hidden layer to outputs.

So there comes a question: **where shall we put our activation function on?**

Ofcourse we need to put the activation functions each layer! Mind that in every layer, we actually do $W^{(1)^T}x + b$. If we were not going to add activate function on each layer, we may have:


$$
W^{(2)^T}(W^{(1)^T}x + b_1) + b_2
$$


actually, this can not use the advantage that we design two layers. Just one layer can also fulfill this form! Otherwise, with activation function added to each layer, we get:


$$
\text{output} = \sigma(W^{(2)^T}\sigma((W^{(1)^T}x + b_1)) + b_2)
$$


This is useful when learning.

## Take a closer look...

Let's take a neuron for an example.

![image-20240820164103287](https://s2.loli.net/2024/08/20/BXfy1NtQcjneKFq.png)

For this neuron, we have:

* input: $z_1, z_2, ..., z_D$. A $D$-dimension vector.
* output: a real number $x$.
* parameters to be learnt: $w_0, w_1, ..., w_D$

### Input-Output Function of a Single Neuron

For a single neuron, let's take a look.

Assume that:


$$
x(z_1,z_2) = \frac {1} {1 + \exp(-w_1z_1 - w_2z_2)}
$$


and set parameter $w = (0, 1)$



we can see that in this case, only input $z_2$ works. And our output value like:

![image-20240820164423814](https://s2.loli.net/2024/08/20/IO5MYL2bcwglSsq.png)

If we change $w = (0.2, 1)$, we have:

![image-20240820165306493](https://s2.loli.net/2024/08/20/fyDBPONKjlkJQWg.png)

It rotates. and so on, so forth. And if we add constrains like $|w| \leq k$, we can set other boundaries:

* $w / |w|$ sets direction of boundary.
* $|w|$ sets steepness of boundary.

## Training a Single Neuron

After all, our target is training and predicting. So it's time to see how to train a single neuron.

Firstly, we shall need to define the input and output.

* input: $\mathbf{z} = (z_1, z_2)$
* output: $\mathbf{x} = 1,0$

Next, our model is a linear model, so we can set model parameters:

* $w = (w_1, w_2)$

Third part is the `objective function`. In machine learning or deep learning, we always have our loss function. In this classification function, we can choose **BCE** loss, aka, Binary Cross Entropy.

Our objective function is:


$$
G(\mathbf{w}) = -\sum_n [t^{(n)}\log x(z^{(n)}; w) + (1 - t^n)\log (1 - x(z^{(n)}; w))]
$$


where $t^{(n)}$ is our classification indicator.



How do we learn the model $w$? use gradient decent to help.

![image-20240820170139615](https://s2.loli.net/2024/08/20/Cf46enxaDQGJjkY.png)

## Overfitting and Weight Decay

We, always face overfitting problem. If we want to cut overfitting, we shall add some "regulization".

Let's change our optimization constrains:

![image-20240820170307076](https://s2.loli.net/2024/08/20/A9MC5Jr4FsczBES.png)

This method is called `weight decay` method. It will shrinks weights towards zero, so the model will not be too complicated, and we can avoid several overfitting problem.

### L1-norm and L2-norm

There are two constrains we often use. L1-norm and L2-norm.

L1-norm constrain is:


$$
||w||_1 \rightarrow \sum |w_i|
$$


and L2-norm constrain is:


$$
||w||_2 \rightarrow \sum w_i^2
$$


In particular, L1 norm will make *sparse* situation. Means that it will force some $w_i$ directly to **zero**. However, L2 norm will make *average* situation. Means that it will let $w_i$ be **similar and smaller**.

![image-20240820170720275](https://s2.loli.net/2024/08/20/1fpFtXTRi6ZhYqA.png)

## Training a Neural Network with a Single Hidden Layer

Networks with hidden layers can be fit using gradient descent using an algorithm called **back-propagation**.

![image-20240820170934648](https://s2.loli.net/2024/08/20/x8JwGgNsAc67QpD.png)

Mind that if we want to use regulization methods, we need to add each layer the constrain.(L2-norm)

## Convolutional Neural Networks

Now, we can talk about CNN-Convolutional Neural Networks.

### If, Multi-Layer Perceptron on images?

![image-20240820171142564](https://s2.loli.net/2024/08/20/RIkoG3fwPNlMqOe.png)

Too many weights we need to use for a neuron.

To handle this, CNN use two methods:

1. Local Connectivity: Each neuron takes info only from a neighbourhood of pixels.
2. Weight Sharing: Neurons connecting all neighbourhoods have identical weights.

![image-20240820171720638](https://s2.loli.net/2024/08/20/dymMVvuDcN5Bezl.png)

<center>Local Connectivity</center>

![image-20240820171748684](https://s2.loli.net/2024/08/20/vh1CP8XyNBTbzVQ.png)

<center>Weight Sharing</center>


---
layout:     post
title:      Deep Learning for Computer Vision Lecture 1
subtitle:   DLCV
date:       2024-8-12 23:37:40 +0800
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

# 2022 Deep Learning for Computer Vision Lecture1

In this lecture, We shall review some basic machine learning concepts. And, some techniques.

![image-20240812235830962](https://s2.loli.net/2024/08/12/1bc6QloavSUVCKY.png)

## Example: Testing of COVID-19

When testing of COVID-19, we will surely always meet positive/negative test results. And we of course can get two distribution of positive/negative results. The further away from each other, the better.

![image-20240813000928996](https://s2.loli.net/2024/08/13/nH9zi2V5Uxd3mkq.png)

Mind that in case below, we get:

* One distribution is about TP(True Positive, aka "test right and surely right") and FN(False Negative, aka "test wrong but surely right"). These two indicators are all connected with **positive for COVID distribution**.
* Another distribution is  about Negative for COVID. One is FP(False Positive, "test false but surely true"), another is TN(True Negative, "test the people do not get COVID, and in fact so do him/her").

## Bayesian Decision Theory

Now we shall talk about Bayesian Decision Theory. This is the pivot of our decision system. First and foremost, let's take a classification/detection task as our example.

Assume we have a 2-class classification task. See if a student would *pass* or *fail* the course of DLCV, with a probabilistic variable $\omega$. ($\omega = \omega_1$ for pass, and $\omega = \omega_2$ for fail)

### Prior Probability

> A priority or prior probability reflects the knowledge of how likely we expect a certain state of nature before observation.
{: .prompt-danger }

For example, we have $P(\omega = \omega_1)$. This can be the prior probability that the next student would pass DLCV course.

> 即，在没有其他information的情况下，只来修课，pass这一门课的机率。
{: .prompt-info }

And the priors must exhibit *exclusivity* and *exhaustivity*. i.e.


$$
\sum_{i = 1}^n P(\omega_i) = 1
$$

![image-20240813111913835](https://s2.loli.net/2024/08/13/SIYOspwihPml9tf.png)

### Decision rule based of priors only

有了先验机率，我们自然要想到办法做出判断，才能体现机率的价值。现在我们来看如何制定我们的Decision Rule呢？

可以这样想，假设我们期望$\omega_1$发生，那就最好要有：


$$
P(\omega_1) > P(\omega_2)
$$


否则我们认为$\omega_2$更容易发生。

这是一个很直观的规则，但更进一步，这个规则出错的机率是多少？好像priors并没有办法给我们答案。

### Class-Conditional Probability Density

考虑到我们做的是分类问题，首先我们应该专注在分类的机率密度函数上。

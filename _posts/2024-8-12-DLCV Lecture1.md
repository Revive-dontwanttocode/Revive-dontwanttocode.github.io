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

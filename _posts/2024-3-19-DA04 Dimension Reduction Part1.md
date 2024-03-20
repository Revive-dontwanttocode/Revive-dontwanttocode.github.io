---
layout:     post
title:      DA04 Dimension Reduction, Part I
subtitle:   Data Analytics
date:       2024-03-19
author:     czx
toc:        true
math:       true
mermaid:    true
catalog:    true
categories: ['Course Notes']
comments:   true
tags:
  - Data Analysis
  - Lecture Note
  - Statistics
---

# DA04 Dimension Reduction :: Part I Note

This is DA04 `part I` note. Include some detail of feature selection and penalized regression.

## Why do we need to reduce the dimension?

This is an interesting and basic question. Why reduce the dimension? Let's see two kind of dataset.

**A flatten dataset**

首先来看这样的一个所谓“扁平”的数据集。我们有$X_{n \times p}$，其中$p >> n$。这表示我们有$p$个回归变量，总共有$n$笔资料。

这时候出现了一个问题，资料的数目远远比回归变量的数目小。在 [DA02](https://revive-dontwanttocode.github.io/posts/DA02-Regression-Analysis-Notes/) 的几何意义诠释中，我们提到的情况是$n >> p$。在那时，我们的目标是用低维度的信息来尝试逼近高维度的信息。在这里我们又该如何做呢？

$\rightarrow$ 一种想法是，我们可以使用 `stepwise regression` 来逐渐添加变量。否则的话，我们的计算过程将变得十分漫长。

**A normal dataset**

与扁平数据集对应的是正常的数据集。即$n >> p$。这时候比较好处理。

在$p >> n$的情况下，我们经常出现所谓的 *维度灾难*[^1] 问题。我们该使用什么方法来解决这个问题呢？

## Feature Selection or Extraction

![](https://s2.loli.net/2024/03/19/OiCqIbwXNjgshyc.png)

维度灾难意味着我们使用了太多的$x_i$，即回归变量。一个很直觉的想法是，我们干脆挑选一批具有**代表性**的回归变量$x_i$出来不就好了？这就是所谓的 `feature selection`。

> 所谓的 `feature` ，就是一些重要的回归变量$x_i$。
{: .prompt-tip }

常用的方法大致可以分为三类。分别是所谓的 `Supervised method` ，所谓的 `unsupervised method ` 和所谓的 `non-linear method` 。其中：

* `Supervised method` 需要我们同时有 $x$ 和 $y$ 值。常用的有：
  * `Subset Regression`
  * `Penealized Regression` (will add little **constraint** on top of the regression)
    * Least Absolute Shrinkage & Selection Operator (Lasso Regression)
    * Ridge Regression (RR)
    * Elastic Net
  * Partial Least Squares Regression (A powerful regression method, it will be used in `multiple regression` case, and this method considers the cov. between $x$ and $y$ )
* `Unsupervised method` 我们并不要求有 $y$ 值。常用的有：
  * Principal Component Analysis (PCA)
  * Factor Analysis (FA)
* `non-linear method` 常用的有：
  * Independent Component Analysis (ICA)
  * Locality Preserving Projections (LPP)
  * t-distributed Stochastic Neighbor Embedding (tSNE)

我们将分别介绍。在 part 1 中，我们主要介绍 `Supervised method` 。

## Subset Regression

在 `Subset Regression` 中，我们希望通过逐个对比不同模型的方法，找到一个**最合适**的模型，从而筛选变量。

以5个回归变量（ regressors ）的模型为例。我们可以通过增加/减少变量的方法来构建不同的 regression model 。

$1^\prime$ 构建 simple regression model ，如 $y = \beta_0 + \beta_1x_1$的形式，总共会有 $C_5^1$ 种 simple regression 模型；

$2^\prime$ 构建复杂的模型，分别选 2， 3， 4， 5 个 regressors ，可以分别得到 $C_5^2$ , $C_5^3$, $C_5^4$ , $C_5^5$ 种模型。

我们就在这31个模型中选择。可以利用我们前面提到的 `R-Square` , `Adjusted R-Square` , `AIC` , `BIC` 来筛选。

> 不过要注意的是，这只是一个仅含有 `linear term` 的模型。我们没法筛选具有交叉项（如 $x_1x_2$ ）的模型。
{: .prompt-danger }

### How to identify key regressors?

那完成了所谓的模型筛选，我们又如何界定什么是**关键的**回归变量呢？

直觉的方法是，比较 $\beta_i$ 的大小，或者比较 $\beta_i$ 对应的 p-value 的大小。但这真的合理吗？

#### If ... Comparing $\beta_i$ ?

$\text{large } \beta_1 , \text{small } \beta_2 \longrightarrow \text{we shall choose } \beta_1 \text{ rather than } \beta_2 $ **(Is this right?)**

如果比较 $\beta_i$ ，有一个致命的问题。

仍然以 [DA02](https://revive-dontwanttocode.github.io/posts/DA02-Regression-Analysis-Notes/) 中的 mpg 回归问题为例。我们能发现，回归系数的值很容易受到回归变量量纲的影响。一个 $\beta$ 值很低，**不一定代表**对应的 regressor 不重要，也可能是这个 regressor 的量纲太大了，导致必须要一个小的 $\beta$ 来“配平”。

> 如果想要用 $\beta$ 的值来筛选 regressor ，那至少要做一次针对 $x$ 的标准化。
{: .prompt-tip }

#### If ... Comparing the p-value of $\beta_i$

是否有所谓的 "small p-value, more significant" 呢？答案依旧是 No 。

注意我们的 p-value ，仅仅是用来进行假设检验的！极小的 p-value 只能说明 $\beta$ 值与 0 大相径庭，而**不能说明** $\beta$ 值背后的 regressor 在我们的模型中很重要。

### Here are OK ways...

![image-20240319223020306](https://s2.loli.net/2024/03/19/RtgFCEMP4NOiVTL.png)

有两种方法来筛选。

$1^\prime$ 先对 variables 进行标准化，然后做回归。这样统一了量纲 ( same scale )，$\beta$ 的值就变得**有意义了**。可以说明哪些 regressor 很重要。

标准化的公式为：


$$
z_i = \frac {x_i - \bar{x_i}} {S_i}
$$


然后构建新的回归模型


$$
y = \beta_0 + \beta_1z_1 + ... + \beta_pz_p
$$


这时候的 $\beta_i$ 可以表示，$z_i$ 变动一个 unit ，$y$ 会变动多少。这就是 $z$ 对 $y$ 的影响！可以进行一定程度的变量筛选。

$2^\prime$ 当使用 stepwise 方法移除变量时，利用 adjusted R square 进行筛选。

## Penalized (Regularized, Shrinkage) Regression

下面我们来介绍所谓的 Penalized Regression 方法。这种方法将用选定的关键变量来构建回归模型。

## Reference

本文中的所有图片，Slides均来源于國立台灣大學工業工程學研究所藍俊宏副教授所開設的資料分析方法課程中之 `DA04 Dimension Reduction.pdf`。特此感謝藍俊宏老師在我學習歷程上的幫助！

[^1]: [Curse of dimensionality, Wikipedia](https://en.wikipedia.org/wiki/Curse_of_dimensionality)

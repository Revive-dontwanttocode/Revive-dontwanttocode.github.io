---
layout:     post
title:      DA04 Dimension Reduction, Part IV
subtitle:   Data Analytics
date:       2024-4-15 21:46:00 +0800
author:     czx
toc:        true
math:       true
mermaid:    true
catalog:    true
categories: ['Course Notes', 'Data Analytics ( IE5054 )']
comments:   true
tags:
  - Data Analysis
  - Lecture Note
  - Statistics
---

# DA04 Dimension Reduction :: Part IV, Partial Least Squares Regression

In this note, we shall talk the last part of `Dimension Reduction`, which is `Partial Least Squares Regression, (PLSR)` .

We need to mind that PLSR is a kind of **supervised** method.

## General Definition from Regression

首先从 regression 的定义说起。

**Definition 1. ( Linear Relationship )** 线性关系用来描述一个或多个 $x$ 与一个或多个 $y$ 之间的关系。其中：

* $x$: independent variables, predictor, regressor
* $y$: dependent variables, response

**Definition 2. ( Type of Regression )** 具体而言，线性回归大概有三种： Simple linear regression, multiple linear regression, multivariate multiple linear regression 。

* Simple Linear Regression: single $x$ and single $y$. e.g. $y = b_0 + b_1x$
* Multiple Linear Regression: multiple $x$ and single $y$. e.g. $y = b_0 + b_1x_1 + ... + b_p x_p$
* Multivariate Multiple Linear Regression: multiple $x$ and multiple $y$. e.g. $\mathbf{y} = \mathbf{X}\mathbf{\beta}$

那么，PLSR 又何谈 linear regression ?

### Question PLSR Facing

#### Type-I Error inflation ?

考虑 Multivariate Multiple Linear Regression 的问题。我们有：


$$
\begin{gathered}
\begin{bmatrix}
y_{11} & y_{12} & ... & y_{1q} \\
y_{21} & y_{22} & ... & y_{2q} \\
\vdots \\
y_{n1} & y_{n2} & ... & y_{nq}
\end{bmatrix}
\end{gathered}_{n \times q}
=
\begin{gathered}
\begin{bmatrix}
x_{11} & x_{12} & ... & x_{1p} \\
x_{21} & x_{22} & ... & x_{2p} \\
\vdots \\
x_{n1} & x_{n2} & ... & x_{np} \\
\end{bmatrix}
\end{gathered}_{n \times p}
\times

\begin{gathered}
\begin{bmatrix}
\beta_{11} & \beta_{12} & ... & \beta_{1q} \\
\beta_{21} & \beta_{22} & ... & \beta_{2q} \\
\vdots \\
\beta_{p1} & \beta_{p2} & ... & \beta_{pq} \\
\end{bmatrix}
\end{gathered}_{p \times q}
$$


我们要用变量 $x_1, ..., x_p$ 这 $p$ 个 regressor 去估计 $y_1, ... y_q$ 这 $q$ 个 response 。

> 一个很直觉的想法是分别对 $y_1, y_2, ..., y_q$ 做 linear regression 。这样可以分别得出相应的回归方程式。
>
> 但显然不是一个好方法，与 ANOVA 相同，会增加类似于 Type-I Error 的概率。从而使得对 $y_1, ...y_q$ 的估计不准确。
>
> 并且，分别做 regression 的方法没有考虑到 $y_1, ..., y_n$ 之间的相关性，这也会造成一定的影响。
{: .prompt-danger }

![image-20240413170809689](https://s2.loli.net/2024/04/13/LecnCS3lpXu4Mtk.png)

#### Challenges of Relationship Modeling

另一个问题是，如何在 insufficient samples （即 $p \gg n$ ）的情况下进行 relationship modeling ？

$\longrightarrow$ 需要使用 Dimension Reduction 技术。例如 Principle Component Regression ( 主成分回归 )。

> Step of  Principle Component Regression:
>
> * 先对 $x$ 做主成分分析，得到一系列主成分 $PC_1, PC_2, ..., PC_p$ 。
> * 随后，利用这些主成分对 $y$ 进行线性回归。回归式为 $y = b_0 + b_1 PC_1 + ... + b_p PC_P$
{: .prompt-info }

当然，也可以使用 Lasso 或 Ridge Regression 来做。不过这不是一种推荐的做法。

同样，面对 multiple $y$ variables ，似乎也可以用 PCR 的方法来做。

#### Problem of PC Regression

PCR 如果这么好，那问题就到此结束了。实际上，PCR 有一个很致命的问题。

在主成分分析中，我们分析的是样本 $x$ 的 covariance matrix ，试图将样本投影到方差最大的方向上。这在 Dimension Reduction 是没有问题的。然而，在回归上，我们要做的是 $y$ 对 $x$ 的回归。**那我们也应该考虑 $x$ 与 $y$ 之间的 covariance matrix 才对。**PC Regression 并没有考虑这一点。

![image-20240413173941086](https://s2.loli.net/2024/04/13/twBuDgCcG7WJeSz.png)

## Information Loss in Dimension Reduction

在做 Dimension Reduction 的时候，到底丢失了什么信息？

![image-20240413174028967](https://s2.loli.net/2024/04/13/L3S9gKEbkd5xNWZ.png)

以 PCA 为例：


$$
T_{n \times q} = X_{n \times p} \times P_{p \times q}
$$

如果没有 information loss ，则按照之前的分解得到的 $T$ 肯定能通过矩阵乘法的方式， transform 回原空间。由于矩阵 $P$ 是正交矩阵，故一定有 $P^TP = I$ 。因此：


$$
T P^T = XPP^T = \hat{X}
$$


是否有 $PP^T = P^TP$ ？答案是 of course not ！很显然，$PP^T$ 是一个 $p \times p$ 的矩阵，而 $P^TP$ 是一个 $q \times q$ 的矩阵。且 $P$ 不可逆，因此不可能有 $\hat{X} = X$ 。出现 information loss 。

因此有：


$$
\text{residual} = X - \hat{X} = E
$$


图示上可以看成。在上图中，我们将观测值 $x_1^{obs}$ 利用主成分分析分解到方向 $w_1$ 与 $w_2$ 上，以期望用 $w_1, w_2$ 来替代观测值。因此产生的残差就是 $x_1^{obs} - t_1$ 与 $x_1^{obs} - t_2$ 。这就是我们丢失的信息。



## Canonical Correlation Analysis (CCA)

下面我们先介绍所谓的`典型相关分析`。这也是一种 dimension reduction 方法，用来解释两个变量之间的关联关系。

与主成分分析不同。在主成分分析当中，我们主要分析数据集 $X$ 的 covariance matrix 。在 CCA 中，我们期望将 $X$ 和 $Y$ 一起分析，考虑 $X$ 和 $Y$ 的 covariance matrix 。

Our objective is, make largest correlation. Which means we need to maximize covariance matrix of (U, V). Where $U = Xw_x$ , $V = Yw_y$ .


$$
\text{Objective: } \max cov[Xw_x, Yw_y] \Rightarrow \max w_x^T X^TYw_y
$$

$$
\text{Subject to: } var[Xw_x] = 1, var[Yw_y] = 1
$$


我们期望 $Yw_y$ 和 $Xw_x$ 相比于 $Y$, $X$ 可以拥有更多的 information 。

几何意义是：

![image-20240415224438210](https://s2.loli.net/2024/04/15/g4bqiHjIGh2AJMf.png)

在 $X$ 的 column space 中，我们找到两个变量 $U, V$ 。期望它们之间的 corr 最大，在 column space 中，也就是 $\vec{v}, \vec{u}$ 。因此我们就是期望**两个向量之间的夹角最大**。

所以在 canonical correlation analysis 中：

* 每个 canonical correlation 都代表了一个特定的 projection 。
* 与 PCA 类似， $w_x$ 是矩阵 $S_{xx}^{-1}S_{xy}S_{yy}^{-1}S_{yx}$ 的特征向量； $w_y$ 是矩阵 $S_{xx}^{-1}S_{xy}S_{yy}^{-1}S_{yx}$ 的特征向量。

在分析后，可以找到 highly correlated 的变量，这样就能减少 $x$ 与 $y$  的变量分析个数，成功做到 dimension reduction 。

## Information Loss in Dimension Reduction

![image-20240415230351137](https://s2.loli.net/2024/04/15/E3SsGVwxlUO8h7N.png)

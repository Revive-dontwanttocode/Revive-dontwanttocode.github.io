---
layout:     post
title:      DA03 Multivariate Statistical Inference
subtitle:   Data Analytics
date:       2024-3-17
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


# DA03 :: Multivariate Statistical Inference

In this section, we will continue delving into multivariate statistical inference.

## Random Matrix, Vector, and Variable

Let's review some mathematical definitions and marks.

$X$ is a random variable and $x_1, x_2, ..., x_n$ are the $n$ observations of a random sample from $X$. 

We have **Sample average**:


$$
\bar{x} = \frac {1} {n} \sum_{i = 1}^n x_i
$$


and we have **Sample variance**:


$$
s^2 = \frac {1} {n - 1} \sum_{i = 1}^n (x_i - \bar{x})^2
$$


In Matrix Form, assume we have $p$ random variables and $n$ observations of each random variables. we can express the random sample as:


$$
X_{n \times p} = 
\begin{gathered}
\begin{bmatrix}
x_1 & x_2 & \dots & x_p
\end{bmatrix}
\end{gathered}
$$


In this case, $x_1, x_2, ..., x_p$ are $n \times 1$ column vectors. We can expand them:


$$
X = 
\begin{gathered}
\begin{bmatrix}
x_{11} & ... & x_{1p} \\
\vdots & \vdots & \vdots \\
x_{n1} & \dots & x_{np} 
\end{bmatrix}
\end{gathered}
$$


and, we always have $n >> p$. The number of rows means we have $n$ observations, and the number of columns means we have $p$ variables. And, this $n >> p$ means we have **much more data** than variables.

### Sample Mean and Variance of Matrix $X$

So we need to know how to calculate `sample mean` and `sample variance` when we have matrix data.

#### Sample mean

Let $\bar{x} = [\bar{x_1}, \bar{x_2}, ..., \bar{x_p}]^T$ be the sample mean vector. This can also in Matrix form:


$$
\bar{X}^T =
\begin{gathered}
\begin{bmatrix}
\frac {1} {n} & \frac {1} {n} & ... & \frac {1} {n}
\end{bmatrix}
\end{gathered}_{1 \times n}
\times X_{n \times p}
$$


这就是将矩阵 $X$ 的每一列加总乘以 $1/n$ ，从而得到相应的 $\bar{x_i}$ 。

要注意， $\bar{x_i}$ 是一个 $p \times 1$ 的列向量。

#### Sample Variance

在矩阵形式的 sample 下，样本方差也是一个矩阵。可记为：


$$
S_x =
\begin{gathered}
\begin{bmatrix}
s_{11} & \dots & s_{1p} \\
\vdots & \ddots & \vdots \\
s_{p1} & \dots & s_{pp}
\end{bmatrix}
\end{gathered}
$$


在这个样本方差矩阵中，要注意的是。对角线上的是对应变量的方差，而 $s_{ij}$ 是变量 $i$ 与变量 $j$ 的协方差。我们也可以将其写作矩阵运算的形式：


$$
S_x = \frac {1} {n - 1} X^T(I - \frac {1} {n} J) X
$$


其中，矩阵 $J$ 是一个 $n \times n$ ，且内部元素全为 1 的矩阵。

![image-20240321222252582](https://s2.loli.net/2024/03/21/sdwy6GDjRnkzUbL.png)

#### Sample Correlation of $X$

让我们来看一下样本相关系数矩阵。


$$
R = 
\begin{gathered}
\begin{bmatrix}
r_{11} & \dots & r_{1p} \\
\vdots & \ddots & \vdots \\
r_{p1} & \dots & r_{pp}
\end{bmatrix}
\end{gathered}
$$


其中， $r_{ij}$ 是变量 $i$ 和变量 $j$ 之间的相关系数。相关系数的计算公式为：


$$
r_{xy} = \frac {cov(x, y)} {\sqrt{var(x) var(y)}}
$$


同样地，也可以用矩阵运算的形式来表示。


$$
R = 
\begin{gathered}
\begin{bmatrix}
\frac {1} {\sqrt{s_{11}}} & \dots & 0 \\
\vdots & \ddots & \vdots \\
0 & \dots & \frac {1} {\sqrt{s_{pp}}}
\end{bmatrix}
\end{gathered}
\times S_x \times 
\begin{gathered}
\begin{bmatrix}
\frac {1} {\sqrt{s_{11}}} & \dots & 0 \\
\vdots & \ddots & \vdots \\
0 & \dots & \frac {1} {\sqrt{s_{pp}}}
\end{bmatrix}
\end{gathered}
$$


其中有 $r_{ij} = \frac {s_{ij}} {s_is_j}$ 。

![image-20240321223013056](https://s2.loli.net/2024/03/21/VBqywFkLxQloceW.png)

### Linear Transformation of $X$

在线性代数中，我们知道矩阵相乘就是一种线性变换。用于做乘法的矩阵是一个函数，将在一个线性空间 $\mathbb{R}^n$ 中的矩阵映射到另一个线性空间 $\mathbb{R}^m$ 中。

在 regression 中，我们也有对 dataset 进行线性变换的需求。因此我们也会需要相应的变换矩阵。


$$
Z = XA
$$


是一个将 $X$ 变换到 $Z$ 的线性变换。其中，


$$
A =
\begin{gathered}
\begin{bmatrix}
a_1 & \dots & a_{p}
\end{bmatrix}
\end{gathered}_{p \times p}
$$


我们可以算出变换后的新 dataset 的样本均值与样本方差。


$$
\bar{Z} = \bar{X} A
$$

$$
S_z = A^TS_xA
$$


> 可以发现，矩阵 $Z$ 中的每个 column 都是原本矩阵 $X$ 的每个 column 的线性组合。
{: .prompt-tip }

![image-20240321223629784](https://s2.loli.net/2024/03/21/ake6HdR28CvG7Bp.png)

### Overall Variability of $X$

Here are several special matrix statistics we need to introduce.

### Generalized Sample Variance of X

这是所谓的 `样本广义方差`，计算方式为对样本方差矩阵 $S_x$ 取行列式值。用来描述数据的稀疏性（ sparsity ）。

#### Total Sample Variance of X

这是总样本方差。就是把每个变量的自身方差相加。即：


$$
\sum_{i = 1}^p S_{ii}
$$


这也是样本方差矩阵 $S_x$ 的 trace。即 $tr(S_x)$ 。

> 这个方差会有一点高估总体的 information 。因为很可能在 $x_1, x_2, ..., x_p$ 中，有一些变量之间有相关性，它们的方差不能简单地加减。举例来说，如果 $x_1$ 和 $x_2$ 呈现高度相关，将其方差直接相加必然有高估（ inflating）。
{: .prompt-danger }

## Distance between Vectors

接下来要介绍的是如何判断两个 vector 之间的距离。这是一个很重要的观念。

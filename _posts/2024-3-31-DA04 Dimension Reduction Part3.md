---
layout:     post
title:      DA04 Dimension Reduction, Part III
subtitle:   Data Analytics
date:       2024-3-31 22:36:00 +0800
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

# DA04 Dimension Reduction :: Part III, Factor Analysis

In this part, we will introduce another dimension reduction technology, `Factor Analysys`.

## FA and PCA ... What Different ?

Recall that PCA is to find the combination of the **original variables**, or, **original dataset**. We have:


$$
T = X P
$$


where,

>* $T$ is our **Principle Components Matrix**.
>* $X$ is our original dataset.
>* $P$ is the **Loading Matrix**, which is, to project original dataset $X$ to the new space. This is so-called *Combination of original variables*.
{: .prompt-info }

However, in Factor Analysis (FA) , we want to find latent (aka underlying) variables that can explain the **original variables**. It may like *Hidden* variables. ( 潜在的变量 ) We have:


$$
X = FA
$$


where, $X$ is the known **original dataset**, or, **original variables**.

>Generally speaking,
>
>* In PCA, *PC* is unknown. We want to use original variables $X$ to `compose` PCs.
>* In FA, we have $X$ known original variables. And we decide to **de-compose** $X$.
{: .prompt-danger }

## Decomposition ?

Previously, we have already known that we want to decompose original $X$ to $FA$ . Let's borrow the concept of SVD.


$$
X = UDV^T
$$


where,

* $X$ is a $n \times p$ matrix, this is the original variable matrix.
* $U$ is a $n \times n$ matrix, and it is **orthogonal**, $U^TU = I$.
* $D$ is a $n \times p$ **diagonal** matrix.
* $V$ is a $p \times p$ matrix.

### What we do to connect this decomposition  with $X = FA$ ?

Let $F = \sqrt{N} UD$, and let $A = V^T / \sqrt{N}$ .

>For decomposition of $F$ :
>
>* we use $\sqrt{N}$ to rescale $F$.
>* $N$ maybe a matrix, to rescale each column of $F$.
>* $F$ is composed of **uncorrelated variables** with unit variances. ( $\sigma^2 = 1$ )
{: .prompt-tip }

So, we have:


$$
X_{n \times p} = F_{n \times p} A_{p \times p}
$$
We also need to mind that $F$ is **not unique**. Given any orthogonal $p \times p$ matrix $R$, we can always let $FA$ become $FR^TRA$, then combine $FR^T$ to new $F^\ast$, and $RA$ to new $A^\ast$.

How about unit variance? $cov(F^*) = R^T cov(F) R = I$ , ( why?[^1] ) still have **unit variance**.

**However ...**

* We know that $F$ is a $n \times p$ matrix, this means $F$ has $p$ factors. However in our initial case, $X$ also has $p$ variables! ( Recall that our target is we want to find *latent* factors and reduce the number of factors ) How to use $p$ factors in $F$ to explain $p$ variables in $X$ ?
* And, how about explanation?

We shall deal with this issues later.

## Reducing $p$ to $q$ ( $q \lt p$ )

Our target is reducing. we want to use $q$ factors. So,


$$
X_{n \times p} = F_{n \times q} A_{q \times p} + E_{n \times p}
$$

Open it,



$$
\begin{gathered}
\begin{bmatrix}
x_1 & ... & x_p
\end{bmatrix}
\end{gathered}
=
\begin{gathered}
\begin{bmatrix}
f_1 & ... & f_q
\end{bmatrix}
\end{gathered}
\times
\begin{gathered}
\begin{bmatrix}
a_1 & ... & a_p
\end{bmatrix}
\end{gathered}
+
\begin{gathered}
\begin{bmatrix}
\epsilon_1 & ... & \epsilon_p
\end{bmatrix}
\end{gathered}
$$

* $[f_1, ..., f_q ]$ is $q$ factors. The covariance of the matrix $F$ is $I$ (Univariance).
* $[a_1, ..., a_p]$ is $p$ loadings.
* $[\epsilon_1, ..., \epsilon_p]$ is $p$ residuals. Dimension of matrix $E$ is same as the dimension of matrix $X$.

> As we try to use $q \ll p $ factors, we surely get residual.
{: .prompt-danger }

And, for more opening step,


$$
\begin{gathered}
\begin{bmatrix}
x_1 & ... & x_p
\end{bmatrix}
\end{gathered}_{n \times p}
=
\begin{gathered}
\begin{bmatrix}
f_1 & ... & f_q
\end{bmatrix}
\end{gathered}_{n \times q}
\times
\begin{gathered}
\begin{bmatrix}
a_{11} & ... & a_{qp} \\
\vdots & \ddots & \vdots \\
a_{q1} & \dots & a_{qp}
\end{bmatrix}
\end{gathered}
+
\begin{gathered}
\begin{bmatrix}
\epsilon_1 & ... & \epsilon_p
\end{bmatrix}
\end{gathered}_{n \times p}
$$


We are interested in the middle matrix $A$, which is our **loadings**. We can reduce $p$ to $q$ !

![image-20240331215809404](https://s2.loli.net/2024/03/31/uyopFUAvnNsm9ZP.png)

So how to get $F$ and $A$ ?

### Assumptions in Factor Analysis

We also need assumptions for factor analysis.

1. $X$ is centered. i.e., $E[X] = 0$.
2. For Factor Matrix $F$, we want:
  1. $E[F] = 0$
  2. $var[F] = E[F^TF] = I$
  3. $F$ is **orthogonal**
3. For residual matrix $E$, we want:
  1. $E[E] = 0$
  2. $var[E]$ is a diagonal matrix, with $\psi_i$ on the diag, and other place are all 0. ( 这意味着 residual 之间是互相独立的，并且 $p$ 个 residual 都有它自己的方差 $\psi_i$ )
4. $cov(E,F) = cov(E, F^T) = 0$ ( residual and our factors are independent )
5. $cov(X, F) = E[F^TX] = E[F^T(FA + E)] = E[F^TF]A + E[F^TE] = A$ ( 数据集 $X$ 和我们分解得到的 factor 之间的协方差是 loading matrix $A$ )

![image-20240331222006217](https://s2.loli.net/2024/03/31/HWqQvY7hLDc4RrV.png)

### Interesting Facts

After our assumptions, we have some interesting facts.

Mind that our decomposition is $X = FA + E$. We surely wang to ask, how about relationship between $X$ and $A$ ?



$\longrightarrow$ For $X^TX$,


$$
\begin{eqnarray}
X^TX &=& (FA + E)^T (FA + E)  \nonumber    \\
~ &=& (A^TF^T + E^T)(FA + E) \nonumber    \\
~ &=& (A^TF^TFA + A^TF^TE + E^TFA + E^TE) \nonumber
\end{eqnarray}
$$



Calculate expectation of $X^TX$ on $F$,


$$
\begin{eqnarray}
E[X^TX] &=& cov(X)  \nonumber    \\
~ &=& A^TE[F^TF]A + A^TE[F^TE] + E[E^TF] + E[E^TE] \nonumber    \\
~ &=& A^TA + 0 + 0 + \psi \nonumber \\
~ &=& A^TA + \psi \nonumber
\end{eqnarray}
$$


也就是说，初始数据 $X$ 的 covariance matrix $\Sigma$ 为：


$$
\Sigma = A^TA +\psi
$$


$A$ is `loading matrix`, and $\psi$ is `residual variance`.

![image-20240331224830346](https://s2.loli.net/2024/03/31/uG1sJaVHr6mUnh5.png)

### Covariance Structure of the Factors

同样，我们也考虑矩阵 $F$ 的 covariance matrix。

## Reference

本文中的所有图片，Slides均来源于國立台灣大學工業工程學研究所藍俊宏副教授所開設的資料分析方法課程中之 `DA04 Dimension Reduction.pdf`。特此感謝藍俊宏老師在我學習歷程上的幫助！


[^1]: In Covariance computing, we have $cov(AX) = Acov(X, X^T)A^T = Acov(X)A^T$, where $A$ is a constant vector.

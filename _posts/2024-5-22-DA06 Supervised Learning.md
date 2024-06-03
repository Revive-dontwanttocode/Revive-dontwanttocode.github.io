---
layout:     post
title:      DA06 Supervised Learning
subtitle:   Data Analytics
date:       2024-5-22 15:46:33 +0800
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

# DA06 :: Supervised Learning

In DA06, we shall talk about supervised learning methods.

## Common Procedure of Data Analytics

* Domain Studying: Consider what you are styding. May related to some domain knowledge.
* Cleaning & Pretreatments: This part we need to load data into some certain format, i.e. `DataFrame`
* Feature Selection & Extraction: This part is an important part. We do not know if $p$ variables are all important. So we may want to do some *cut*. Generally, this part include:
  * generate new features which are no tin original dataset. (like PCA)
  * dimension reduction (such as LASSO, RR)
* Mining and Learning: Construct the model, such as $y = f(x)$.
* Result Evaluation & Interpretation: This part we may use some domain knowledge, to translate statistics to description results. What's more, we also need to consider overfit/underfit question.
* Presentation: Show our result.

## What is supervised learning?

> Supervised Indicates Additional Info other than original dataset $X$.

例如，我们有 dataset $X$:


$$

$$

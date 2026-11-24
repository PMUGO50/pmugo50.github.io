---
title: 方差最小化的线性回归
date: 2026-12-02 22:49:56
categories: 时间序列处理
mathjax: true
description: 以方差最小化为目标的线性回归
---

# 最小二乘回归

假定随机变量 $y$ 可以由随机向量 $\boldsymbol{x}$ 各分量的线性组合来表示，即
$$
y = \boldsymbol{\beta} \cdot \boldsymbol{x} + \alpha
$$
那么对于样本序列 $\{ \boldsymbol{x}_i \}$ 和 $\{y_i\}$ ，理论上应当满足
$$
y_i = \boldsymbol{\beta} \cdot \boldsymbol{x}_i + \alpha_i
$$
从已知的样本序列来估计 $\boldsymbol{\beta}, \alpha$ 的问题被称为线性回归。特别地，如果满足以下条件：

1. 残差 $\alpha_i$ 是独立同分布的，且 $\mathrm{E}(\alpha_i)=0$ ，$\mathrm{Var}(\alpha_i) = \sigma^2$ ，即是说残差序列是一个平稳的白噪声
2. 对于 $\{ \boldsymbol{x}_i \}$ 中任意一个分量，其无法始终被另一个分量线性表出

此时回归的解法被称为简单最小二乘（OLS），它的做法是令估计的方差最小化


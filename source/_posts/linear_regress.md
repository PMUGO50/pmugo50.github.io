---
title: 方差最小化的线性回归
date: 2026-12-02 22:49:56
categories: 时间序列处理
mathjax: true
description: 以方差最小化为目标的线性回归
---

# 最小二乘回归

假定随机变量 $y$ 可以由随机向量 $\boldsymbol{x}$ 各分量的线性组合来表示，即 $y = \boldsymbol{\beta} \cdot \boldsymbol{x} + \alpha$ ；那么对于样本序列 $\{ \boldsymbol{x}_i \}$ 和 $\{y_i\}$ ，理论上应当满足
$$
y_i = \boldsymbol{\beta} \cdot \boldsymbol{x}_i + \alpha
$$
从已知的样本序列来估计 $\boldsymbol{\beta}, \alpha$ 的问题被称为线性回归。回归模型给出预测值 $\hat{y} = \hat{\boldsymbol{\beta}} \cdot \boldsymbol{x} + \hat{\alpha}$ ，对每一个样本来说就是
$$
\hat{y}_i = \hat{\boldsymbol{\beta}} \cdot \boldsymbol{x}_i + \hat{\alpha}
$$
预测值与实际值之间的误差被称为残差 $e_i = y_i - \hat{y}_i$ ；特别地，如果满足以下条件：

1. 残差是独立同分布的，且 $\mathrm{E}(e_i)=0$ ，即是说残差序列是一个白噪声
2. 对于 $\{ \boldsymbol{x}_i \}$ 中任意一个分量，其无法始终被另一个分量线性表出

此时回归的解法被称为普通最小二乘（OLS），它的做法是令总残差的平方和最小化。首先，残差的平方和是
$$
E^2 = \sum_{i=1}^n e_i^2 = \sum_{i=1}^n \left( y_i - \hat{\boldsymbol{\beta}}^T \boldsymbol{x}_i - \hat{\alpha} \right)^2
$$
它是关于估计值 $\hat{\boldsymbol{\beta}}, \hat{\alpha}$ 的二次函数。因而应当只有一个极值点，极值条件由梯度为零给出，即
$$
\begin{aligned}
& \frac{\partial E^2}{\partial \hat{\boldsymbol{\beta}}} = 2\sum_{i=1}^n \left(\hat{\boldsymbol{\beta}}^T \boldsymbol{x}_i + \hat{\alpha} - y_i\right)\boldsymbol{x}_i^T = 0 \\[12pt]
& \frac{\partial E^2}{\partial \hat{\alpha}} = 2\sum_{i=1}^n \left(\hat{\boldsymbol{\beta}}^T \boldsymbol{x}_i + \hat{\alpha} - y_i\right) = 0
\end{aligned}
$$

将上面两式分别做转置之后，成为两个线性方程组
$$
\left(  \sum_{i=1}^n \boldsymbol{x}_i \boldsymbol{x}_i^T \right)\hat{\boldsymbol{\beta}} + \left( \sum_{i=1}^n \boldsymbol{x}_i \right)\hat{\alpha} = \sum_{i=1}^n y_i \boldsymbol{x}_i \quad,\quad \left(  \sum_{i=1}^n \boldsymbol{x}_i^T \right)\hat{\boldsymbol{\beta}} + n\hat{\alpha} = \sum_{i=1}^n y_i
$$

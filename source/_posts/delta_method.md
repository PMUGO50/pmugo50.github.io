---
title: 样本矩函数的渐近分布
date: 2026-11-20 22:32:49
categories: 时间序列处理
mathjax: true
description: 在大样本下样本矩的函数的渐近分布
---

# Delta 方法

## 一阶方法

一些统计量可以显式地表示为样本矩的函数，比如在[学生氏统计量的渐近分布](/2026/10/28/geometric_sharpe)中出现的几何 Sharpe 比率，就是两个样本矩 $\bar{r}$ 和 $s^2$ 的函数。Delta 方法要解决的是：在已知样本量 $N \rightarrow\infty$ 时样本矩的分布特性后，如何获知其函数的分布特性

记样本矩的真值向量为 $\theta$ ，估计值是 $\hat{\theta}$ ，并记 $\hat{\theta} - \theta \sim D(\theta, N)$ ，这里 $D$ 是一个可以先验地获知的概率分布。如果对于矩的估计是**相合**的，也就是说当样本量趋向于无穷时有 $\hat{\theta} - \theta \overset{\mathrm{p}}{\longrightarrow} 0$ ，那么对于一个函数 $g(\hat{\theta})$ ，就可以在真值附近对其进行 Taylor 展开

先保留到一阶的情况，有
$$
g(\hat{\theta}) = g(\theta) + \frac{\partial g}{\partial \hat{\theta}}(\theta) \cdot (\hat{\theta} - \theta) + o(|\hat{\theta} - \theta|)
$$
或者利用 Lagrange 中值定理，将上式写为
$$
g(\hat{\theta}) = g(\theta) + \frac{\partial g}{\partial \hat{\theta}}(\tilde{\theta}) \cdot (\hat{\theta} - \theta)
$$
$\tilde{\theta}$ 是位于 $\hat{\theta}$ 和 $\theta$ 之间的某个值。当样本量增大时，$\hat{\theta} - \theta$ 依概率收敛到零，因而 $\tilde{\theta} - \hat{\theta}$ 同样依概率收敛。于是上式可以给出一阶 Delta 方法 

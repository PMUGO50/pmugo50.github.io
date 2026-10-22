---
title: 常见的收益比率
date: 2026-10-23 22:44:21
categories: 时间序列处理
mathjax: true
description: 按照波动对收益进行调整的比率
---

# Sharpe 比率

## 经典 Sharpe 比率

Sharpe 比率是以标准差衡量的波动 $\sigma_R$ 来惩罚超额收益 $\mu_R - R_f$ ，来衡量资产组合中风险资产的持有价值。这里下标 $R$ 表示期望和方差都是在简单收益率下取得的，$R_f$ 是无风险收益率，Sharpe 比率定义为
$$
\mathrm{Sharpe} = \frac{\mathrm{E}(R) - R_f}{\sqrt{\mathrm{Var}(R)}}
$$
这样定义的一个初衷是：投资者可以同时持有一部分风险资产和无风险资产，既然 Sharpe 比率意在衡量风险资产本身的持有价值，那么只要投资者不改变持有的风险资产，而只是调整持有风险资产占总资产的比例 $y$ 时，Sharpe 比率就不应当改变

上式定义的 Sharpe 比率是符合这样的要求的。按照[长期复利收益](/2026/08/28/compound_return)中最后说明的，在同一时间截面上，资产组合的简单收益率就是其中各资产简单收益率的市值加权，即
$$
R_\mathrm{Total} = yR + (1-y)R_f \quad,\quad \mathrm{E}(R_\mathrm{Total}) = y\mu_R + (1-y)R_f \quad,\quad \mathrm{Var}(R_\mathrm{Total}) = y^2\sigma^2_R
$$
资产组合的 Sharpe 比率是
$$
\mathrm{Sharpe} = \frac{\mathrm{E}(R_\mathrm{Total}) - R_f}{\sqrt{\mathrm{Var}(R_\mathrm{Total})}} = \frac{\mu_R - R_f}{\sigma_R}
$$
它不依赖于 $y$ ，因而是符合上述要求的。并且还有

{% note primary %}
**Sharpe比率对持仓的独立性**：对于一个资产组合，它的 Sharpe 比率就等于其中风险资产的 Sharpe 比率，且不依赖于风险资产占总资产的持仓比例
{% endnote %}

Sharpe 比率的意义是：承受单位波动能获得的超额收益。显然，一位理性的投资者总是想最大化 Sharpe 比率的。假设这样的投资者手中有 $n$ 个可选的风险资产，分配给它们的比例用一个向量表示 $\boldsymbol{x} = [x_1, \cdots, x_n]$ ，那么风险资产总的收益是一个多元联合分布的随机变量的线性组合 $R = \boldsymbol{R}^T \boldsymbol{x}$ 

从而风险资产收益的期望是
$$
\mathrm{E}(R) = \mathrm{E}(\boldsymbol{R}^T \boldsymbol{x}) = \boldsymbol{\mu}_R^T \boldsymbol{x}
$$
方差则是协方差矩阵的二次型
$$
\mathrm{Var}(R) = \mathrm{E}\left[ (R - \mathrm{E}(R))^2 \right] = \boldsymbol{x}^T\mathrm{E}\left[ (\boldsymbol{R} -\boldsymbol{\mu}_R)(\boldsymbol{R}^T -\boldsymbol{\mu}_R^T) \right]\boldsymbol{x} = \boldsymbol{x}^T\boldsymbol{\Sigma}_R\boldsymbol{x}
$$
这里 $\boldsymbol{\Sigma}_R = \mathbf{Cov}(\boldsymbol{R}) = \mathrm{E}\left[ (\boldsymbol{R} -\boldsymbol{\mu}_R)(\boldsymbol{R}^T -\boldsymbol{\mu}_R^T) \right]$ 是协方差矩阵。最大化 Sharpe 比率的任务可以分两步完成：

1. 对于给定的 $\mathrm{E}(R)$ ，找到对应的最小 $\mathrm{Var}(R)$ ，这用 Lagrange 乘数法完成（经典力学的第一类 Lagrange 方程）
2. 遍历 $\mathrm{E}(R)$ 以及对应的最小 $\mathrm{Var}(R)$ ，找到最大的 Sharpe 比率

第二步无需多言。第一步可以表述为
$$
\min \boldsymbol{x}^T\boldsymbol{\Sigma}_R\boldsymbol{x} \quad \mathrm{s.t.}\ \boldsymbol{\mu}_R^T \boldsymbol{x} = \mathrm{E}(R)
$$
构造 Lagrange 函数 $L(\boldsymbol{x},\lambda) = \boldsymbol{x}^T\boldsymbol{\Sigma}_R\boldsymbol{x} + \lambda(\boldsymbol{\mu}_R^T \boldsymbol{x} - \mathrm{E}(R))$ ，令其梯度为零给出
$$
\frac{\partial L}{\partial \boldsymbol{x}} = 2 \boldsymbol{\Sigma}_R\boldsymbol{x} + \lambda \boldsymbol{\mu}_R = 0 \quad,\quad \frac{\partial L}{\partial \lambda} = \boldsymbol{\mu}_R^T \boldsymbol{x} - \mathrm{E}(R) = 0
$$
由第一个方程组解出 $ \boldsymbol{x} = (\lambda/2) \boldsymbol{\Sigma}_R^{-1} \boldsymbol{\mu}_R $ ，常数 $\lambda$ 是一个待定系数，它由 $\sum_{i=1}^n x_i = 1$ 来确定

## 资本资产定价模型

资本资产定价模型（CAPM）给出在理性市场上的竞争导致的定价结果。



# 连续复合收益的学生氏检验

# 其它比率

## Sortino 比率

## Omega 比率

## Calmar 比率

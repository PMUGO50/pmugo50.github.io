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

资本资产定价模型（CAPM）给出在理性市场上的竞争导致的定价结果，它基于以下假设：

1. 所有投资者都是理性的，意味着他们都会最大化自己的 Sharpe 比率
2. 所有投资者都能获得同样的信息，且没有内幕交易

因此首先

{% note primary %}

**命题1**：在市场均衡时，所有投资者的风险资产组合将具有相同的权重分配 $\{ x_i\}$ 

{% endnote %}

既然如此，假设投资者 $k$ 配置的总风险资产额为 $P_k$ ，那么对于风险资产 $i$ ，该投资者贡献的市场市值为 $x_iP_k$ ；这样的话，风险资产 $i$ 在市场上的总市值 $x_i$ 是
$$
P_i = x_i\sum_{k} P_k \quad,\quad x_i = \frac{P_i}{\sum_k P_k} = \frac{P_i}{\sum_{i=1}^n P_i}
$$
这里利用了 $\sum_{i=1}^n P_i = \sum_{i=1}^n x_i \sum_{k} P_k = \sum_k P_k$ ，这就意味着

{% note primary %}

**命题2**：这个权重分配 $\{ x_i\}$ 正是资产 $i$ 的市值占市场总资产的比例，因此每个投资者都持有了市场的市值加权指数，他们的收益期望 $\mu_R$ 就等于市场指数的收益期望 $\mu_M$ ，收益波动 $\sigma_R$ 也正是市场指数的收益波动 $\sigma_M$ 

{% endnote %}

既然如此，每个投资者自然也将拥有同样的 Sharpe 比率，并且由于他们的理性，自然也是最优化的，也就是说必要条件是
$$
\frac{\partial}{\partial x_i}\mathrm{Sharpe} = \frac{\sigma_M\partial\mu_M/\partial x_i - (\mu_M - R_f)\partial\sigma_M/\partial x_i}{\sigma_M^2} = 0
$$
考虑到
$$
\frac{\partial\mu_M}{\partial x_i} = \mu_{R,i} \quad,\quad \frac{\partial\sigma_M}{\partial x_i} = \frac{1}{\sigma_M}\sum_{m=1}^n \Sigma_{R,im}x_m
$$
于是导函数为零这个方程成为
$$
\mu_{R,i} = \frac{\mu_M - R_f}{\sigma_M^2}\sum_{m=1}^n \Sigma_{R,im}x_m
$$
按照协方差定义，被求和的项可以写为
$$
\mathrm{E} \left[(R_i - \mu_{R,i})\cdot \sum_{m=1}^nx_m(R_m - \mu_{R,m})\right] = \mathrm{E} \left[(R_i - \mu_{R,i})(R_M - \mu_{M})\right] = \rho_{i,M} \sigma_{R,i}\sigma_M
$$
这里 $\rho_{i,M}$ 是资产 $i$ 和市场指数收益之间的相关性，结合以上两式给出

{% note primary %}
**命题3**：在 CAPM 的假设下，单个资产的期望收益和市值加权市场指数的期望收益之间的关系是
$$
\mu_{R,i} = \rho_{i,M}\frac{\sigma_{R,i}}{\sigma_M} (\mu_M - R_f)
$$
{% endnote %}

# 连续复合收益的学生氏检验

# 其它比率

## Sortino 比率

## Omega 比率

## Calmar 比率

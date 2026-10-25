---
title: 对数收益的假设检验
date: 2026-10-28 12:15:40
categories: 时间序列处理
mathjax: true
description: 在不同的分布假设下对对数收益率进行假设检验
---

# 独立正态样本

仿照简单收益率的 Sharpe 比率，将同样的公式应用于对数收益率，将这个结果称为**几何 Sharpe 比率** ，即
$$
\mathcal{S} = \frac{\mathrm{E}(r) - r_f}{\sqrt{\mathrm{Var}(r)}}
$$
相比于简单收益率的情况，对数收益的不便在于：对一个风险资产组合，它的对数收益并不等于各个风险资产的对数收益按照市值加权的结果

然而对数收益最大的好处就在于它的可加性，因而期望和均值可以直接通过对收益序列进行矩估计获得。如果对数收益的样本序列是一组独立的正态样本，那么几何 Sharpe 比率的估计值直接构成一个学生氏检验问题

对于总数为 $n$ 的独立正态对数收益样本序列，它的学生氏统计量是
$$
\tau = \frac{\bar{r} - \mu'}{s/\sqrt{n}} \sim t(n-1) \rightarrow N(0,1)
$$

极限在 $n \rightarrow \infty$ 时取到，这是学生氏分布的性质

举个例子，样本序列是 $\{ r_i\}$ ，一年中有 $n_\mathrm{A}$ 个样本，那么年化 Sharpe 比率的估计值是
$$
\mathcal{S} = \sqrt{n_\mathrm{A}}\frac{\bar{r} - r_f}{s} = \sqrt{\frac{n_\mathrm{A}}{n}}\frac{\bar{r} - r_f}{s/\sqrt{n}} = \sqrt{\frac{n_\mathrm{A}}{n}}\tau \quad\mathrm{when}\quad \mu' = r_f
$$
也就是说

{% note primary %}

**独立正态样本的几何 Sharpe 比率**：如果证券的对数收益样本序列是独立的正态变量，且其期望与无风险收益相等 $\mu' = r_f$ ，记其年化的几何 Sharpe 比率为 $\mathcal{S}$ ，一年包含的样本数为 $n_\mathrm{A}$ ，那么
$$
\tau = \sqrt{\frac{n}{n_\mathrm{A}}} \mathcal{S} \sim t(n-1)
$$
{% endnote %}

如此一来，可以利用该值进行单边学生氏检验。做出原假设 $H_0: \mu' \leq r_f$ ，如果计算得到的 $\tau$ 值大于等于学生氏分布的 $\alpha \%$ 分位数，那么就可以断言：**有 $\alpha \%$ 的把握认为原假设不成立，因而证券的期望收益 $\mu'$ 明显超过无风险收益** 

显然，几何 Sharpe 比率越大，把握值 $\alpha \%$ 也越大，越能说明证券具有高的超额收益

# 非正态样本的修正

一般的对数收益分布不会是正态分布，往往具有**负的偏度**和**正的超额峰度**。因此需要考虑对几何 Sharpe 比率的概率分布做调整，一个经典的做法是采用 Delta 方法完成

Sharpe 比率涉及到两个矩参数，即期望和方差，编排成一个向量 $\theta = [\mu', \sigma^2]$ ，它的估计值自然是 $\hat{\theta} = [\bar{r}, s^2]$ ；由于样本是独立同分布的，因而应用中心极限定理，估计值服从正态分布。进行与[随机游走与矩估计](/2026/10/16/random_walk)中一致的推导，就可以得出

1. $\mathrm{E}(\hat{\theta}) = \theta$ 
2. 由 Khinchin 大数定律，$\hat{\theta} \rightarrow \theta$ 

现在同样，在 $r_f = \mu'$ 的假设下考虑问题。几何 Sharpe 比率的估计值 $\mathcal{S}(\hat{\theta}) = (\bar{r} - \mu')/s$ 是 $\hat{\theta}$ 的函数，将其在真值 $\theta$ 处 Taylor 展开为
$$
\mathcal{S}(\hat{\theta}) = \mathcal{S}(\theta) + \frac{\partial \mathcal{S}}{\partial \hat{\theta}} (\hat{\theta} - \theta) + o(|\hat{\theta} - \theta|)
$$
当样本量 $n \rightarrow \infty$ 时，小量部分成为一阶项的高阶无穷小，这就有

{% note primary %}

**一阶 Delta 方法**：如果一个参数向量 $\theta$ 的估计值是 $\hat{\theta}$ ，它的一个函数是 $\mathcal{S}(\hat{\theta})$ ，且在大数定律下 $\hat{\theta} \rightarrow \theta$ ，以及 $\theta$ 处的 $\partial \mathcal{S}/\partial \hat{\theta} \neq 0$ ，那么
$$
\mathcal{S}(\hat{\theta}) - \mathcal{S}(\theta) = \frac{\partial \mathcal{S}}{\partial \hat{\theta}} \cdot(\hat{\theta} - \theta)
$$
{% endnote %}

在这里所考虑的情况下有
$$
\mathcal{S}(\theta) = 0 \quad,\quad \frac{\partial \mathcal{S}}{\partial \hat{\theta}} = \left[\begin{matrix} 1/s \\ (\mu'-\bar{r})/s^2 \end{matrix}\right] \quad,\quad \hat{\theta} - \theta \sim N(0, \boldsymbol{\Sigma})
$$

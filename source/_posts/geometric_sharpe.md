---
title: 几何Sharpe比率的渐近分布
date: 2026-10-28 12:15:40
categories: 时间序列处理
mathjax: true
description: 几何Sharpe比率在不同的分布假设下的渐近统计特性
---

# 学生氏检验

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
\hat{\mathcal{S}} = \sqrt{n_\mathrm{A}}\frac{\bar{r} - r_f}{s} = \sqrt{\frac{n_\mathrm{A}}{n}}\frac{\bar{r} - r_f}{s/\sqrt{n}} = \sqrt{\frac{n_\mathrm{A}}{n}}\tau \quad\mathrm{when}\quad \mu' = r_f
$$
也就是说

{% note primary %}

**独立正态样本的几何 Sharpe 比率**：如果证券的对数收益样本序列是独立的正态变量，且其期望与无风险收益相等 $\mu' = r_{f}$ ，记其年化的几何 Sharpe 比率估计值为 $\hat{\mathcal{S}}$ ，一年包含的样本数为 $n_\mathrm{A}$ ，那么
$$
\tau = \sqrt{\frac{n}{n_\mathrm{A}}} \hat{\mathcal{S}} \sim t(n-1)
$$
{% endnote %}

如此一来，可以利用该值进行

> **单边学生氏检验**：
>
> 1. 做出原假设 $H_0: \mu' \leq r_f$ 
> 2. 按照上述命题计算 $\tau$ 值
> 3. 如果 $\tau$ 大于等于学生氏分布的 $\alpha \%$ 分位数，那么就可以断言：有 $\alpha \%$ 的把握认为原假设不成立，因而证券的期望收益 $\mu'$ 明显超过无风险收益
>

显然，几何 Sharpe 比率越大，把握值 $\alpha \%$ 也越大，越能说明证券具有高的超额收益

# 一般的渐近分布

以上简单直接的考量有两个不足之处：

1. 无法给出 $r_f \neq 0$ 时的 Sharpe 比率遵循何种分布
2. 一般的对数收益分布也不会是正态分布，往往具有**负的偏度**和**正的超额峰度**

因此需要考虑一般情况下几何 Sharpe 比率的统计特性，一个经典的做法是采用 Delta 方法。以下内容是 A. W. Lo 和 E. Mertens 在 2002 年公布的，见 <span style="color:#417FBE; font-style:italic">E. Mertens, "Comments on variance of the IID estimator in Lo (2002)," 2002,</span>  *[https://www.elmarmertens.com/research/workingpapers](https://www.elmarmertens.com/research/workingpapers).* 

Sharpe 比率涉及到两个矩参数，即期望和方差，编排成一个向量 $\theta = [\mu', \sigma^2]$ ，它的估计值自然是 $\hat{\theta} = [\bar{r}, s^2]$ ；由于样本是独立同分布的，因而应用中心极限定理，估计值服从正态分布。进行与[随机游走与矩估计](/2026/10/16/random_walk)中一致的推导，就可以得出

- $\mathrm{E}(\hat{\theta}) = \theta$ 
- 由 Khinchin 大数定律，$\hat{\theta} \rightarrow \theta$ 

现在几何 Sharpe 比率的估计值 $\hat{\mathcal{S}} = \mathcal{S}(\hat{\theta}) = (\bar{r} - r_f)/s$ 是 $\hat{\theta}$ 的函数，将其在真值 $\theta$ 处 Taylor 展开为
$$
\mathcal{S}(\hat{\theta}) = \mathcal{S}(\theta) + \frac{\partial \mathcal{S}}{\partial \hat{\theta}} (\hat{\theta} - \theta) + o(|\hat{\theta} - \theta|)
$$
当样本量 $n \rightarrow \infty$ 时，小量部分成为一阶项的高阶无穷小，这就有

{% note primary %}

**一阶 Delta 方法**：如果一个参数向量 $\theta$ 的估计值是 $\hat{\theta}$ ，它的一个函数是 $\mathcal{S}(\hat{\theta})$ ，且在大数定律下 $\hat{\theta} \rightarrow \theta$ ，以及 $\theta$ 处的 $\partial \mathcal{S}/\partial \hat{\theta} \neq 0$ ，那么
$$
\mathcal{S}(\hat{\theta}) - \mathcal{S}(\theta) \rightarrow \frac{\partial \mathcal{S}}{\partial \hat{\theta}} \cdot(\hat{\theta} - \theta)
$$
{% endnote %}

在这里所考虑的情况下有
$$
\left.\frac{\partial \mathcal{S}}{\partial \hat{\theta}}\right|_{\hat{\theta}=\theta} = \left[\begin{matrix} 1/\sigma \\ (r_f-\mu')/2\sigma^3 \end{matrix}\right] \quad,\quad \hat{\theta} - \theta \sim N(0, \boldsymbol{\Sigma})
$$
这里协方差矩阵 $\boldsymbol{\Sigma}$ 是
$$
\boldsymbol{\Sigma} = \left[\begin{matrix} \mathrm{E}[(\bar{r}-\mu')^2] & \mathrm{E}[(\bar{r}-\mu')(s^2-\sigma^2)] \\[6pt] \mathrm{E}[(\bar{r}-\mu')(s^2-\sigma^2)] & \mathrm{E}[(s^2-\sigma^2)^2] \end{matrix}\right]
$$
下面列出矩阵中需要分别计算的三项

___

1. 样本均值的方差：

$$
\mathrm{E}[(\bar{r}-\mu')^2] = \frac{1}{n^2} \sum_{i=1}^n \mathrm{E} \left[(r_i - \mu')^2\right] = \frac{\sigma^2}{n}
$$

2. 样本方差的方差：

   $$
   \mathrm{E}[(s^2-\sigma^2)^2] = \frac{1}{(n-1)^2} \mathrm{E}\left\{ \left[ \sum_{i=1}^n\left( (r_i - \mu') - (\bar{r}-\mu') \right)^2 \right]^2 \right\} - \sigma^4
   $$

   将期望里的内容展开得到
   $$
   \begin{aligned}
   \mathrm{E}[(s^2-\sigma^2)^2] = \frac{1}{(n-1)^2} & \left\{ \sum_{i=1}^n \mathrm{E}[(r_i - \mu')^4] + \sum_{i\neq k} \mathrm{E}[(r_i - \mu')^2(r_k - \mu')^2] \right. \\[12pt]
   & \left. - 2n\sum_{i=1}^n \mathrm{E}\left[ (r_i - \mu')^2(\bar{r} - \mu')^2 \right] + n^2\mathrm{E}[(\bar{r} - \mu')^4] \right\} - \sigma^4
   \end{aligned}
   $$
   
   四个期望项分别是
   
   - $\mathrm{E}[(r_i - \mu')^4] = \kappa_4\sigma^4$ 
   
   - 对于 $i \neq k$ ，由独立性得出 $\mathrm{E}[(r_i - \mu')^2(r_k - \mu')^2] = \mathrm{E}[(r_i - \mu')^2]\mathrm{E}[(r_k - \mu')^2] = \sigma^4$ ；这样的 $(i,k)$ 一共有 $n(n-1)$ 个
   
   - 同样借由独立性，$\mathrm{E}\left[ (r_i - \mu')^2(\bar{r} - \mu')^2 \right]$ 可以计算如下
     $$
     \mathrm{E}\left[ (r_i - \mu')^2(\bar{r} - \mu')^2 \right] = \frac{1}{n^2} \left\{ \mathrm{E}\left[ (r_i - \mu')^4 \right] + \sum_{k\neq i}\mathrm{E}\left[ (r_i - \mu')^2(r_k - \mu')^2 \right] \right\} = \frac{\kappa_4 + (n-1)}{n^2}\sigma^4
     $$
     
   - 最后是 $\mathrm{E}[(\bar{r} - \mu')^4]$ ，展开并利用独立性得到
     $$
     \mathrm{E}[(\bar{r} - \mu')^4] = \frac{1}{n^4} \left\{ \sum_{i=1}^n \mathrm{E}\left[(r_i - \mu')^4\right] + 3\sum_{i\neq k} \mathrm{E}\left[(r_i - \mu')^2(r_k - \mu')^2\right] \right\} = \frac{\kappa_4}{n^3}\sigma^4 + \frac{3(n-1)}{n^3}\sigma^4
     $$
     这里只保留了 $4+0$ 和 $2+2$ 两种拆法，是因为 $3+1$ 这种拆法中一次项的期望值是零，因而没有任何贡献
   
   最终得到
   $$
   \begin{aligned}
   \mathrm{E}[(s^2-\sigma^2)^2] &= \frac{\sigma^4}{(n-1)^2} \left[ \left( n-2+\frac{1}{n} \right)(\kappa_4 + 3) + \left( n-2+\frac{3}{n} \right)(n-1) \right]  - \sigma^4 \\[12pt]
   &= \left[ \frac{\kappa_4}{n} - \frac{n-3}{n(n-1)} \right]\sigma^4
   \end{aligned}
   $$
   


3. 样本均值与样本方差的协方差：
   $$
   \mathrm{E}[(\bar{r}-\mu')(s^2-\sigma^2)] = \frac{1}{n(n-1)} \sum_{i=1}^n\sum_{k=1}^n \mathrm{E}\left\{ (r_i - \mu') [(r_k - \mu') - (\bar{r}-\mu')]^2 \right\}
   $$
   展开就是
   $$
   \mathrm{E}[(\bar{r}-\mu')(s^2-\sigma^2)] = \frac{1}{n(n-1)} \left\{ \sum_{i=1}^n\sum_{k=1}^n \mathrm{E}[(r_i - \mu')(r_k - \mu')^2] - n\sum_{i=1}^n\mathrm{E}[(r_i - \mu')(\bar{r} - \mu')^2] \right\}
   $$
   第一项中显然只能保留 $i=k$ 的项；第二项中将 $\bar{r} - \mu'$ 展开后只能保留 $3+0$ 这种拆法（理由和上面一样，$2+1$ 这种拆法的期望是零），因而计算化简后
   $$
   \mathrm{E}[(\bar{r}-\mu')(s^2-\sigma^2)] = \frac{\kappa_3\sigma^3}{n}
   $$

___

于是协方差矩阵最终是
$$
\boldsymbol{\Sigma} = \frac{1}{n}\left[\begin{matrix} \sigma^2 & \kappa_3\sigma^3 \\[6pt] \kappa_3\sigma^3 & [\kappa_4 - (n-3)/(n-1)]\sigma^4 \end{matrix}\right]
$$
如此一来，当 $n \rightarrow \infty$ 时，由一阶 Delta 方法得出 $\mathcal{S}(\hat{\theta}) - \mathcal{S}(\theta) \sim N\left( \mathrm{E}[\mathcal{S}(\hat{\theta})], \mathrm{Var}[\mathcal{S}(\hat{\theta})] \right)$ ，这里期望是
$$
\mathrm{E}(\hat{\mathcal{S}}) \rightarrow \mathcal{S}(\theta) + \frac{\partial \mathcal{S}}{\partial \hat{\theta}} \cdot\mathrm{E}\left[ (\hat{\theta} - \theta) \right] = \mathcal{S}(\theta)
$$
而方差是
$$
\mathrm{Var}(\hat{\mathcal{S}}) \rightarrow \left( \frac{\partial \mathcal{S}}{\partial \hat{\theta}} \right)^T \mathrm{E}\left[ (\hat{\theta} - \theta)(\hat{\theta} - \theta)^T \right]\left( \frac{\partial \mathcal{S}}{\partial \hat{\theta}} \right) = \left( \frac{\partial \mathcal{S}}{\partial \hat{\theta}} \right)^T \boldsymbol{\Sigma} \left( \frac{\partial \mathcal{S}}{\partial \hat{\theta}} \right)
$$
代入协方差矩阵，并考虑到此时 $(n-3)/(n-1) \rightarrow 1$ ，即得到
$$
\mathrm{Var}(\hat{\mathcal{S}}) \rightarrow \frac{1}{n} \left[ 1 - \kappa_3\frac{\mu' - r_f}{\sigma} + \frac{\kappa_4-1}{4}\left( \frac{\mu' - r_f}{\sigma} \right)^2 \right]
$$
考虑到真值 $\mathcal{S}(\theta) = (\mu' - r_f)/\sigma$ ，以及一般采用年化值，就得到

{% note primary %}

**独立非正态样本的几何 Sharpe 比率**：如果证券的对数收益样本序列是独立变量，记其年化几何 Sharpe 比率的估计值为 $\hat{\mathcal{S}}$ ，一年包含的样本数为 $n_\mathrm{A}$ ，那么当样本数量 $n \rightarrow \infty$ 时有
$$
\zeta = \sqrt{\frac{n}{n_\mathrm{A}}} \frac{\hat{\mathcal{S}} - \mathcal{S}}{V} \sim N \left( 0, 1 \right) \quad,\quad V^2 = 1 - \kappa_3\frac{\mathcal{S}}{\sqrt{n_\mathrm{A}}} + \frac{\kappa_4-1}{4}\cdot\frac{\mathcal{S}^2}{n_\mathrm{A}}
$$
这里 $\kappa_3$ 是样本分布的偏度，$\kappa_4$ 是分布的峰度，$\mathcal{S}$ 是年化几何 Sharpe 比率的真值

{% endnote %}

也就是说，通过设置不同的目标 Sharpe 比率 $\mathcal{S}_\mathrm{tar}$ ，上述命题可以构造一个 Z 检验问题

> **单边 Z 检验**：
>
> 1. 做出原假设 $H_0: \mathcal{S} \leq \mathcal{S}_\mathrm{tar}$ 
> 2. 在 $n$ 足够大的情况下，按照上述命题构造统计量 $\zeta$ （由于原假设认为 $\mathcal{S}$ 的真值不大于 $\mathcal{S}_\mathrm{tar}$ ，所以这里自然以 $\mathcal{S}_\mathrm{tar}$ 来计算 $V^2$ ）
> 3. 如果 $\zeta$ 大于等于标准正态分布的 $\alpha \%$ 分位数，那么就可以断言：有 $\alpha \%$ 的把握认为原假设不成立，因而证券的几何 Sharpe 比率 $\mathcal{S}$ 明显超过目标比率 $\mathcal{S}_\mathrm{tar}$ 

和正态情况一样，几何 Sharpe 比率越大，把握值 $\alpha \%$ 也越大，越能说明证券具有优秀的收益风险比


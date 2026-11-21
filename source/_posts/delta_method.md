---
title: 样本矩函数的渐近分布
date: 2026-11-20 22:32:49
categories: 时间序列处理
mathjax: true
description: 在大样本下样本矩的函数的渐近分布
---

# Delta 方法

## 一阶方法

一些统计量可以显式地表示为样本矩的函数，比如在[学生氏统计量的渐近分布](/2026/10/28/geometric_sharpe)中出现的几何 Sharpe 比率，就是两个样本矩 $\bar{r}$ 和 $s^2$ 的函数。Delta 方法要解决的是：在已知样本量 $n \rightarrow\infty$ 时样本矩的分布特性后，如何获知其函数的分布特性

记样本矩的真值向量为 $\theta$ ，估计值是 $\hat{\theta}$ ，并记 $\hat{\theta} - \theta \sim D(\theta, N)$ ，这里 $D$ 是一个可以先验地获知的概率分布。如果对于矩的估计是**相合**的，也就是说当样本量趋向于无穷时有 $\hat{\theta} - \theta \overset{\mathrm{p}}{\longrightarrow} 0$ ，那么对于一个函数 $g(\hat{\theta})$ ，就可以在真值附近对其进行 Taylor 展开

先保留到一阶的情况，有
$$
g(\hat{\theta}) = g(\theta) + \frac{\partial g}{\partial \hat{\theta}}^T(\theta) (\hat{\theta} - \theta) + o(|\hat{\theta} - \theta|)
$$
这里上标 $T$ 就是一般意义下的对梯度向量转置。于是上式可以给出

{% note primary %}

**一阶 Delta 方法**：对于某个统计量估计值 $\hat{\theta}$ 的函数 $g(\hat{\theta})$ ，若估计是相合的，且函数在 $\theta$ 处的梯度 $\partial g/\partial\theta$ 不为零，那么该函数与其真值之差则依分布收敛于
$$
g(\hat{\theta}) - g(\theta)\overset{\mathrm{d}}{\longrightarrow} \frac{\partial g}{\partial \hat{\theta}}^T(\theta) (\hat{\theta} - \theta)
$$

{% endnote %}

特别地，如果样本矩来源于独立同分布序列，那么在样本量不断增大时 $\hat{\theta} - \theta \sim N(0, \boldsymbol{C})$ ，这里 $\boldsymbol{C}$ 是样本矩之间的协方差矩阵。此时 $g(\hat{\theta}) - g(\theta)$ 将同样服从零期望的正态分布，即
$$
g(\hat{\theta}) - g(\theta) \sim N\left( 0, \frac{\partial g}{\partial \hat{\theta}}^T\boldsymbol{C}\frac{\partial g}{\partial \hat{\theta}} \right)
$$

## 二阶方法

一阶方法要求函数的梯度并不为零，即真值不位于极值点。而对于极值点处的情况，则涉及到二阶 Taylor 展开，即
$$
g(\hat{\theta}) = g(\theta) + \frac{1}{2}(\hat{\theta} - \theta)^T\frac{\partial^2 g}{\partial \hat{\theta}^2}(\theta) (\hat{\theta} - \theta) + o(|\hat{\theta} - \theta|^2)
$$
这里 $\partial^2 g/\partial \hat{\theta}^2$ 是 Hessian 矩阵。该式也就是

{% note primary %}

**二阶 Delta 方法**：对于某个统计量估计值 $\hat{\theta}$ 的函数 $g(\hat{\theta})$ ，若估计是相合的，且函数在 $\theta$ 处的梯度 $\partial g/\partial\theta$ 为零，但 Hessian 矩阵不为零，那么该函数与其真值之差则依分布收敛于
$$
g(\hat{\theta}) - g(\theta)\overset{\mathrm{d}}{\longrightarrow} \frac{1}{2}(\hat{\theta} - \theta)^T\frac{\partial^2 g}{\partial \hat{\theta}^2}(\theta) (\hat{\theta} - \theta)
$$

{% endnote %}

同样来考虑一下独立同分布的极限情况。将式子右侧记为 $\sum_{i,k} H_{ik} x_ix_k/2$ ，这里 $x_i = \hat{\theta}_i - \theta_i$ 是零均值正态变量，$H_{ik}$ 是 Hessian 矩阵。来看它的期望是
$$
\frac{1}{2}\sum_{i,k=1}^n H_{ik} \mathrm{E}(x_ix_k) = \frac{1}{2}\sum_{i,k=1}^n H_{ik} C_{ik} = \frac{1}{2}\sum_{i,k=1}^n H_{ik} C_{ki}
$$

而方差则是
$$
\begin{aligned}
\frac{1}{4}\mathrm{E}\left(\sum_{i,k=1}^n H_{ik} x_ix_k - \sum_{i,k=1}^n H_{ik} C_{ki}\right)^2 &= \frac{1}{4}\sum_{i,k,i',k'} H_{ik}H_{i'k'} \mathrm{E}[(x_ix_k - C_{ki})(x_{i'}x_{k'} - C_{k'i'})] \\[12pt]
&= \frac{1}{4}\sum_{i,k,i',k'} H_{ik}H_{i'k'} [\mathrm{E}(x_ix_kx_{i'}x_{k'}) - C_{ki}C_{k'i'}]
\end{aligned}
$$

利用实正态变量的 Isserlis 定理，上式成为
$$
\frac{1}{4}\sum_{i,k,i',k'} H_{ik}H_{i'k'} (C_{ii'}C_{kk'} + C_{ik'}C_{ki'}) = \frac{1}{4} \left( \sum_{i,k,i',k'} H_{ik}C_{kk'}C_{ii'}H_{i'k'} + \sum_{i,k,i',k'} H_{ik}C_{ki'}H_{i'k'} C_{k'i} \right)
$$

于是可以得到在这种情况下
$$
\mathrm{E}[g(\hat{\theta}) - g(\theta)] = \frac{1}{2}\mathrm{Tr}\left(\frac{\partial^2 g}{\partial \hat{\theta}^2} \boldsymbol{C}\right) \quad,\quad \mathrm{Var}[g(\hat{\theta}) - g(\theta)] = \frac{1}{4} \left[ \mathrm{Tr}\left(\boldsymbol{C}^T\frac{\partial^2 g}{\partial \hat{\theta}^2}^T\boldsymbol{C}\frac{\partial^2 g}{\partial \hat{\theta}^2} \right) + \mathrm{Tr}\left( \frac{\partial^2 g}{\partial \hat{\theta}^2}\boldsymbol{C}\frac{\partial^2 g}{\partial \hat{\theta}^2}\boldsymbol{C} \right) \right]
$$

# 几何 Sharpe 比率估计的标准误

## 样本方差的统计特性

几何 Sharpe 比率的估计值是样本均值与样本方差的函数 $\hat{\mathcal{S}} = (\bar{r} - r_f)/s$ ，为了得到它的标准误，要知道样本均值和样本方差的极限分布。以下的内容全部可以参照 *[M. Mertens, "Comments on Variance of the IID estimator in Lo (2002)," 2002.](https://www.elmarmertens.com/research/workingpapers)* 

在**独立同分布**假设下，样本均值的极限分布直接由 Lindeberg-Lévy CLT 给出，即 $\bar{r} - \mu \sim N(0, \sigma^2/n)$ 

对于样本方差，采用无偏估计和有偏估计都是相合的，然而一般来讲都考虑无偏估计。顾名思义，无偏是指 $\mathrm{E}(s^2) = \sigma^2$ ，它的方差则是
$$
\mathrm{Var}(s^2) = \mathrm{E}(s^4) - \sigma^4 = \mathrm{E}\left\{ \frac{1}{(n-1)^2}\sum_{i,k=1}^n [(r_i-\mu)-(\bar{r}-\mu)]^2 [(r_k-\mu)-(\bar{r}-\mu)]^2 \right\} - \sigma^4
$$
此即
$$
\mathrm{Var}(s^2) = \frac{1}{(n-1)^2} \left\{ \sum_{i,k=1}^n \mathrm{E}[(r_i-\mu)^2(r_k-\mu)^2] - 2n\mathrm{E}\left[ (\bar{r}-\mu)^2\sum_{i=1}^n (r_i-\mu)^2 \right] + n^2\mathrm{E}[(\bar{r}-\mu)^4] \right\} -\sigma^4
$$
利用独立同分布特性，第一项的结果是
$$
\sum_{i,k=1}^n \mathrm{E}[(r_i-\mu)^2(r_k-\mu)^2] = \sum_{i=1}^n \mathrm{E}[(r_i-\mu)^4] + \sum_{i\neq k} \mathrm{E}[(r_i-\mu)^2(r_k-\mu)^2] = \left[n\kappa_4 + n(n-1)\right]\sigma^4
$$
第二项是
$$
-2n\mathrm{E}\left[ (\bar{r}-\mu)^2\sum_{i=1}^n (r_i-\mu)^2 \right] = -\frac{2}{n} \sum_{i,k,m=1}^n \mathrm{E}\left[ (r_i-\mu)^2 (r_k-\mu)(r_m-\mu)\right] = -[2\kappa_4 + 2(n-1)]\sigma^4
$$
上式化简的关键在于，所有 $k\neq m$ 的项都会出现一个零期望，因而得到零。利用同样的思想，第三项是
$$
\begin{aligned}
n^2\mathrm{E}\left[ (\bar{r}-\mu)^4 \right] &= \frac{1}{n^4} \sum_{i,k,m,q=1}^n \mathrm{E}\left[ (r_i-\mu) (r_k-\mu)(r_m-\mu)(r_q-\mu)\right] \\[12pt]
&= n^2\frac{1}{n^4} \left\{ \sum_{i=1}^n \mathrm{E}[(r_i-\mu)^4] + 3\sum_{i\neq k} \mathrm{E}\left[ (r_i-\mu)^2 (r_k-\mu)^2\right] \right\} \\[12pt]
&= \frac{\kappa_4 + 3(n-1)}{n}\sigma^4
\end{aligned}
$$
代入得到
$$
\mathrm{Var}(s^2) = \frac{1}{(n-1)^2} \left[ \frac{(n-1)^2}{n}\kappa_4 + \frac{(n-1)^3}{n} + 2\frac{n-1}{n} \right]\sigma^4 - \sigma^4 = \frac{1}{n}\left( \kappa_4 - \frac{n-3}{n-1} \right)\sigma^4
$$
最后，还需要知道样本均值和样本方差之间的协方差，也即它们之间的相关性，来计算
$$
\mathrm{Cov}(\bar{r}, s^2) = \mathrm{E}[(\bar{r}-\mu)(s^2-\sigma^2)] = \frac{1}{n(n-1)}\sum_{i,k=1}^n \mathrm{E}\left\{(r_i-\mu)[(r_k-\mu) - (\bar{r}-\mu)]^2\right\} - \sigma^2\frac{1}{n}\sum_{i=1}^n \mathrm{E}(r_i-\mu)
$$
显然，最后一项的结果是零，所以
$$
\begin{aligned}
\mathrm{Cov}(\bar{r}, s^2) &= \frac{1}{n(n-1)}\left\{ \sum_{i,k=1}^n \mathrm{E}[(r_i-\mu)(r_k-\mu)^2] - n\sum_{i=1}^n \mathrm{E}[(r_i-\mu)(\bar{r}-\mu)^2] \right\} \\[12pt]
&= \frac{1}{n(n-1)}\left\{ \sum_{i,k=1}^n \mathrm{E}[(r_i-\mu)(r_k-\mu)^2] - \frac{1}{n}\sum_{i,k,m=1}^n \mathrm{E}[(r_i-\mu)(r_k-\mu)(r_m-\mu)] \right\}
\end{aligned}
$$
同样地，上式中只有 $i=k=m$ 的项才有非零贡献，因而
$$
\mathrm{Cov}(\bar{r}, s^2) = \frac{1}{n(n-1)} \left( n-1 \right)\kappa_3\sigma^3 = \frac{\kappa_3\sigma^3}{n}
$$
如此一来，在 $n\rightarrow\infty$ 时，样本均值和方差将服从
$$
\hat{\theta} - \theta = \left[\begin{matrix} \bar{r}-\mu \\ s^2-\sigma^2 \end{matrix}\right] \sim N(0,\boldsymbol{C}) \quad,\quad \boldsymbol{C} = \frac{1}{n}\left[\begin{matrix} \sigma^2 & \kappa_3\sigma^3 \\ \kappa_3\sigma^3 & (\kappa_4-1)\sigma^4 \end{matrix}\right]
$$

## 应用 Delta 方法

将 Delta 方法应用于几何 Sharpe 比率，首先来求其梯度
$$
\frac{\partial \hat{\mathcal{S}}}{\partial \hat{\theta}}(\theta) = \left[\begin{matrix} 1/\sigma \\ -(\mu-r_f)/2\sigma^3 \end{matrix}\right]
$$
Delta 方法首先表明估计值是渐近正态的
$$
\hat{\mathcal{S}} - \mathcal{S} \sim N(0, \mathrm{SE}^2)
$$
这里 $\mathrm{SE}$ 是估计的标准误，按照一阶 Delta 方法，它是
$$
\mathrm{SE}^2 = \frac{1}{n}\left[1 - \kappa_3\frac{\mu - r_f}{\sigma} + \frac{\kappa_4-1}{4}\left(\frac{\mu - r_f}{\sigma}\right)^2 \right] = \frac{1}{n}\left(1 - \kappa_3\mathcal{S} + \frac{\kappa_4-1}{4}\mathcal{S}^2 \right)
$$
事实上利用 Lagrange 中值定理，可以将一阶展开写为
$$
g(\hat{\theta}) - g(\theta) + \frac{\partial g}{\partial \hat{\theta}}^T(\tilde{\theta}) (\hat{\theta} - \theta)
$$
$\tilde{\theta}$ 是位于 $\hat{\theta}$ 和 $\theta$ 之间的某个值。由于当样本量增大时，$\hat{\theta} - \theta$ 依概率收敛到零，因而 $\tilde{\theta}, \hat{\theta}, \theta$ 在样本量足够大之时都是收敛到同一个值的。所以样本量足够大时，可以用估计值代替真值得到
$$
\mathrm{SE}^2 = \frac{1}{n}\left(1 - \hat{\kappa}_3\hat{\mathcal{S}} + \frac{\hat{\kappa}_4-1}{4}\hat{\mathcal{S}}^2 \right)
$$
标准误给出的是对这个值进行估计的误差大小；而[学生氏统计量的渐近分布](/2026/10/28/geometric_sharpe)中的分布 $F(\tau)$ 则是在零假设下，学生氏统计量 $\mathcal{T}$ 理论上应当服从的概率分布。这二者的意义并不相同

---
title: 有限样本下渐近分布的偏移
date: 2026-10-30 23:47:35
categories: 时间序列处理
mathjax: true
description: 在有限的样本量下实际的统计量分布与中心极限定理下标准正态分布之间的偏移
---

# Edgeworth 展开

Edgeworth 展开是对中心极限定理（CLT）的补充，区别只在于它保留了更多的余项。接下来始终假设涉及到的矩都是收敛的，导函数则都是存在的

{% note primary %}

**Lindberg-Lévy 中心极限定理**：对于一个期望为 $\mu'$ ，方差为 $\sigma^2$ 的独立同分布样本序列 $\{r_i\}$ ，随着样本数量 $n$ 增大，样本均值的分布趋向于 $N(\mu',\sigma^2/n)$ ，或者说可以写为
$$
\lim_{n \rightarrow \infty} z_n = \frac{\bar{r} - \mu'}{\sigma/\sqrt{n}} \sim N(0,1)
$$
{% endnote %}

定理最常见的证明需要用到特征函数。令 $y_i = (r_i-\mu')/\sigma$ ，首先就有
$$
\mathrm{E}(y_i) = 0 \quad,\quad \mathrm{E}(y_i^2) = \mathrm{Var}(y_i) = 1 \quad,\quad \mathrm{E}(y_i^3) = \mathrm{Skew}(r_i) = \kappa_3 \quad,\quad \mathrm{E}(y_i^4) = \mathrm{Kurt}(r_i) = \kappa_4
$$
设它的概率密度函数是 $p(y)$ ，那么特征函数是 $M(\omega) = \mathrm{E}[e^{j\omega y}] = \mathcal{F}^{-1}[p(y)]$ ，即概率密度函数的 Fourier（逆）变换。如此一来，利用 $\{y_i\}$ 的独立同分布特性，$z_n = \sqrt{n}\bar{y}$ 的特征函数是
$$
M_z(\omega) = \mathrm{E}\left[ e^{-j\omega \sqrt{n}\bar{y}} \right] = \prod_{i=1}^n \left[ \int_{-\infty}^{+\infty} p(y_i)e^{j\omega y_i/\sqrt{n}} \mathrm{d}y_i \right] = \left[ \int_{-\infty}^{+\infty} p(y)e^{j\omega y/\sqrt{n}} \mathrm{d}y \right]^n
$$
对指数函数部分进行 Taylor 展开，得到
$$
M_z(\omega) = \left[ 1 + \sum_{k=1}^\infty \frac{(j\omega)^k}{k!n^{k/2}} \mathrm{E}(y^k) \right]^n = \exp\left[ -\frac{\omega^2}{2} + \frac{(j\omega)^3}{6\sqrt{n}}\kappa_3 + \frac{(j\omega)^4}{24n}\kappa_4 +o(n^{-1})\right]
$$
第二个等号是重要极限 $e = \lim_{n\rightarrow\infty}(1+1/n)^n$ 的结果。再将 $e^{-\omega^2/2}$ 提出来，并且进行一次 Taylor 展开得到
$$
M_z(\omega) = e^{-\omega^2/2}\left[ 1 + \frac{(j\omega)^3}{6\sqrt{n}}\kappa_3 + \frac{(j\omega)^4}{24n}\kappa_4 + \frac{(j\omega)^6}{72n}\kappa_3^2 + o(n^{-1}) \right]
$$

Gauss 函数 $e^{-\omega^2/2}$ 的 Fourier 变换正是标准正态分布的概率密度函数 $\phi(z)$ ；所以如果上式不保留带有 $n$ 的这些余项，就得到了 Lindberg-Lévy CLT. 

Edgeworth 展开则是继续保留后面的余项，特别地，这里保留到二阶余项。考虑到 Fourier 变换中 $ \mathrm{d}^k\phi/\mathrm{d}z^k \leftrightarrow (-j\omega)^k \mathcal{F}^{-1}(\phi)$ 的对应关系，保留更多余项后的概率分布函数就是
$$
p(z) = \phi(z) - \frac{\kappa_3}{6\sqrt{n}} \frac{\mathrm{d}^3\phi}{\mathrm{d}z^3} + \frac{\kappa_4}{24n} \frac{\mathrm{d}^4\phi}{\mathrm{d}z^4} + \frac{\kappa_3^2}{72n} \frac{\mathrm{d}^6\phi}{\mathrm{d}z^6} + o(n^{-1})
$$
至于这些导数，按照定义就是 Hermite 多项式
$$
H_k(z) = (-1)^k \frac{1}{\phi}\frac{\mathrm{d}^k\phi}{\mathrm{d}z^k} = (-1)^k e^{z^2/2} \frac{\mathrm{d}^k e^{-z^2/2}}{\mathrm{d}z^k}
$$

> 在量子力学中，提到 Hermite 多项式时一般约定为 $(-1)^k e^{z^2}(\mathrm{d}^k e^{-z^2}/\mathrm{d}z^k)$ ；统计学中则约定为 $(-1)^k e^{z^2/2}(\mathrm{d}^k e^{-z^2/2}/\mathrm{d}z^k)$ ，二者略有不同，主要都是为了方便

所以如果用 Hermite 多项式来表示，就有

{% note primary %}

**二阶 Edgeworth 展开**：对于一个独立同分布样本序列 $\{r_i\}$ ，记其期望、方差、偏度、峰度分别为 $\mu', \sigma^2, \kappa_3, \kappa_4$ ，那么统计量 $z = \sqrt{n}(\bar{r}-\mu')/\sigma$ 的概率密度函数是
$$
p(z) = \phi(z) + \frac{\kappa_3}{6\sqrt{n}}H_3(z) + \frac{\kappa_4}{24n}H_4(z) + \frac{\kappa_3^2}{72n}H_6(z) + o(n^{-1})
$$
这里 $\phi(z)$ 是标准正态分布的概率密度函数，$H_k(z)$ 是统计学中约定的 Hermite 多项式

{% endnote %}

# 几何 Sharpe 比率的渐近分布

## 特征函数

接下来考虑有限样本下，Edgeworth 修正对基于几何 Sharpe 比率的假设检验带来的影响。以下始终作原假设 $H_0: \mu'=r_f$ ，于是学生氏统计量是
$$
\tau = \sqrt{n}\hat{\mathcal{S}} = \sqrt{n}\frac{\bar{r} - \mu'}{s} = \sqrt{n}\left( \frac{\sigma}{s} \right)\frac{\bar{r} - \mu'}{\sigma}
$$
考虑到样本方差的计算式是
$$
s^2 = \frac{1}{n-1}\sum_{i=1}^n (r_i - \bar{r})^2 = \left(\frac{n\sigma^2}{n-1}\right) \left[\frac{1}{n}\sum_{i=1}^n \left(\frac{r_i-\mu'}{\sigma}\right)^2 - \left(\frac{\bar{r}-\mu'}{\sigma}\right)^2\right] = \sigma^2\left( 1 + \frac{1}{n-1} \right)\left(\overline{y^2} - \bar{y}^2\right)
$$
这里仍然记 $y_i = (r_i - \mu')/\sigma$ ，并将 $y_i^2$ 的样本均值记为 $\overline{y^2}$ 

## 累积分布函数

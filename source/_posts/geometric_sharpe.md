---
title: 学生氏统计量的渐近分布
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
\mathcal{T} = \frac{\bar{r} - \mu'}{s/\sqrt{n}} \sim t(n-1) \rightarrow N(0,1)
$$

极限在 $n \rightarrow \infty$ 时取到，这是学生氏分布的性质

举个例子，样本序列是 $\{ r_i\}$ ，一年中有 $n_\mathrm{A}$ 个样本，那么年化 Sharpe 比率的估计值是
$$
\hat{\mathcal{S}} = \sqrt{n_\mathrm{A}}\frac{\bar{r} - r_f}{s} = \sqrt{\frac{n_\mathrm{A}}{n}}\frac{\bar{r} - r_f}{s/\sqrt{n}} = \sqrt{\frac{n_\mathrm{A}}{n}}\mathcal{T} \quad\mathrm{when}\quad \mu' = r_f
$$
也就是说

{% note primary %}

**独立正态样本的几何 Sharpe 比率**：如果证券的对数收益样本序列是独立的正态变量，且其期望与无风险收益相等 $\mu' = r_{f}$ ，记其年化的几何 Sharpe 比率估计值为 $\hat{\mathcal{S}}$ ，一年包含的样本数为 $n_\mathrm{A}$ ，那么
$$
\mathcal{T} = \sqrt{\frac{n}{n_\mathrm{A}}} \hat{\mathcal{S}} \sim t(n-1)
$$
{% endnote %}

如此一来，可以利用该值进行**单边学生氏检验**。并且显然几何 Sharpe 比率越大，置信度也越大，越能说明证券具有高的超额收益。不过这里有一个问题：

**收益率的样本往往不服从正态分布，而是具有负的偏度和正的超额峰度**

当样本数足够大时，样本均值和样本方差都服从正态分布，学生氏统计量 $\mathcal{T}$ 作为它们的函数按照 Delta 方法也服从正态分布，因而检验问题退化为 Z 检验。但问题就在于：对于需要精准把控的置信度来说，样本量往往是不够大的（10 年也就才 3000 多天，更不用说许多证券上市可能甚至就几周）

这时就需要考虑 $\mathcal{T}$ 实际的渐近分布，这分为两步进行：首先考虑方差 $\sigma^2$ 能先验获得的情况，这时学生氏统计量退化为 Z 统计量；然后考虑用样本方差 $s^2$ 估计真实方差的情况

下面的内容均可参照 <span style="color:#417FBE; font-style:italic">P. Hall, "The Bootstrap and Edgeworth Expansion," Chap. 2, Sec. 6.</span> 以及后来的 <span style="color:#417FBE; font-style:italic">M. Nkurunziza and L. Vermeire, "A Review on Approximation Approach for the Distribution of the Studentized Mean," Appl. Math. Sci., vol. 17, pp. 297(2023).</span> 

# Z 统计量的渐近分布

## Edgeworth 展开

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
设它的概率密度函数是 $p(y)$ ，那么特征函数是 $\varphi(\omega) = \mathrm{E}[e^{-j\omega y}] = \mathcal{F}[p(y)]$ ，即概率密度函数的 Fourier 变换。如此一来，利用 $\{y_i\}$ 的独立同分布特性，$z_n = \sqrt{n}\bar{y}$ 的特征函数是
$$
\varphi_z(\omega) = \mathrm{E}\left[ e^{-j\omega \sqrt{n}\bar{y}} \right] = \prod_{i=1}^n \left[ \int_{-\infty}^{+\infty} p(y_i)e^{-j\omega y_i/\sqrt{n}} \mathrm{d}y_i \right] = \left[ \int_{-\infty}^{+\infty} p(y)e^{-j\omega y/\sqrt{n}} \mathrm{d}y \right]^n
$$
对指数函数部分进行 Taylor 展开，得到
$$
\varphi_z(\omega) = \left[ 1 + \sum_{k=1}^\infty \frac{(-j\omega)^k}{k!n^{k/2}} \mathrm{E}(y^k) \right]^n = \exp\left[ -\frac{\omega^2}{2} - \frac{(j\omega)^3}{6\sqrt{n}}\kappa_3 + \frac{(j\omega)^4}{24n}\kappa_4 +o(n^{-1})\right]
$$
第二个等号是重要极限 $e = \lim_{n\rightarrow\infty}(1+1/n)^n$ 的结果。再将 $e^{-\omega^2/2}$ 提出来，并且进行一次 Taylor 展开得到
$$
\varphi_z(\omega) = e^{-\omega^2/2}\left[ 1 - \frac{(j\omega)^3}{6\sqrt{n}}\kappa_3 + \frac{(j\omega)^4}{24n}\kappa_4 + \frac{(j\omega)^6}{72n}\kappa_3^2 + o(n^{-1}) \right]
$$

Gauss 函数 $e^{-\omega^2/2}$ 的 Fourier 变换正是标准正态分布的概率密度函数 $\phi(z)$ ；所以如果上式不保留带有 $n$ 的这些余项，就得到了 Lindberg-Lévy CLT. 

Edgeworth 展开则是继续保留后面的余项，特别地，这里保留到二阶余项。考虑到 Fourier 变换中 $ \mathrm{d}^k\phi/\mathrm{d}z^k \overset{\mathcal{F}}{\longleftrightarrow} (j\omega)^k \mathcal{F}(\phi)$ 的对应关系，保留更多余项后的概率分布函数就是
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
p(z) = \phi(z)\left[1 + \frac{\kappa_3}{6\sqrt{n}}H_3(z) + \frac{\kappa_4}{24n}H_4(z) + \frac{\kappa_3^2}{72n}H_6(z) + o(n^{-1})\right]
$$
这里 $\phi(z)$ 是标准正态分布的概率密度函数，$H_k(z)$ 是统计学中约定的 Hermite 多项式

{% endnote %}

Edgeworth 给出了 Z 统计量的渐近分布，利用同样的思路——即对样本数量 $n$ 做级数展开——也可以得到学生氏统计量的渐近分布

## 累积生成函数

累积生成函数（CGF）是特征函数的自然对数 $K(\omega) = \ln \varphi(\omega)$ ，这里 $\varphi(\omega) = \mathrm{E}(e^{-j\omega y}) = \mathcal{F}[p(y)]$ 是特征函数，即概率密度函数 $p(y)$ 的 Fourier变换。将变换进行两次 Taylor 展开，得到
$$
K(\omega) = \ln \left[\int_{-\infty}^{+\infty} p(y) e^{-j\omega y}\mathrm{d}y\right] = \ln \left[1 + \sum_{k=1}^\infty \frac{(-j\omega)^k}{k!} \mathrm{E}(z^k) \right] = \sum_{m=1}^\infty \frac{(-1)^{m-1}}{m}\left[\sum_{k=1}^\infty \frac{(-j\omega)^k}{k!} \mathrm{E}(z^k) \right]^m
$$

CGF 最终展开的系数 $c_k$ 也被称为**累积量**，即
$$
K(\omega) = \sum_{k=1}^\infty \frac{c_k}{k!} (-j\omega)^k
$$
来考虑比较常用的前四阶累积量，用组合数的方式可以得出它们分别是
$$
\begin{aligned}
& c_1 = E(y) \quad,\quad c_2 = E(y^2) - E(y)^2 \\[6pt]
& c_3 = E(y^3) - 3E(y^2)E(y) +2E(y)^3 \quad,\quad c_4 = E(y^4) - 4E(y^3)E(y) - 3E(y^2)^2 + 12E(y^2)E(y)^2 - 6E(y)^4
\end{aligned}
$$
由于 CLT ，样本均值总会收敛到正态分布。而正如 Edgeworth 展开所表现的，这时它的特征函数表现为 $\varphi(\omega) = r_n(\omega)e^{-\omega^2/2}$ ，那么 CGF 就是
$$
K(\omega) = -\frac{\omega^2}{2} + \ln r_n(\omega)
$$
要估计它与标准正态分布之间的分布差异，只需要得出 $\ln r_n(\omega)$ 就可以了，而它可以由前几阶累积量进行估计

# 学生氏估计的渐近分布

## Delta 方法

接下来考虑有限样本下，Edgeworth 修正对学生氏检验带来的影响。以下始终作原假设 $H_0: \mu'=r_f$ ，于是学生氏统计量是

$$
\mathcal{T} = \sqrt{n}\hat{\mathcal{S}} = \sqrt{n}\frac{\bar{r} - \mu'}{s}
$$
按照定义，样本方差的计算式是
$$
s^2 = \frac{1}{n-1}\sum_{i=1}^n (r_i - \bar{r})^2 = \frac{n\sigma^2}{n-1} \left[ \frac{1}{n}\sum_{i=1}^n \left(\frac{r_i-\mu'}{\sigma}\right)^2 - \left(\frac{\bar{r}-\mu'}{\sigma}\right)^2\right] = \frac{n\sigma^2}{n-1} \left( 1 + \bar{x} - \bar{y}^2 \right)
$$

这里和前面一样定义了 $y_i = (r_i-\mu')/\sigma$ ，与此同时还有 $x_i = y_i^2 - 1$​ 

___

构造辅助统计量 $z_n = \sqrt{n}\bar{y}$ 以及 $v_n = \sqrt{n}\bar{x}$ ；那么 $\mathrm{E}(z_n) = 0$ ，$\mathrm{Var}(z_n) = 1$ 以及 $n \rightarrow\infty$ 时 $z_n \sim N(0,1)$ 就都是老生常谈的结果

至于 $v_n$ ，利用独立性很快算出它的期望和方差分别是
$$
\mathrm{E}(v_n) = \mathrm{E}(x_i) = 0 \quad,\quad \mathrm{Var}(v_n) = \mathrm{Var}(x_i) = \left[ \mathrm{E}(y_i^4) - \mathrm{E}(y_i^2)^2 \right] = \kappa_4 - 1
$$
以及毫无疑问 CLT 也能给出 $n \rightarrow\infty$ 时 $v_n \sim N(0,\kappa_4-1)$ ；既如此 $z_n, v_n$ 趋向于一个联合正态分布，还缺少一个描述这个分布的信息就是协方差，即
$$
\mathrm{Cov}(z_n, v_n) = \mathrm{Cov}(y_i, x_i) = \frac{1}{n} \sum_{i=1}^n \left[\mathrm{E}(y_i^3) - \mathrm{E}(y_i)\right] = \kappa_3
$$

更高阶的矩也可以用独立性和相关性展开即可求得

___

这样一来学生氏统计量是
$$
\mathcal{T} = z_n \left( 1-\frac{1}{n} \right)^{1/2} \left( 1 + \frac{v_n}{\sqrt{n}} - \frac{z_n^2}{n} \right)^{-1/2} = z_n - \frac{z_nv_n}{2}n^{-1/2} + \left(\frac{z_n^3}{2} + \frac{3z_nv_n^2}{8} - \frac{z_n}{2} \right)n^{-1} + o(n^{-1})
$$
将上式记为记 $\mathcal{T} = U + o(n^{-1})$ ，由于 $P(\mathcal{T} \leq \tau) = P(U \leq \tau-o(n^{-1}))$ ，所以在 $n^{-1}$ 这个精度下只需要计算 $U$ 的特征函数，并反演回概率密度函数或者累积分布函数即可。事实上这就是 **Delta 方法** 

## 特征函数的计算

为了知道 $U$ 的特征函数，并在 Edgeworth 展开中保留到 $n^{-1}$ ，需要计算 $U$ 的前四阶原点矩 $\mathrm{E}(U^k)$ 

1. 一阶中心矩：这里涉及到 $\mathrm{E}(z_n^3) = \kappa_3n^{-1/2}$ ，计算得到
   $$
   \mathrm{E}(U) = -\frac{\kappa_3}{2}n^{-1/2} + o(n^{-1})
   $$

2. 二阶中心矩：首先考虑保留的阶数，展开得到
   $$
   \mathrm{E}(U^2) = \mathrm{E}\left[ z_n^2 - z_n^2v_n n^{-1/2} + \left(z_n^4 + z_n^2v_n^2 - z_n^2 \right)n^{-1} \right] + o(n^{-1})
   $$
   这里涉及到 $\mathrm{E}(z_n^4) = 3 + (\kappa_4-3)n^{-1}$ ，$\mathrm{E}(z_n^2v_n) = (\kappa_4-1)n^{-1/2}$ ，以及 $\mathrm{E}(z_n^2v_n^2) = (\kappa_4-1) + 2\kappa_3^2 + o(1)$ ，如此一来得到
   $$
   \mathrm{E}(U^2) = \left[ 1 + 2(1+\kappa_3^2)n^{-1} \right] + o(n^{-1})
   $$

3. 三阶中心矩：同样，先考虑保留的阶数，展开得到
   $$
   \mathrm{E}(U^3) = \mathrm{E}\left[ z_n^3 - \frac{3}{2}z_n^3v_n n^{-1/2} + \left(\frac{3}{2}z_n^5 + \frac{15}{8}z_n^3v_n^2 - \frac{3}{2}z_n^3 \right)n^{-1} \right] + o(n^{-1})
   $$
   对 $n^{-1}$ 项的系数取期望后都是 $o(1)$ 的，所以这一项没有贡献，要考虑的只有 $\mathrm{E}(z_n^3v_n) = 3\kappa_3 + o(n^{-1/2})$ ，得到
   $$
   \mathrm{E}(U^3) = -\frac{7}{2}\kappa_3 n^{-1/2} + o(n^{-1})
   $$

4. 四阶中心矩：考虑保留的阶数，展开得到
   $$
   \mathrm{E}(U^4) = \mathrm{E}\left[ z_n^4 - 2z_n^4v_n n^{-1/2} + \left(2z_n^6 + 3z_n^4v_n^2 - 2z_n^4 \right)n^{-1}\right] + o(n^{-1})
   $$
   这里涉及到 $\mathrm{E}(z_n^4v_n) = [4\kappa_3^2 + 6(\kappa_4-1)]n^{-1/2} + o(n^{-1})$ ，$\mathrm{E}(z_n^4v_n^2) = 3(\kappa_4-1) + 12\kappa_3^2 + o(1)$ ，以及 $\mathrm{E}(z_n^6) = 15 + o(n^{-1/2})$ ，于是
   $$
   \mathrm{E}(U^4) = \left[ 3+\left(24 - 2\kappa_4 + 28\kappa_3^2 \right)n^{-1}\right] + o(n^{-1})
   $$

接下来先计算累积量，然后得到 CGF ，前四阶累积量是

$$
\begin{aligned}
& c_1 = -\frac{\kappa_3}{2}n^{-1/2} + o(n^{-1}) \quad,\quad c_2 = \left[ 1 + \left(2 + \frac{7}{4}\kappa_3^2 \right)n^{-1} \right] + o(n^{-1}) \\[12pt]
& c_3 = -2\kappa_3 n^{-1/2} + o(n^{-1}) \quad,\quad c_4 = \left(12 -2\kappa_4 + 12\kappa_3^2\right)n^{-1} + o(n^{-1})
\end{aligned}
$$

如此一来，CGF 就是
$$
K_t(\omega) = -\frac{\omega^2}{2} + \left[ \frac{j\omega}{2} +\frac{(j\omega)^3}{3}  \right] \kappa_3 n^{-1/2} + \left[ -\left(1 + \frac{7}{8}\kappa_3^2 \right)\omega^2 + \left(\frac{1}{2} - \frac{\kappa_4}{12} +\frac{\kappa_3^2}{2}\right)\omega^4 \right] n^{-1} + o(n^{-1})
$$
只要对 CGF 取指数就可以得到特征函数 $\varphi_t(\omega) = \exp[K_t(\omega)]$ ；并且在这里先将对应标准正态的部分 $e^{-\omega^2/2}$ 提出来，将余下的部分记作 $k_{-1/2}(\omega)n^{-1/2} + k_{-1}(\omega)n^{-1} + o(n^{-1})$ 并 Taylor 展开就有
$$
\varphi_t(\omega) = e^{-\omega^2/2} \left[ 1 + k_{-1/2}(\omega)n^{-1/2} + k_{-1}(\omega)n^{-1} + \frac{1}{2}k_{-1/2}(\omega)^2n^{-1} \right] + o(n^{-1})
$$
代入合并，得到最终的特征函数
$$
\begin{aligned}
\varphi_t(\omega) = e^{-\omega^2/2} & \left\{ 1 + \frac{\kappa_3}{6}\left[  3j\omega + 2(j\omega)^3 \right]n^{-1/2} \right. \\[12pt]
&+\left. \left[-\left(1 + \kappa_3^2 \right)\omega^2  + \left(\frac{1}{2} - \frac{\kappa_4}{12} +\frac{2\kappa_3^2}{3}\right)\omega^4 - \frac{\kappa_3^2}{18} \omega^6\right] n^{-1} \right\} + o(n^{-1})
\end{aligned}
$$

## 累积分布函数

既然有了 $\varphi_t(\omega)$ ，那么学生氏统计量 $\tau$ 的概率密度函数 $p(\tau) = \mathcal{F}^{-1}[\varphi_t(\omega)]$ 也就确定了。不过在假设检验中，实际上一般关心的是落在某个值左侧或右侧的概率是多少，因此如果得出累积分布函数——即概率密度函数的积分——是更好的

利用 Fourier 变换的关系 $\int\phi(\tau)\mathrm{d}\tau \overset{\mathcal{F}}{\longleftrightarrow} (j\omega)^{-1} \mathcal{F}(\phi)$ ，累积分布函数 $F(\tau)$ 的 Fourier 变换将是
$$
\mathcal{F}[F(\tau)] = \mathcal{F}\left[ \int_{-\infty}^{\tau} p(\xi) \mathrm{d}\xi\right] = \frac{\varphi_t(\omega)}{j\omega}
$$
将上面 $\varphi_t(\omega)$ 的表达式代入之后，再逐项 Fourier 反演回来就有
$$
\begin{aligned}
F(\tau) = & \int_{-\infty}^\tau \phi(\xi)\mathrm{d}\xi + \frac{\kappa_3}{6}\left[ 3\phi(\tau) + 2\frac{\mathrm{d}^2\phi}{\mathrm{d}\tau^2} \right]n^{-1/2} \\[12pt]
&+ \left[ (1+\kappa_3^2)\frac{\mathrm{d}\phi}{\mathrm{d}\tau} + \left( \frac{1}{2} - \frac{\kappa_4}{12} + \frac{2\kappa_3^2}{3} \right)\frac{\mathrm{d}^3\phi}{\mathrm{d}\tau^3} + \frac{\kappa_3^2}{18}\frac{\mathrm{d}^5\phi}{\mathrm{d}\tau^5} \right] n^{-1} + o(n^{-1})
\end{aligned}
$$
自然，这里又可以用 Hermite 多项式来表达，这就有

{% note primary %}

**学生氏统计量的渐近分布**：对于一个独立同分布样本序列 $\{r_i\}$ ，记其期望、方差、偏度、峰度分别为 $\mu', \sigma^2, \kappa_3, \kappa_4$ ，那么学生氏统计量 $\tau = \sqrt{n}(\bar{r}-\mu')/s$ 的累积分布函数是
$$
F(\tau) = \Phi(\tau) + \left[q_{-1/2}(\tau)n^{-1/2} + q_{-1}(\tau)n^{-1}\right]\phi(\tau)  + o(n^{-1})
$$
这里 $\phi(\tau)$ 是标准正态分布的概率密度函数，$\Phi(\tau)$ 是标准正态分布的累积分布函数，两个多项式分别是
$$
q_{-1/2}(\tau) = \frac{\kappa_3}{6}\left[ 3 + 2H_2(\tau) \right] \quad,\quad q_{-1}(\tau) = - \left[ (1+\kappa_3^2)H_1(\tau) + \left( \frac{1}{2} - \frac{\kappa_4}{12} + \frac{2\kappa_3^2}{3} \right)H_3(\tau) + \frac{\kappa_3^2}{18}H_5(\tau) \right]
$$
$H_k(\tau)$ 是统计学中约定的 Hermite 多项式

{% endnote %}

或者可以代入前几阶 Hermite 多项式的具体形式
$$
\begin{aligned}
& H_1(\tau) = \tau \quad,\quad H_2(\tau) = \tau^2 - 1 \quad,\quad H_3(\tau) = \tau^3-3\tau \\[6pt]
& H_4(\tau) = \tau^4 - 6\tau^2 + 3 \quad,\quad H_5(\tau) = \tau^5 - 10\tau^3 + 15\tau \quad,\quad \cdots
\end{aligned}
$$
得到
$$
q_{-1/2}(\tau) = \frac{\kappa_3}{6}\left( 2\tau^2 - 1 \right) \quad,\quad q_{-1}(\tau) = -\frac{1}{2}(\tau^3 - \tau) +\frac{\kappa_4}{12}(\tau^3 - 3\tau) - \frac{\kappa_3^2}{18}(\tau^5 + 2\tau^3 - 3\tau)
$$
于是就有数量有限、任意样本分布下的学生氏检验

> **单边学生氏检验**：
>
> 1. 做出原假设 $H_0: \mu' \leq r_f$ 
> 2. 统计 $\bar{r}, s^2, \kappa_3, \kappa_4$ ，计算 $\tau = \sqrt{n/n_\mathrm{A}} \hat{\mathcal{S}}$ 以及累积分布函数 $F$ 
> 3. 如果 $F(\tau) \geq \alpha\%$ ，那么就可以断言：有 $\alpha \%$ 的置信度认为原假设不成立，因而证券的期望收益 $\mu'$ 明显超过无风险收益

最后，来验证两个特殊情况：

- 样本来自于正态分布，即 $\kappa_3=0, \kappa_4=3$ ，此时 $\tau \sim t(n-1)$ ，它的累积分布函数是

  $$
  F(\tau) = \Phi(\tau) - \left[H_1(\tau) + \frac{H_3(\tau)}{4} \right]\phi(\tau)n^{-1} + o(n^{-1}) = \Phi(\tau) - \frac{1}{4}(\tau^3+\tau)\phi(\tau)n^{-1} + o(n^{-1})
  $$

- 样本数量趋于无穷，即 $n \rightarrow \infty$ ，此时 $\tau$ 当然趋向于 $N(0,1)$ ，它的累积分布函数是 $F(\tau) = \Phi(\tau)$ 

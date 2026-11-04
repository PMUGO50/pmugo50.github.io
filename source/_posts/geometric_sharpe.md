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

# 一般的渐近分布

## 特征函数

接下来考虑有限样本下，Edgeworth 修正对学生氏检验带来的影响，这部分内容参照 <span style="color:#417FBE; font-style:italic">M. Nkurunziza and L. Vermeire, "A Review on Approximation Approach for the Distribution of the Studentized Mean," Appl. Math. Sci., vol. 17, pp. 297(2023).</span> 

以下始终作原假设 $H_0: \mu'=r_f$ ，于是学生氏统计量是
$$
\tau = \sqrt{n}\hat{\mathcal{S}} = \sqrt{n}\frac{\bar{r} - \mu'}{s} = \sqrt{n}\left( \frac{\sigma}{s} \right)\frac{\bar{r} - \mu'}{\sigma}
$$
按照定义，样本方差的计算式是
$$
s^2 = \frac{1}{n-1}\sum_{i=1}^n (r_i - \bar{r})^2 = \frac{n\sigma^2}{n-1} \left[ \frac{1}{n}\sum_{i=1}^n \left(\frac{r_i-\mu'}{\sigma}\right)^2 - \left(\frac{\bar{r}-\mu'}{\sigma}\right)^2\right] = \frac{n\sigma^2}{n-1} \left( 1 + \frac{1}{\sqrt{n}}a_n - \frac{1}{n}z_n^2 \right)
$$

___

这里和前面一样仍然记 $z_n = \sqrt{n}(\bar{r}-\mu')/\sigma$ ；那么 $\mathrm{E}(z_n) = 0$ ，$\mathrm{Var}(z_n) = 1$ 以及 $n \rightarrow\infty$ 时 $z_n \sim N(0,1)$ 就都是老生常谈的结果

另外一个辅助量则定义为
$$
u_n = \frac{1}{\sqrt{n}} \sum_{i=1}^n x_i \quad,\quad x_i = y_i^2 - 1 \quad,\quad y_i = \frac{r_i-\mu'}{\sigma}
$$
利用独立性很快算出它的期望和方差分别是
$$
\mathrm{E}(u_n) = \mathrm{E}(x_i) = 0 \quad,\quad \mathrm{Var}(u_n) = \mathrm{Var}(x_i) = \left[ \mathrm{E}(y_i^4) - \mathrm{E}(y_i^2)^2 \right] = \kappa_4 - 1
$$
以及毫无疑问 CLT 也能给出 $n \rightarrow\infty$ 时 $u_n \sim N(0,\kappa_4-1)$ ；既如此 $z_n, u_n$ 趋向于一个联合正态分布，还缺少一个描述这个分布的信息就是协方差，即
$$
\mathrm{Cov}(z_n, u_n) = \mathrm{Cov}(y_i, x_i) = \frac{1}{n} \sum_{i=1}^n \left[\mathrm{E}(y_i^3) - \mathrm{E}(y_i)\right] = \kappa_3
$$

___

于是就得到
$$
\begin{aligned}
\frac{\sigma}{s} = \sqrt{1-\frac{1}{n}} \left( 1 + \frac{1}{\sqrt{n}}u_n - \frac{1}{n}z_n^2 \right)^{-1/2} &= \left[ 1 - \frac{1}{2n} + o(n^{-1}) \right] \left[ 1 -\frac{1}{2\sqrt{n}}u_n + \frac{1}{2n}z_n^2 + \frac{3}{8n}u_n^2 + o(n^{-1}) \right] \\[12pt]
&= 1 - \frac{u_n}{2}n^{-1/2} + \left(\frac{z_n^2}{2} + \frac{3u_n^2}{8} - \frac{1}{2} \right)n^{-1} + o(n^{-1})
\end{aligned}
$$
这样一来将其代入，学生氏统计量就成为
$$
\tau = z_n - \frac{z_nu_n}{2}n^{-1/2} + \left(\frac{z_n^3}{2} + \frac{3z_nu_n^2}{8} - \frac{z_n}{2} \right)n^{-1}  + o(n^{-1})
$$
并且来求它的特征函数 $\varphi_t(\omega) = \mathrm{E}(e^{-j\omega\tau})$，就是
$$
\varphi_t(\omega)  = \mathrm{E}\left\{ e^{-j\omega z_n} \left[1 + j\omega\frac{z_nu_n}{2}n^{-1/2} - j\omega\left(\frac{z_n^3}{2} + \frac{3z_nu_n^2}{8} - \frac{z_n}{2} \right)n^{-1} -\omega^2\frac{z_n^2u_n^2}{8}n^{-1} + o(n^{-1})\right] \right\}
$$

## 统计矩的计算

特征函数的计算依赖于许多形如 $\mathrm{E}( e^{-j\omega z_n}z_n^k u_n^m )$ 的项，可以通过联合的特征函数来求这个期望，也就是说令 $x_i = y_i^2 - 1$ ，然后利用独立性可以构造出
$$
\varphi(\omega_x, \omega_y) = \mathrm{E}\left[ e^{-j(\omega_x z_n + \omega_y u_n)} \right] = \prod_{i=1}^n \mathrm{E}\left[ e^{-j(\omega_x x_i + \omega_y y_i)/\sqrt{n}} \right] = \mathrm{E}\left[ e^{-j(\omega_x x_i + \omega_y y_i)/\sqrt{n}} \right]^n
$$
然后利用二元 Fourier 变换的性质 $(-jx)^m(-jy)^k\phi \overset{\mathcal{F}}{\longleftrightarrow} \partial^{k+m}\mathcal{F}(\phi)/\partial \omega_x^m\partial \omega_y^k$ ，于是立刻得到
$$
\mathrm{E}( e^{-j\omega z_n}z_n^k u_n^m ) = \iint_{\mathbb{R}^2} z_n^k u_n^m p(z_n, u_n) e^{-j\omega z_n} \mathrm{d}z_n\mathrm{d}u_n = \left. j^{k+m} \frac{\partial^{k+m}}{\partial\omega_x^m \partial \omega_y^k}\mathrm{E}\left[ e^{-j(\omega_x x_i + \omega_y y_i)/\sqrt{n}} \right]^n \right|_{\omega_x=0, \omega_y=\omega}
$$
和之前一样地，用 Taylor 展开将期望中的这个复指数展开，然后利用 $\mathrm{E}(x_i)=\mathrm{E}(y_i)=0$ ，$\mathrm{Var}(x_i)=\kappa_4-1$ ，$\mathrm{Var}(y_i)=1$ ，以及 $\mathrm{Cov}(x_i,y_i) = \kappa_3$ ，还有重要极限，最终就有
$$
\begin{aligned}
\mathrm{E}\left[ e^{-j(\omega_x x_i + \omega_y y_i)/\sqrt{n}} \right]^n = \exp & \left\{ -\frac{1}{2}\left[\omega_y^2 + (\kappa_4-1)\omega_x^2 + 2\kappa_3\omega_x\omega_y\right] \right. \\[12pt]
& \left. - \frac{j^3}{6\sqrt{n}} \mathrm{E}[(\omega_x x_i + \omega_y y_i)^3] + \frac{j^4}{24n}\mathrm{E}[(\omega_x x_i + \omega_y y_i)^4] + o(n^{-1}) \right\}
\end{aligned}
$$
当然，也和之前一样，再进行一次 Taylor 展开得到
$$
\begin{aligned}
\mathrm{E}\left[ e^{-j(\omega_x x_i + \omega_y y_i)/\sqrt{n}} \right]^n = & e^{-\left[\omega_y^2 + (\kappa_4-1)\omega_x^2 + 2\kappa_3\omega_x\omega_y\right]/2} \left\{ 1 - \frac{j^3}{6\sqrt{n}} \mathrm{E}[(\omega_x x_i + \omega_y y_i)^3] \right.\\[12pt]
& \left.  + \frac{j^4}{24n}\mathrm{E}[(\omega_x x_i + \omega_y y_i)^4] + \frac{j^6}{72n}\mathrm{E}[(\omega_x x_i + \omega_y y_i)^3]^2 + o(n^{-1}) \right\}
\end{aligned}
$$
但由于在 $\varphi_t(\omega)$ 中出现的 $\mathrm{E}( e^{-j\omega z_n}z_n u_n )$ 已经乘上了 $n^{-1/2}$ 这个系数，因此对这一项的计算只需要保留到 $n^{-1/2}$ ；而 $\mathrm{E}( e^{-j\omega z_n}z_n )$ ，  $\mathrm{E}( e^{-j\omega z_n}z_n^2 )$ ， $\mathrm{E}( e^{-j\omega z_n}z_n u_n^2 )$ ， $\mathrm{E}( e^{-j\omega z_n}z_n^2 u_n^2 )$ 四项前则已经乘上了 $n^{-1}$ ，因此对它们的计算只需要保留到 $n^0$ 

那么就来展开计算这几项，有关这些项的计算，参照 <span style="color:#417FBE; font-style:italic">P. Hall, "The Bootstrap and Edgeworth Expansion."</span> 

1. 先来看 $\mathrm{E}( e^{-j\omega z_n}z_n u_n )$ ，考虑到最终取 $\omega_x = 0$ ，因此求导后含有 $\omega_x$ 的项最终都是零（*即使如此也挺麻烦的，交给 sympy 或者 AI 会快得多*）
   $$
   \mathrm{E}( e^{-j\omega z_n}z_n u_n ) = \left[\kappa_3(1-\omega^2) - \frac{\kappa_3^2}{6}\left( 4(j\omega)^3 + (j\omega)^5 \right)n^{-1/2} - \frac{\kappa_4-1}{2}\left( (j\omega)^3 + 2j\omega \right)n^{-1/2} \right]e^{-\omega^2/2} + o(n^{-1/2})
   $$

2. 然后是 $\mathrm{E}( e^{-j\omega z_n}z_n )$ ，它只需要对 $\omega_y$ 求一次偏导
   $$
   \mathrm{E}( e^{-j\omega z_n}z_n ) = -j\omega e^{-\omega^2/2} + o(1)
   $$

3. $\mathrm{E}( e^{-j\omega z_n}z_n^3 )$ ，它需要对 $\omega_y$ 求三次偏导
   $$
   \mathrm{E}( e^{-j\omega z_n}z_n^3 ) = -[(j\omega)^3 + 3j\omega]e^{-\omega^2/2} + o(1)
   $$

4. $\mathrm{E}( e^{-j\omega z_n}z_n u_n^2 )$ ，它需要先对 $\omega_x$ 求两次偏导，然后对 $\omega_y$ 求一次偏导
   $$
   \mathrm{E}( e^{-j\omega z_n}z_n u_n^2 ) = -\left[ \kappa_3^2(j\omega)^3 + (\kappa_4-1+2\kappa_3^2)j\omega \right]e^{-\omega^2/2} + o(1)
   $$

5. $\mathrm{E}( e^{-j\omega z_n}z_n^2 u_n^2 )$ ，它需要先对 $\omega_x$ 求两次偏导，然后对 $\omega_y$ 求两次偏导
   $$
   \mathrm{E}( e^{-j\omega z_n}z_n^2 u_n^2 ) = \left[ (\kappa_4-1)(1 - \omega^2) + \kappa_3^2( \omega^4 - 5\omega^2 + 2) \right]e^{-\omega^2/2} + o(1)
   $$

6. 最后实际上还有 $\mathrm{E}( e^{-j\omega z_n} )$ ，它不用求导，是最简单的一个，但要保留到 $n^{-1}$ 
   $$
   \mathrm{E}( e^{-j\omega z_n} ) = \left[ 1 - \frac{\kappa_3(j\omega)^3}{6}n^{-1/2} + \frac{\kappa_4(j\omega)^4}{24}n^{-1} + \frac{\kappa_3^2(j\omega)^6}{72}n^{-1} \right]e^{-\omega^2/2} + o(n^{-1})
   $$

于是代入，最后得到的特征函数将是（*这一连串暴算过程太麻烦了*）
$$
\begin{aligned}
\varphi_t(\omega) = e^{-\omega^2/2} &\left\{ 1 + \frac{\kappa_3}{6} \left[3j\omega + 2(j\omega)^3\right]n^{-1/2} \right. \\[12pt]
& + \left.\left[ -(1+\kappa_3^2)\omega^2 + \left( \frac{1}{2} - \frac{\kappa_4}{12} + \frac{2\kappa_3^2}{3} \right)\omega^4 - \frac{\kappa_3^2}{18}\omega^6 \right]n^{-1} + o(n^{-1}) \right\}
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
于是假设检验的过程成为

> **有限样本下单边学生氏检验的修正**：
>
> 1. 做出原假设 $H_0: \mu' \leq r_f$ 
> 2. 统计 $\bar{r}, s^2, \kappa_3, \kappa_4$ ，计算 $\tau = \sqrt{n/n_\mathrm{A}} \hat{\mathcal{S}}$ 以及累积分布函数 $F$ 
> 3. 如果 $F(\tau) \geq \alpha\%$ ，那么就可以断言：有 $\alpha \%$ 的把握认为原假设不成立，因而证券的期望收益 $\mu'$ 明显超过无风险收益

最后，来验证两个特殊情况：

- 样本来自于正态分布，即 $\kappa_3=0, \kappa_4=3$ ，此时 $\tau \sim t(n-1)$ ，它的累积分布函数是

  $$
  F(\tau) = \Phi(\tau) - \left[H_1(\tau) + \frac{H_3(\tau)}{4} \right]\phi(\tau)n^{-1} + o(n^{-1}) = \Phi(\tau) - \frac{1}{4}(\tau^3+\tau)\phi(\tau)n^{-1} + o(n^{-1})
  $$

- 样本数量趋于无穷，即 $n \rightarrow \infty$ ，此时 $\tau$ 当然趋向于一个正态分布；不过特别地，这里考虑一下它的方差

   

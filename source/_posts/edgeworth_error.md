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
设它的概率密度函数是 $p(y)$ ，那么特征函数是 $\Phi(\omega) = \mathrm{E}[e^{-j\omega y}] = \mathcal{F}[p(y)]$ ，即概率密度函数的 Fourier 变换。如此一来，利用 $\{y_i\}$ 的独立同分布特性，$z_n = \sqrt{n}\bar{y}$ 的特征函数是
$$
\Phi_z(\omega) = \mathrm{E}\left[ e^{-j\omega \sqrt{n}\bar{y}} \right] = \prod_{i=1}^n \left[ \int_{-\infty}^{+\infty} p(y_i)e^{-j\omega y_i/\sqrt{n}} \mathrm{d}y_i \right] = \left[ \int_{-\infty}^{+\infty} p(y)e^{-j\omega y/\sqrt{n}} \mathrm{d}y \right]^n
$$
对指数函数部分进行 Taylor 展开，得到
$$
\Phi_z(\omega) = \left[ 1 + \sum_{k=1}^\infty \frac{(-j\omega)^k}{k!n^{k/2}} \mathrm{E}(y^k) \right]^n = \exp\left[ -\frac{\omega^2}{2} - \frac{(j\omega)^3}{6\sqrt{n}}\kappa_3 + \frac{(j\omega)^4}{24n}\kappa_4 +o(n^{-1})\right]
$$
第二个等号是重要极限 $e = \lim_{n\rightarrow\infty}(1+1/n)^n$ 的结果。再将 $e^{-\omega^2/2}$ 提出来，并且进行一次 Taylor 展开得到
$$
\Phi_z(\omega) = e^{-\omega^2/2}\left[ 1 - \frac{(j\omega)^3}{6\sqrt{n}}\kappa_3 + \frac{(j\omega)^4}{24n}\kappa_4 + \frac{(j\omega)^6}{72n}\kappa_3^2 + o(n^{-1}) \right]
$$

Gauss 函数 $e^{-\omega^2/2}$ 的 Fourier 变换正是标准正态分布的概率密度函数 $\phi(z)$ ；所以如果上式不保留带有 $n$ 的这些余项，就得到了 Lindberg-Lévy CLT. 

Edgeworth 展开则是继续保留后面的余项，特别地，这里保留到二阶余项。考虑到 Fourier 变换中 $ \mathrm{d}^k\phi/\mathrm{d}z^k \leftrightarrow (j\omega)^k \mathcal{F}(\phi)$ 的对应关系，保留更多余项后的概率分布函数就是
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

# 几何 Sharpe 比率的渐近分布

## 特征函数

接下来考虑有限样本下，Edgeworth 修正对基于几何 Sharpe 比率的假设检验带来的影响。以下始终作原假设 $H_0: \mu'=r_f$ ，于是学生氏统计量是
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
并且来求它的特征函数 $\Phi_t(\omega) = \mathrm{E}(e^{-j\omega\tau})$，就是
$$
\Phi_t(\omega)  = \mathrm{E}\left\{ e^{-j\omega z_n} \left[1 + j\omega\frac{z_nu_n}{2}n^{-1/2} - j\omega\left(\frac{z_n^3}{2} + \frac{3z_nu_n^2}{8} - \frac{z_n}{2} \right)n^{-1} -\omega^2\frac{z_n^2u_n^2}{8}n^{-1} + o(n^{-1})\right] \right\}
$$

## 统计矩的计算

特征函数的计算依赖于许多形如 $\mathrm{E}( e^{-j\omega z_n}z_n^k u_n^m )$ 的项，可以通过联合的特征函数来求这个期望，也就是说令 $x_i = y_i^2 - 1$ ，然后利用独立性可以构造出
$$
\Phi(\omega_x, \omega_y) = \mathrm{E}\left[ e^{-j(\omega_x z_n + \omega_y u_n)} \right] = \prod_{i=1}^n \mathrm{E}\left[ e^{-j(\omega_x x_i + \omega_y y_i)/\sqrt{n}} \right] = \mathrm{E}\left[ e^{-j(\omega_x x_i + \omega_y y_i)/\sqrt{n}} \right]^n
$$
然后利用二元 Fourier 变换的性质 $(-jx)^m(-jy)^k\phi \leftrightarrow \partial^{k+m}\mathcal{F}(\phi)/\partial \omega_x^m\partial \omega_y^k$ ，于是立刻得到
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
但由于在 $\Phi_t(\omega)$ 中出现的 $\mathrm{E}( e^{-j\omega z_n}z_n u_n )$ 已经乘上了 $n^{-1/2}$ 这个系数，因此对这一项的计算只需要保留到 $n^{-1/2}$ ；而 $\mathrm{E}( e^{-j\omega z_n}z_n )$ ，  $\mathrm{E}( e^{-j\omega z_n}z_n^2 )$ ， $\mathrm{E}( e^{-j\omega z_n}z_n u_n^2 )$ ， $\mathrm{E}( e^{-j\omega z_n}z_n^2 u_n^2 )$ 四项前则已经乘上了 $n^{-1}$ ，因此对它们的计算只需要保留到 $n^0$ 

那么就来展开计算

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
   \mathrm{E}( e^{-j\omega z_n}z_n^2 u_n^2 ) = \left[ (\kappa_4-1)(1 - \omega^2) + \kappa_3( \omega^4 - 5\omega^2 + 2) \right]e^{-\omega^2/2} + o(1)
   $$

6. 最后实际上还有 $\mathrm{E}( e^{-j\omega z_n} )$ ，它不用求导，是最简单的一个，但要保留到 $n^{-1}$ 
   $$
   \mathrm{E}( e^{-j\omega z_n} ) = \left[ 1 - \frac{\kappa_3(j\omega)^3}{6}n^{-1/2} + \frac{\kappa_4(j\omega)^4}{24}n^{-1} + \frac{\kappa_3^2(j\omega)^6}{72}n^{-1} \right]e^{-\omega^2/2} + o(n^{-1})
   $$

于是代入，最后得到的特征函数将是（*这一连串计算过程太变态了*）

## 累积分布函数

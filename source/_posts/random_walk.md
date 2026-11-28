---
title: 随机游走与去相关
date: 2026-10-16 14:36:07
categories: 时间序列处理
mathjax: true
description: 随机游走以及对其的差分化
---

# 随机游走

## Wiener 过程

独立的标准白噪声序列的求和被称为**随机游走**，即

$$
w_N = \sum_{i=1}^{N} z_{i} \quad,\quad z_i \sim N(0,\sigma_i) \quad,\quad \mathrm{E}(z_i z_{i'}) = \sigma_i\delta_{ii'}
$$

只需要按照一些常数加权，这就成为许多常见现象的模型。比如说认为 $z_i$ 是速度，那么这就是粒子的 Brown 运动

$$
w(t) = \sum_{i=1}^{N(t)} z_{i} \Delta t_i \quad,\quad t = \sum_{i=1}^{N(t)} \Delta t_i
$$

$w(t)$ 是 $t$ 时刻粒子所处的位置。再比如认为 $z_i$ 是角速度，那么就有了无零漂的陀螺仪输出模型。对于随机游走，它的期望和方差分别是

$$
\mathrm{E}[w(t)] = \sum_{i=1}^{N(t)} \mathrm{E}(z_{i}) \Delta t_i = 0 \quad,\quad \mathrm{Var}[w(t)] = \sum_{i=1}^{N(t)} \mathrm{E}(z_{i}^2) \Delta t_i^2 = \sum_{i=1}^{N(t)} \sigma_i^2\Delta t_i^2
$$

在 $t$ 不变的情况下令 $N(t) \rightarrow \infty$ ，$w(t)$ 成为一个 Riemann 积分。它的期望自然是零，方差则是

$$
\mathrm{Var}[w(t)] = \lim_{N(t) \rightarrow \infty}\sum_{i=1}^{N(t)} \sigma_i^2\Delta t_i^2
$$

如果 $\sigma_i$ 的构造能够使得方差收敛且不为零，那么就称 $x(t)$ 是一个 **Wiener 过程**。这个条件要求对于足够多的 $i$ 有 $\sigma_i^2\Delta t_i^2 \propto 1/N(t) \propto \Delta t_i$，所以 $\sigma_i \propto 1/\sqrt{\Delta t_i}$. 当这个比例系数始终为1时，称 $w(t)$ 是标准的 Wiener 过程，即

$$
w(t) = \int_0^t u(\tau) \mathrm{d}\tau \quad,\quad \mathrm{d}w = z \mathrm{d}t \sim N \left(0, \sqrt{\mathrm{d}t} \right)
$$

这时可以得出标准Wiener过程的方差是

$$
\mathrm{Var}[w(t)] = \lim_{N(t) \rightarrow \infty}\sum_{i=1}^{N(t)} \Delta t_i = t
$$

标准Wiener过程与一个常义的微分之和被称为**广义 Wiener 过程**，即

{% note primary %}
$$
\mathrm{d}x = \mu\mathrm{d}t + \sigma\mathrm{d}w \quad,\quad \mathrm{d}w \sim N \left(0, \sqrt{\mathrm{d}t} \right)
$$
{% endnote %}

$\mu, \sigma$ 都是 $t$ 的函数。广义 Wiener 过程是在无零漂的随机游走中引入了一个缓慢的漂移项

## Itô 引理

对于广义 Wiener 过程的函数$f(x, t)$，对于微小的变动 $\Delta t$ ，差分可以展开写为

$$
\Delta f = \frac{\partial f}{\partial x} \Delta x + \frac{\partial f}{\partial t} \Delta t + \frac{1}{2}\frac{\partial^2 f}{\partial x^2} (\Delta x)^2 + \frac{\partial^2 f}{\partial x\partial t} \Delta x\Delta t + \frac{1}{2}\frac{\partial^2 f}{\partial t^2} (\Delta t)^2 + o \left( (\Delta t)^2 \right)
$$

这里自然 $\Delta x = \mu\Delta t + \sigma\Delta w + o(\Delta t)$，$\Delta w \sim N(0, \sqrt{\Delta t})$，不妨用一个标准正态变量 $u$ 将其写为 $\Delta w = u\sqrt{\Delta t}$ ，如此一来将上式保留到一次项就有

$$
\Delta f = \frac{\partial f}{\partial x} (\mu'\Delta t + \sigma u\sqrt{\Delta t}) + \frac{\partial f}{\partial t} \Delta t + \frac{1}{2}\frac{\partial^2 f}{\partial x^2} \sigma^2u^2\Delta t + o \left( \Delta t \right)
$$

令 $\Delta t \rightarrow 0$ ，得到极限形式的微分，并代回 $\Delta w = u\sqrt{\Delta t}$ 就有

$$
\mathrm{d}f = \left( \mu \frac{\partial f}{\partial x} + \frac{\sigma^2}{2} u^2\frac{\partial^2 f}{\partial x^2} + \frac{\partial f}{\partial t}\right) \mathrm{d}t + \sigma\frac{\partial f}{\partial x} \mathrm{d}w
$$

也即

$$
f(x, t) = \int_0^t \left( \mu \frac{\partial f}{\partial x} + \frac{\sigma^2}{2} u^2\frac{\partial^2 f}{\partial x^2} + \frac{\partial f}{\partial t}\right) \mathrm{d}\tau + \int_0^t \sigma\frac{\partial f}{\partial x} \mathrm{d}w(\tau)
$$

对于含 $u^2$ 的这一项，它也是一个随机变量。由于 $u^2$ 是标准正态变量的平方，因而服从 $\chi^2(1)$ 分布，它的期望是 $1$ ，所以这个积分项的期望就是

$$
\mathrm{E}\left( \int_0^t \frac{\sigma^2}{2} u^2\frac{\partial^2 f}{\partial x^2} \mathrm{d}\tau \right) = \int_0^t \frac{\sigma^2}{2} \frac{\partial^2 f}{\partial x^2} \mathrm{d}\tau
$$

代入不同 $t$ 的 $u(t)$ 之间的独立性，二阶矩则是

$$
\begin{aligned}
\mathrm{E}\left[\left( \int_0^t \frac{\sigma^2}{2} u^2\frac{\partial^2 f}{\partial x^2} \mathrm{d}\tau \right)^2\right] &= \int_0^t\int_0^t \frac{\sigma(\tau)^2\sigma(\tau')^2}{4} \mathrm{E}\left[u(\tau)^2u(\tau')^2\right]\frac{\partial^2 f}{\partial x^2}(\tau) \frac{\partial^2 f}{\partial x^2}(\tau') \mathrm{d}\tau\mathrm{d}\tau' \\[12pt]
&= \int_0^t\int_0^t \frac{\sigma(\tau)^2\sigma(\tau')^2}{4} \mathrm{E}[u(\tau)^2]\mathrm{E}[u(\tau')^2]\frac{\partial^2 f}{\partial x^2}(\tau) \frac{\partial^2 f}{\partial x^2}(\tau') \mathrm{d}\tau\mathrm{d}\tau' \\[12pt]
&= \left( \int_0^t \frac{\sigma^2}{2} \frac{\partial^2 f}{\partial x^2} \mathrm{d}\tau \right)^2
\end{aligned}
$$

固然 $\tau=\tau'$ 时 $\mathrm{E}\left[u(\tau)^2u(\tau')^2\right] = \mathrm{E}\left[u(\tau)^4\right]$ ，但是这一条线在整个面积分中是没有贡献的。所以可以得到积分式的方差是

$$
\mathrm{Var}\left( \int_0^t \frac{\sigma^2}{2} u^2\frac{\partial^2 f}{\partial x^2} \mathrm{d}\tau \right) = \mathrm{E}\left[\left( \int_0^t \frac{\sigma^2}{2} u^2\frac{\partial^2 f}{\partial x^2} \mathrm{d}\tau \right)^2\right] - \mathrm{E}\left( \int_0^t \frac{\sigma^2}{2} u^2\frac{\partial^2 f}{\partial x^2} \mathrm{d}\tau \right)^2 = 0
$$

也就是说在宏观的时间尺度上看来，这一积分项总是取到它的期望，实际上是个定值。将其再代回就得到

$$
f(x, t) = \int_0^t \left( \mu \frac{\partial f}{\partial x} + \frac{\sigma^2}{2} \frac{\partial^2 f}{\partial x^2} + \frac{\partial f}{\partial t}\right) \mathrm{d}\tau + \int_0^t \sigma\frac{\partial f}{\partial x} \mathrm{d}w(\tau))
$$

求回微分就是

{% note primary %}

**Itô 引理**：对于一个广义Wiener过程 $\mathrm{d}x = \mu\mathrm{d}t + \sigma\mathrm{d}w$ ，这里 $\mathrm{d}w \sim N(0, \sqrt{\mathrm{d}t})$ ，函数 $f(x(t),t)$ 的全微分是
$$
\mathrm{d}f = \left( \mu \frac{\partial f}{\partial x} + \frac{\sigma^2}{2} \frac{\partial^2 f}{\partial x^2} + \frac{\partial f}{\partial t}\right) \mathrm{d}t + \sigma\frac{\partial f}{\partial x} \mathrm{d}w
$$
{% endnote %}



## 几何Brown运动

令 $\mathrm{d}x = \mu x\mathrm{d}t + \sigma x \mathrm{d}w$ ，此时的广义 Wiener 过程被称为**几何 Brown 运动**，现在让 $f(x,t) = \ln x$ ，代入 Itô引理显然有

$$
\mathrm{d}f = \left( \mu - \frac{\sigma^2}{2} \right) \mathrm{d}t + \sigma \mathrm{d}w
$$

假定 $\mu, \sigma$ 在所关注的时间段内都是常数，积分得到

$$
\ln x(t) = \left( \mu - \frac{\sigma^2}{2} \right) t + \sigma w(t)
$$

由于 $w(t)$ 是标准 Wiener 过程，所以这意味着

$$
\ln x(t) \sim N\left( \left( \mu - \frac{\sigma^2}{2} \right)t,\ \sigma^2 t \right)
$$

证券的价格 $P(t)$ 是典型的几何 Brown 运动，证券的对数收益率定义为

$$
r(\Delta t) = \ln P(t) - \ln P(t - \Delta t) = \int_{t-\Delta t}^t \left( \mu - \frac{\sigma^2}{2} \right) \mathrm{d}\tau + \int_{t-\Delta t}^t \sigma \mathrm{d}w(\tau)
$$

完全一样地，就有

{% note primary %}

**对数收益率的分布**：如果价格的变动是一个几何 Brown 运动，那么它的对数收益率服从如下正态分布
$$
r(\Delta t) = \left( \mu - \frac{\sigma^2}{2} \right) \Delta t + \sigma \Delta w \sim N\left( \left( \mu - \frac{\sigma^2}{2} \right) \Delta t,\ \sigma^2\Delta t \right)
$$
{% endnote %}

并且不同时间段内的 $r(\Delta t)$ 是相互独立的。在之后的一节，为方便将记 $\mu' = \mu - \sigma^2/2$ 

# 差分化与去相关

对于一个随机游走序列 $w_n$ ，它的自相关系数是
$$
\gamma(l) = \frac{\mathrm{E}(w_n w_{n-l})}{\mathrm{Var}(w_n)} = \frac{1}{\sum_{i=1}^{n} \sigma_i^2\Delta t_i^2}\sum_{i=1}^n\sum_{i'=1}^{n-l} \mathrm{E}(z_i z_{i'})\Delta t_i \Delta t_{i'} = \frac{\sum_{i=1}^{n-l} \mathrm{E}( z_{i}^2) \Delta t_i^2}{\sum_{i=1}^{n} \sigma_i^2\Delta t_i^2} = = \frac{\sum_{i=1}^{n-l} \sigma_i^2 \Delta t_i^2}{\sum_{i=1}^{n} \sigma_i^2\Delta t_i^2}
$$
当 $n \rightarrow \infty$ 时，对于所有 $l$ 都有 $\gamma(l) \rightarrow 1$ ；在实际中能够得到的是自相关系数的相合估计值
$$
\hat{\gamma}(l) = \frac{1}{s^2}\left[ \frac{1}{n-l} \sum_{k=1}^n (w_k - \bar{w}) (w_{k-l} - \bar{w}) \right] \overset{\mathrm{p}}{\longrightarrow} \gamma(l)
$$
通过绘制 $\hat{\gamma}(l)$ ，并观察是否对于所有的 $l$ 其都接近 1 ，这样就可以识别一个序列是否为随机游走。对理想的随机游走进行差分化，即执行 $w_n - w_{n-1}$ ，获得的是白噪声 $z_i \Delta t_i$ ，白噪声序列自相关系数自然是 $\gamma_z(l) = \delta_{l0}$ 

从时间序列处理的角度来看，随机游走可以被视为一个一阶 AR 低通滤波器的极限形式，即 $w_n - a w_{n-1} = z_i\Delta t_i$ 并令 $a \rightarrow 1$ ；这个 AR 滤波器的传递函数是
$$
H(z) = \frac{1}{1-az^{-1}} = \frac{z}{z-a} \quad\Rightarrow\quad H(\omega) = -\frac{e^{j\omega}/a}{1-e^{j\omega}/a}
$$
它有一个零点 $z_0=0$ ，一个极点 $p_0=a$ ，不难获得其半高全宽是 $2\arccos \left[2a/(a^2+1)\right]$ ；当 $a\rightarrow 1$ 时，极点逼近复平面上的单位圆，变成一个不稳定的低通滤波器，半高全宽也成为零，因为其在直流处的幅频响应是无限大的。而就是 $H(z)$ 的逆系统，它是一个 MA 高通滤波器，在直流处具有零的幅频响应，因而能够将随机游走重新稳定化

由 Wiener-Khinchin 定理，自相关系数和归一化的功率谱密度（PSD）构成一组 Fourier 变换。时间强相关的序列（例如随机游走）的自相关系数会延展很长，所以其 PSD 在直流处将具有一个高峰。而一个高通滤波（例如差分化）能够抑制直流处的峰值，抹平 PSD ，这反过来将使得自相关系数尖锐化，降低序列的时间相关性

所以在识别到时间强相关序列后，一个常见的做法就是对其进行差分化或者其它高通滤波处理，然后再考察滤波之后序列的统计特性。这类高通滤波器也可以称为**去相关系统** 。自然，并非所有情形下获得到的“时间强相关序列”都是随机游走，它的自相关系数可能并非像随机游走一样完全平坦，而是有起伏，或者延展长度有限。所以使其去相关的高通滤波也就不是简单的一阶差分

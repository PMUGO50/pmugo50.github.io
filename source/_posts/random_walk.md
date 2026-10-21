---
title: 随机游走与对数收益
date: 2026-10-16 14:36:07
categories: 时间序列处理
mathjax: true
description: 对数价格与几何 Brown 运动
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

>$$
>\mathrm{d}x = \mu\mathrm{d}t + \sigma\mathrm{d}w \quad,\quad \mathrm{d}w \sim N \left(0, \sqrt{\mathrm{d}t} \right)
>$$

$\mu, \sigma$ 都是 $t$ 的函数。广义 Wiener 过程是在无零漂的随机游走中引入了一个缓慢的漂移项

## Itô 引理

对于广义 Wiener 过程的函数$f(x, t)$，对于微小的变动 $\Delta t$ ，差分可以展开写为

$$
\Delta f = \frac{\partial f}{\partial x} \Delta x + \frac{\partial f}{\partial t} \Delta t + \frac{1}{2}\frac{\partial^2 f}{\partial x^2} (\Delta x)^2 + \frac{\partial^2 f}{\partial x\partial t} \Delta x\Delta t + \frac{1}{2}\frac{\partial^2 f}{\partial t^2} (\Delta t)^2 + o \left( (\Delta t)^2 \right)
$$

这里自然 $\Delta x = \mu\Delta t + \sigma\Delta w + o(\Delta t)$，$\Delta w \sim N(0, \sqrt{\Delta t})$，不妨用一个标准正态变量 $u$ 将其写为 $\Delta w = u\sqrt{\Delta t}$ ，如此一来将上式保留到一次项就有

$$
\Delta f = \frac{\partial f}{\partial x} (\mu\Delta t + \sigma u\sqrt{\Delta t}) + \frac{\partial f}{\partial t} \Delta t + \frac{1}{2}\frac{\partial^2 f}{\partial x^2} \sigma^2u^2\Delta t + o \left( \Delta t \right)
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

> **Itô 引理**：对于一个广义Wiener过程 $\mathrm{d}x = \mu\mathrm{d}t + \sigma\mathrm{d}w$ ，这里 $\mathrm{d}w \sim N(0, \sqrt{\mathrm{d}t})$ ，函数 $f(x(t),t)$ 的全微分是
> $$
> \mathrm{d}f = \left( \mu \frac{\partial f}{\partial x} + \frac{\sigma^2}{2} \frac{\partial^2 f}{\partial x^2} + \frac{\partial f}{\partial t}\right) \mathrm{d}t + \sigma\frac{\partial f}{\partial x} \mathrm{d}w
> $$

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

> **对数收益率的分布**：如果价格的变动是一个几何 Brown 运动，那么它的对数收益率服从如下正态分布
> $$
> r(\Delta t) = \left( \mu - \frac{\sigma^2}{2} \right) \Delta t + \sigma \Delta w \sim N\left( \left( \mu - \frac{\sigma^2}{2} \right) \Delta t,\ \sigma^2\Delta t \right)
> $$

并且不同时间段内的 $r(\Delta t)$ 是相互独立的

# 矩的估计

## 样本均值

假定按时间顺序获得了一系列对数收益率样本 $r_i(\Delta t_i)$ ，它们包含的时间范围两两不重合，因而两两独立。记样本总长度是 $n$ ，所有 $\Delta t_i$ 的和记为总时长 $T$ ，那么对数收益率均值的估计值是

$$
\bar{r} = \frac{1}{T} \sum_{i=1}^n r_i(\Delta t_i)
$$

求样本均值的期望

$$
\mathrm{E}(\bar{r}) = \frac{1}{T} \sum_{i=1}^n \mathrm{E}(r_i) = \left( \mu - \frac{\sigma^2}{2} \right)\frac{1}{T} \sum_{i=1}^n \Delta t_i = \mu - \frac{\sigma^2}{2}
$$

以及方差

$$
\mathrm{Var}(\bar{r}) = \mathrm{E} \left\{ \left[ \frac{1}{T} \sum_{i=1}^n r_i - \mathrm{E}(\bar{r}) \right]^2 \right\} = \mathrm{E} \left\{ \left[ \frac{1}{T} \sum_{i=1}^n \left(r_i - \mathrm{E}(r_i) \right) \right]^2 \right\}
$$

由于独立性，求和号展开后有

$$
\mathrm{Var}(\bar{r}) = \frac{1}{T^2} \sum_{i=1}^n \mathrm{E}\left[ r_i - \mathrm{E}(r_i) \right]^2 = \frac{1}{T^2} \sum_{i=1}^n \mathrm{Var}(r_i) = \sigma^2 \frac{1}{T^2} \sum_{i=1}^n \Delta t_i = \frac{\sigma^2}{T}
$$

随着 $T \rightarrow \infty$ ，方差趋向于零。因而由 Chebshev 大数定律，$\bar{r} \rightarrow \mu - \sigma^2/2$ 

## 样本方差

记 $\mu' = \mu - \sigma^2/2$ ，并在以 $\bar{r}$ 作为期望的估计值的情况下，样本方差的估计值是
$$
s^2 = \frac{1}{\Theta} \sum_{i=1}^n [r_i(\Delta t_i) - \bar{r}\Delta t_i]^2 \quad,\quad \Theta = T - \frac{1}{T}\sum_{i=1}^n (\Delta t_i)^2
$$

展开计算 $s^2$ 的期望是

$$
\begin{aligned}
\mathrm{E}(s^2) &= \frac{1}{\Theta} \sum_{i=1}^n \mathrm{E}\left\{ [(r_i - \mathrm{E}(r_i)) - (\bar{r}\Delta t_i - \mathrm{E}(r_i))]^2 \right\} \\[12pt]
&= \frac{1}{\Theta} \sum_{i=1}^n \left\{ \mathrm{E}[(r_i - \mathrm{E}(r_i))^2] + \mathrm{E}[(\bar{r}\Delta t_i - \mathrm{E}(r_i))^2] - 2\mathrm{E}[(r_i - \mathrm{E}(r_i))(\bar{r}\Delta t_i - \mathrm{E}(r_i))] \right\}
\end{aligned}
$$

将三项分开处理：

1. 对于第一项，记 $u_i = [r_i - \mathrm{E}(r_i)]/\sigma\sqrt{\Delta t_i}$ ，它是标准正态变量，其平方服从 $\chi^2(1)$ 分布，期望为 $1$ ，所以 $\mathrm{E}[(r_i - \mathrm{E}(r_i))^2] = \sigma^2\Delta t_i$ 

2. 第二项和第三项中的期望展开后分别是
   $$
   \begin{aligned}
   & \mathrm{E}[(\bar{r}\Delta t_i - \mathrm{E}(r_i))^2] = \mathrm{E}[(\bar{r}\Delta t_i)^2] + \mathrm{E}(r_i)^2 - 2\mathrm{E}(r_i)\mathrm{E}(\bar{r}\Delta t_i) \\[12pt]
   & \mathrm{E}[(r_i - \mathrm{E}(r_i))(\bar{r}\Delta t_i - \mathrm{E}(r_i))] = \mathrm{E}(r_i\bar{r}\Delta t_i) - \mathrm{E}(r_i)\mathrm{E}(\bar{r}\Delta t_i)
   \end{aligned}
   $$
   也就是说后两项之和为
   $$
   \mathrm{E}[(\bar{r}\Delta t_i - \mathrm{E}(r_i))^2] - 2\mathrm{E}[(r_i - \mathrm{E}(r_i))(\bar{r}\Delta t_i - \mathrm{E}(r_i))] = \mathrm{E}[(\bar{r}\Delta t_i)^2] + \mathrm{E}(r_i)^2 -2 \mathrm{E}(r_i\bar{r}\Delta t_i)
   $$


   代入以前得到的结果，并考虑到 $\mathrm{E}(r_i\bar{r}\Delta t_i) = (\Delta t_i/T) \left[ \mathrm{Var}(r_i) + \mathrm{E}(r_i)\sum_{k=1}^n \mathrm{E}(r_k)\right]$ ，上式就化为
$$
\mathrm{E}[(\bar{r} - \mathrm{E}(r_i))^2] - 2\mathrm{E}[(r_i - \mathrm{E}(r_i))(\bar{r} - \mathrm{E}(r_i))] = -\frac{\sigma^2}{T}(\Delta t_i)^2
$$

现在将三项加在一起求和，得到
$$
\mathrm{E}(s^2) = \frac{\sigma^2}{\Theta} \sum_{i=1}^n \left[ \Delta t_i - \frac{(\Delta t_i)^2}{T} \right] = \frac{\sigma^2}{\Theta} \left[ T - \frac{1}{T}\sum_{i=1}^n (\Delta t_i)^2 \right] = \sigma^2
$$
随着 $T \rightarrow \infty$ ，也会有 $s^2 \rightarrow \sigma^2$ （懒得证了，感觉很难算）。如此一来就会有

> **对数收益率的期望和波动估计**：记 $T_\mathrm{Annual}$ 是一年的时长，那么对数收益的年化期望和年化波动可以估计为
> $$
> \mu'_\mathrm{Annual} = \left( \mu - \frac{\sigma^2}{2} \right)T_\mathrm{Annual} \approx  \bar{r}\ T_\mathrm{Annual} \quad,\quad \sigma_\mathrm{Annual} = \sigma\sqrt{T_\mathrm{Annual}} \approx s \sqrt{T_\mathrm{Annual}}
> $$
>

## 高阶样本矩

对于更高阶的归一化中心矩，在已经准确得知期望的情况下，用样本矩进行估计的值是

$$
b_m = \frac{1}{T} \sum_{i=1}^n \left[ \frac{r_i(\Delta t_i) - \bar{r}\Delta t_i}{\sqrt{\Delta t_i}} \right]^m \Delta t_i
$$

特别地，偏度和超额峰度估计为

$$
\kappa_3 = \frac{b_3}{s^3} \quad,\quad \kappa_4 = \frac{b_4}{s^4} - 3
$$

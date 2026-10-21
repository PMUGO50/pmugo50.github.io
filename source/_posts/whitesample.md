---
title: 对噪声的采样
date: 2025-09-20 09:02:20
categories: 时间序列处理
mathjax: true
description: 对噪声进行采样产生的混叠现象
---

# 零阶保持

对于许多 ADC 而言，从一个连续的信号中进行采样的过程是
$$
y(t) = \sum_k x(kT_s) h_0(t-kT_s)
$$
这里 $x$ 是被采样的信号，$y$ 是采样后并进行保持的信号，$h_0$ 是一阶保持器的响应核，即
$$
h_0(t) = u(t) - u(t-T_s)
$$
这里 $u(t)$ 是单位阶跃函数。可以很快给出 $h_0$ 的 Fourier 变换
$$
\mathcal{F}[h_0(t)] = \int_0^{T_s} e^{-j2\pi ft}\mathrm{d}t = \frac{\sin(\pi f/f_s)}{\pi f} e^{-j\pi f/f_s}
$$
这里 $f_s=1/T_s$ 是采样频率，同时可以给出
$$
\mathcal{F}[h_0(t-kT_s)] = \int_{kT_s}^{(k+1)T_s} e^{-j2\pi ft}\mathrm{d}t = \frac{\sin(\pi f/f_s)}{\pi f} e^{-j\pi f/f_s}e^{-j2k\pi f/f_s} = \mathcal{F}[h_0(t)]e^{-j2k\pi f/f_s}
$$

# 对功率信号的采样

对于功率信号 $x(t)$ ，可以将其展开写为
$$
x(t) = \lim_{T \rightarrow \infty}\sqrt{T}\int_{-\infty}^{+\infty} X(f) e^{j2\pi ft} \mathrm{d}f
$$
这里 $T$ 可以认为是功率信号持续的时间，$X(f)$ 是随机变量。那么对采样后的信号求取功率谱密度（PSD）将有
$$
S_Y(f) = \lim_{T \rightarrow\infty} \frac{1}{T} \mathrm{E}\left[\left| \int_{-T/2}^{T/2} y(t) e^{-j2\pi ft}\mathrm{d}t \right|^2\right]
$$
代入 $y(t)$ 的表达式，将有
$$
S_Y(f) = \lim_{T \rightarrow\infty} \frac{1}{T} \mathrm{E}\left[\left| \mathcal{F}[h_0(t)]\sum_k x(kT_s)e^{-j2k\pi f/f_s}  \right|^2\right]
$$

___

这里插播一个关系式，注意可以通过反用 Dirac 函数的 Fourier 变换式得到
$$
\sum_k x(kT_s)e^{-j2k\pi f/f_s} = \sum_k x(kT_s) \int_{-\infty}^{+\infty} \delta(t-k/f_s)e^{-j2\pi ft}\mathrm{d}t
$$
考虑到 $f_s=1/T_s$ ，并交换积分和求和的顺序，此即
$$
\begin{aligned}
\sum_k x(kT_s)e^{-j2k\pi f/f_s} &= \int_{-\infty}^{+\infty} \left[\sum_k x(kT_s) \delta(t-kT_s)\right]e^{-j2\pi ft}\mathrm{d}t \\[12pt]
&= \int_{-\infty}^{+\infty} \left[x(t)\sum_k \delta(t-kT_s)\right]e^{-j2\pi ft}\mathrm{d}t \\[12pt]
\end{aligned}
$$
也就是对括号内信号的 Fourier 变换，利用 Fourier 变换的乘积-卷积关系，上式可以继续写为
$$
\sum_k x(kT_s)e^{-j2k\pi f/f_s} = \sqrt{T}X(\nu) \ast \sum_k e^{-j2k\pi f/f_s}
$$
卷积右侧的元素是 Dirichlet 核，其结果是
$$
\sum_k e^{-j2k\pi f/f_s} = \lim_{N\rightarrow\infty} \frac{\sin[(2N+1)\pi f/f_s]}{\sin(\pi f/f_s)} = f_s\sum_{k}\delta(f-kf_s)
$$
第二个等号的成立可以这样不严谨地理解：在 $f\rightarrow kf_s$ 时，分子分母均趋向于零，其极限为 $2N+1$ ，随着 $N$ 增大趋向于无穷大；反之在其它点处则趋向于零。系数 $f_s$ 是由于 Dirac 函数积分为 $1$ 的性质带来的。总之将有
$$
\sum_k x(kT_s)e^{-j2k\pi f/f_s} = f_s\sqrt{T}\sum_{k}X(f-kf_s)
$$

____

将这个结果代入，将有
$$
S_Y(f) = \lim_{T \rightarrow\infty} \frac{T}{T} \mathrm{E}\left[\left| \mathrm{sinc}(f/f_s)e^{-j\pi f/f_s}\sum_{k}X(f-kf_s) \right|^2\right]
$$
这里 sinc 函数是
$$
\mathrm{sinc}(f/f_s) = \frac{\sin(\pi f/f_s)}{\pi f/f_s}
$$
展开复数的幅值平方，将有
$$
S_Y(f) = \mathrm{sinc}^2(f/f_s)\mathrm{E}\left[\sum_{k}X(f-kf_s)\sum_{k'}X^\ast(f-k'f_s)\right]
$$
对于各频率独立贡献的噪声，其不同频率分量之间没有相关性，即当 $k\neq k'$ 时系综的数学期望为零，所以上式成为
$$
\begin{aligned}
S_Y(f) &= \mathrm{sinc}^2(f/f_s)\sum_{k}\mathrm{E}\left[X(f-kf_s)X^\ast(f-kf_s)\right] \\[12pt]
&= \mathrm{sinc}^2(f/f_s)\sum_{k}S_X(f-kf_s)
\end{aligned}
$$

## 混叠

如果 $S_X$ 在 $\pm f_s/2$ 之外有明显的值，那么在上式相加的过程中将会出现
$$
S_Y(f) = \mathrm{sinc}^2(f/f_s)\left[ \cdots + S_X(f-f_s) + S_X(f-f_s) + S_X(f+f_s) + \cdots \right]
$$
 这样会将 $\pm f_s, \pm 2f_s$ 等等分量叠进 $y(t)$ 的 PSD ，从而显著增加采样后信号的噪声 PSD 高度

因此一个降噪的做法是在采样前加一个带宽等于或低于 $f_s/2$ 的低通滤波器，滤除 $\pm f_s/2$ 以外的噪声贡献，从而防止混叠带来的噪声 PSD 抬升

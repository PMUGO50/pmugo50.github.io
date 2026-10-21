---
title: 因果系统的实部自满性
date: 2024-02-01 12:12:29
categories: 光和电磁的描述
mathjax: true
description: 因果系统实部自满性与复解析信号
---

这是信号系统考试的一道题，当时考场上有点感觉，但是没写出来，遂乱写一通交卷。考题是这样的

> $a(t)$ 是一个因果系统的单位脉冲响应且为实的，$A(f)$ 是它的 Fourier 变换，记 $R(f)$ 是 $A(f)$ 的实部，$X(f)$ 是 $A(f)$ 的虚部，求证：
> $$
> R(f) = \frac{1}{\pi}\int_{-\infty}^{+\infty} \frac{X(\eta)}{f-\eta} \mathrm{d}\eta \quad , \quad X(f) = -\frac{1}{\pi}\int_{-\infty}^{+\infty} \frac{R(\eta)}{f-\eta} \mathrm{d}\eta
> $$

## 实部自满性

因果系统（前提是单位脉冲响应在原点没有奇异性）有所谓的实部自满性，就是其 Fourier 变换的实部能包含该信号全部信息

关于这一点，课本内容表明对于任何实信号都有
$$
\mathrm{Ev}[a(t)] \overset{\mathcal{F}}{\longleftrightarrow} \mathrm{Re}[A(f)]
$$
而因果信号 $a(t)$ 的偶部是没有信号重叠的，所以只要将 $\mathrm{Re}[A(f)]$ 反变换回时域，然后把 $t<0$ 的部分剪掉，再乘以 2 就能得到原信号 $a(t)$ ，也就是 $\mathrm{Re}[A(f)]$ 已经包含了原信号 $a(t)$ 的全部信息

也可以从另一个角度来看，先将 $a(t)$ 写为 $a(t)=a(t)u(t)$ ，这是因为 $a(t)$ 是因果的（这里 $u(t)$ 是单位阶跃函数），所以 $t<0$ 的部分恒为零，然后由于
$$
\mathcal{F}[u(t)] = \frac{1}{j2\pi f} + \frac{\delta(f)}{2}
$$
以及 Fourier 变换的相乘性质 $a_1(t)a_2(t) \overset{\mathcal{F}}{\longleftrightarrow} A_1(f) * A_2(f)$​ ，所以
$$
A(f) = A(f) * \left[ \frac{1}{j2\pi f} + \frac{\delta(f)}{2} \right] = \frac{1}{j2\pi}\int_{-\infty}^{+\infty} \frac{A(\eta)}{f-\eta} \mathrm{d}\eta + \frac{1}{2}A(f)
$$
将 $A(f)/2$ 移到左边就是
$$
A(f) = \frac{1}{j\pi}\int_{-\infty}^{+\infty} \frac{A(\eta)}{f-\eta} \mathrm{d}\eta
$$
现在将 $A(f)$ 写为 $A(f)=R(f)+jX(f)$ ，上式成为
$$
R(f) + jX(f) = \frac{1}{\pi}\int_{-\infty}^{+\infty} \frac{X(\eta)}{f-\eta} \mathrm{d}\eta - \frac{j}{\pi}\int_{-\infty}^{+\infty} \frac{R(\eta)}{f-\eta} \mathrm{d}\eta
$$
令实部虚部分别相等就得到考题要求证明的结果。这表明因果系统 Fourier 变换的实部和虚部是通过一个卷积关系互相关联的，也就是给定其实部也能求出虚部，同样说明了其具有实部自满性

## Hilbert 变换

上面过程中出现的式子
$$
R(f) = \frac{1}{\pi}\int_{-\infty}^{+\infty} \frac{X(\eta)}{f-\eta} \mathrm{d}\eta = X(f) * \frac{1}{\pi f}
$$
称为 $X(f)$​​​ 的 Hilbert 变换。这个变换作用在时域上时将会有

$$
\mathcal{H}[a(t)] = a(t) * \frac{1}{\pi t} \overset{\mathcal{F}}{\longleftrightarrow} A(f) \mathcal{F} \left[ \frac{1}{\pi t} \right]
$$
利用 Fourier 变换的对偶性，即已知 $u(t)$ 的 Fourier 变换是 $1/j2\pi f + \delta(f)/2$ ，那么应该有
$$
\mathcal{F} \left[ \frac{1}{j2\pi t} + \frac{\delta(t)}{2} \right] = u(-f)
$$
而又因为 $\mathcal{F}[\delta(t)]=1$ ，所以
$$
\mathcal{F} \left[ \frac{1}{\pi t} \right] = j2\cdot \mathcal{F} \left[ \frac{1}{j2\pi t} \right] = j2\cdot \left[ u(-f) - \frac{1}{2} \right] = j[2 u(-f) - 1]
$$
这实际是一个符号函数
$$
\mathcal{F} \left[ \frac{1}{\pi t} \right] = -j\ \mathrm{sgn}(f) \quad,\quad \mathrm{sgn}(f) = \begin{cases} 1 &,f>0 \\ 0 &,f=0 \\ -1 &,f<0 \end{cases}
$$
然后再以 $a(t)$ 为实部，以它的 Hilbert 变换为虚部构造一个新信号，将会有
$$
a(t) + j \mathcal{H}[a(t)] \overset{\mathcal{F}}{\longleftrightarrow} A(f) [1+\mathrm{sgn}(f)] = 2A(f)u(f)
$$
这将 $a(t)$ 的频谱进行了翻折并相加，称 $a(t) + j \mathcal{H}[a(t)]$ 为原信号 $a(t)$ 的复解析信号。在一些领域中，复解析信号有着重要的地位。一个典型的例子是在经典波动光学中，只要不遇到非线性系统，那么总是可以用复解析形式来表示电磁波，这将令计算过程以及物理图景更为清晰。

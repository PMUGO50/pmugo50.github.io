---
title: 自发辐射光源
date: 2025-02-03 13:18:01
categories: 光和电磁的描述
mathjax: true
description: 给出自发辐射光源下的光谱
---

# 光强

对于真空中的单色平面电波 $\boldsymbol{e}_r(t)=\boldsymbol{E}\cos(2\pi ft - 2\pi \boldsymbol{m} \cdot \boldsymbol{x} + \theta)$ ，其 Poynting 矢量为
$$
\boldsymbol{s}_r = \frac{c}{4\pi} \left(\boldsymbol{e}_r \times \boldsymbol{h}_r\right)
$$
这里磁场经由 Faraday 电磁感应定律和电场联系起来，即
$$
\nabla \times \boldsymbol{e}_r = -\frac{1}{c}\frac{\partial \boldsymbol{h}_r}{\partial t} \quad\Longrightarrow\quad 2\pi\boldsymbol{m} \times \boldsymbol{E}\sin(2\pi ft - 2\pi \boldsymbol{m} \cdot \boldsymbol{x} + \theta) = \frac{2\pi f}{c}\boldsymbol{H}\sin(2\pi ft - 2\pi \boldsymbol{m} \cdot \boldsymbol{x} + \theta)
$$
从而 $\boldsymbol{H} = (c\boldsymbol{m}/f) \times \boldsymbol{E}$ ，加之电磁场的方向以及 Poynting 矢量互相垂直，所以
$$
|\boldsymbol{s}_r| = \frac{c}{4\pi}|\boldsymbol{e}_r||\boldsymbol{h}_r| = \frac{c}{4\pi}|\boldsymbol{e}_r|^2
$$
那么对于单偏振光 $e_r(t)=E\cos(2\pi ft + \theta)$，在 $t$ 时刻通过特定位置单位面积的光功率就是
$$
I_s(t) = \frac{c}{4\pi}e_r(t)^2 = \frac{c}{8\pi}E^2[1 + \cos(4\pi ft + 2\theta)]
$$
由于光的变化频率一般很高，所以实际的光电探测中检测到的往往是上式的均值，这个时间平均的窗口 $T \gg 1/f$ ，且时间平均的结果就被称为光强
$$
I = \Braket{I_s(t)} = \lim_{T\rightarrow\infty}\frac{1}{T} \int_{-T/2}^{T/2} I_s(t) \mathrm{d}t = \frac{c}{8\pi}E^2
$$
这里 $\Braket{\cdot}$ 是指时间平均。当然，光强也可以定义为正比于磁场的平方的量，但由于 Wiener 驻波实验表明在光与物质互动时，磁场分量几乎没有任何贡献，因而往往用电场定义光强。

由于光学中存在着大量的线性运算，所以可以将余弦形式的实电波转化为解析的复电波，这实际上只是令 $e(t) = Ee^{j2\pi ft}$ ，这里 $E$ 是一个复数，包含了幅值以及初相信息。此时若有任意线性算子 $L$ 作用在实电波上，总可以通过先将其作用在复电波上然后再取实部得到结果，即
$$
L[e_r(t)] = \mathrm{Re}\{ L[e(t)] \}
$$
而光强则成为一般常用的定义式
$$
I = \frac{c}{8\pi}\Braket{\frac{1}{2}[e(t) + e^{\ast}(t)]^2} = \frac{c}{8\pi}\Braket{e(t)e^{\ast}(t)} = \frac{c}{8\pi}\Braket{|e(t)|^2}
$$
# 自发辐射的光谱

## 谱分解

关于平稳随机信号的谱分解，详细的描述参照 *L. Landau, and E. Lifshitz, "Statistical physics, part 1," 3rd ed., Sec. 122.* 以及 *A. Papoulis, and S. U. Pillai, "Probability, random variables and stochastic processes," 4th ed., Chap. 11, Sec. 4.* 

在自发辐射中，$e(t)$ 是由大量原子辐射出的无关波列叠加而成的，因而往往是一个随机过程，一般还认为它是**遍历（ergodic）**的，即其任意阶矩的时间平均和数学期望是相等的，这即是说在时间上总能够“遍历”完所有可能的状态

为了获得频率域上的信息，可以对 $e(t)$ 进行 Fourier 变换，其结果自然也是平稳的随机过程，即
$$
e(t) =\int_{0}^{\infty} E(f) e^{j2\pi ft} \mathrm{d}f \quad,\quad E(f) = \int_{-\infty}^{+\infty} e(t) e^{-j2\pi ft} \mathrm{d}t
$$

那么光强就可以写为
$$
I = \frac{c}{8\pi}\mathrm{E}\left[ |e(t)|^2 \right] = \int_0^\infty \int_0^\infty \frac{c}{8\pi}\mathrm{E}\left[E(f)E^\ast(\nu) \right] e^{j2\pi (f-\nu)t} \mathrm{d}f\mathrm{d}\nu
$$
继续将内部的数学期望展开写为
$$
\begin{aligned}
\frac{c}{8\pi}\mathrm{E}\left[E(f)E^\ast(\nu) \right] &= \int_{-\infty}^{+\infty} \int_{-\infty}^{+\infty} \frac{c}{8\pi}\mathrm{E}\left[e(t)e^\ast(t')\right] e^{-j2\pi (ft-\nu t')} \mathrm{d}t\mathrm{d}t' \\[12pt]
& = \int_{-\infty}^{+\infty} \int_{-\infty}^{+\infty} \frac{c}{8\pi}\mathrm{E}\left[e(t)e^\ast(t-\tau)\right] e^{-j2\pi f\tau}e^{-j2\pi (f-\nu)t'} \mathrm{d}t'\mathrm{d}\tau \\[12pt]
&= \int_{-\infty}^{+\infty} \frac{c}{8\pi}\mathrm{E}\left[e(t)e^\ast(t-\tau)\right] e^{-j2\pi f\tau}\mathrm{d}\tau \cdot \delta(f-\nu)
\end{aligned}
$$
上式中的 $\tau = t-t'$ ，Dirac 函数 $\delta(f-\nu)$ 是由对 $t'$ 的积分给出的。在最后等号右侧的积分显然是一个 Fourier 变换，被变换的函数是电场的**自相关函数（ACF）**，记为 $\varGamma(\tau)$ ；变换的结果则记为 $S(f)$ ，即
$$
\varGamma(\tau) = \frac{c}{8\pi} \mathrm{E}\left[e(t)e^\ast(t-\tau)\right] \quad,\quad S(f) = \int_{-\infty}^{+\infty} \varGamma(\tau) e^{-j2\pi f\tau}\mathrm{d}\tau
$$
这给出平稳随机过程频谱的零相关性，即

{% note primary %}

**Wiener-Khinchin 定理**：对于广义平稳的随机过程 $e(t)$ ，记其 Fourier 变换为 $E(f)$ ，其自相关函数 $\varGamma(\tau)$ 的 Fourier 变换为 $S(f)$ ，那么
$$
\frac{c}{8\pi}\mathrm{E}\left[E(f)E^\ast(\nu) \right] = S(f) \delta(f-\nu)
$$
{% endnote %}

将上述结果代回 $I$ 的表达式，或者注意 $I=\varGamma(0)$ ，总之就得到
$$
I = \int_0^\infty S(f) \mathrm{d}f
$$
也即光强是由函数 $S(f)$ 在频域上线性叠加的结果，因此 $S(f)$ 被称为**光谱** ，不难看出光谱是实函数，因而 ACF 作为它的 Fourier 逆变换一定是一个偶函数

## 时间相干性

由于 ACF 与光谱构成 Fourier 变换对，这就意味着它们之间存在类似于量子力学中不确定性关系的约束：光谱越宽，ACF 就越尖锐；反之光谱越窄，ACF 则越平坦

关于不确定性关系的详细推导，参照 *M. Born, and E. Wolf, "Principle of optics," 7th ed., Chap. 10, Sec. 8.* 

按照方差的形式，可以将光谱和 ACF 的宽度分别定义为
$$
f_\mathrm{b}^2 = \frac{ \int_{-\infty}^{+\infty} (f-f_\mathrm{c})^2 S(f)^2 \mathrm{d}f }{ \int_{-\infty}^{+\infty} S(f)^2 \mathrm{d}f } \quad,\quad \tau_\mathrm{co}^2 = \frac{ \int_{-\infty}^{+\infty} \tau^2 |\varGamma(\tau)|^2 \mathrm{d}\tau }{ \int_{-\infty}^{+\infty} |\varGamma(\tau)|^2 \mathrm{d}\tau }
$$
当然，尽管这里对光谱的积分范围遍及了负频率，但在复解析形式下负频率的光谱始终为零；$f_c$ 是由期望的形式定义的中心频率
$$
f_\mathrm{c} = \frac{ \int_{-\infty}^{+\infty} f S(f)^2 \mathrm{d}f }{ \int_{-\infty}^{+\infty} S(f)^2 \mathrm{d}f }
$$
而对于 ACF ，因为其为偶函数，其中心位置便一定为零

由 **Parseval-Plancherel 定理**， $\tau_\mathrm{co}^2$ 和 $f_\mathrm{b}^2$ 定义式中的分母是相等的。至于分子，首先进行变量代换 $f \rightarrow f + f_\mathrm{c}$ ，然后将其中一个 $S(f+ f_\mathrm{c})$ 写为 $\varGamma(\tau)$ 的 Fourier 变换，所以
$$
f_\mathrm{b}^2 = \frac{ \int_{-\infty}^{+\infty} \varGamma(\tau)\int_{-\infty}^{+\infty} f^2 S(f+ f_\mathrm{c}) e^{-j2\pi (f+f_c)\tau} \mathrm{d}f \mathrm{d}\tau }{ \int_{-\infty}^{+\infty} |\varGamma(\tau)|^2 \mathrm{d}\tau  } = -\frac{1}{4\pi^2}\frac{ \int_{-\infty}^{+\infty} \varGamma(\tau)\cdot \mathrm{d}^2\varGamma^\ast/\mathrm{d}\tau^2\cdot \mathrm{d}\tau }{ \int_{-\infty}^{+\infty} |\varGamma(\tau)|^2 \mathrm{d}\tau  }
$$
第二个等号应用了 Fourier 变换的微分-倍乘性质以及共轭-反演性质，分子上的积分可以用分部积分来处理。一般认为自相关函数以及至少到它的二阶导数，在无穷远处都是趋近于零的，这是因为 $f^2S(f)$ 在全频率域上的积分一般一定是收敛的，否则意味着光将具有足够多的无限高频分量。因此上式继续写为
$$
\tau_\mathrm{co}^2 = \frac{1}{4\pi^2}\frac{ \int_{-\infty}^{+\infty} \left|\mathrm{d}\varGamma^\ast/\mathrm{d}\tau\right|^2 \mathrm{d}f }{ \int_{-\infty}^{+\infty} |\varGamma(\tau)|^2 \mathrm{d}\tau  } \geq \frac{1}{16\pi^2} \frac{ \int_{-\infty}^{+\infty} |\varGamma(\tau)|^2 \mathrm{d}\tau }{ \int_{-\infty}^{+\infty} \tau^2 |\varGamma(\tau)|^2 \mathrm{d}\tau }
$$
这个不等号是**量子力学中的对易不等式**，在量子力学相关书目中很容易找到，或者参照 *M. Born, and E. Wolf, "Principle of optics," 7th ed., App. I.* 总之上式立即给出
$$
f_\mathrm{b}^2 \tau_\mathrm{co}^2 \geq \frac{1}{16\pi^2}
$$
不等式的等号只有在光谱具有 Gauss 形状时才成立，因此一般上式更多地近似写为 $f_\mathrm{b} \tau_\mathrm{co} \approx 1$ ，进行量级上的估计；当采用波长而非频率衡量谱宽时，只要 $f_\mathrm{b} \ll f_\mathrm{c}$ ，那么就有 $\tau_\mathrm{co} \approx \lambda_\mathrm{c}^2/c\Delta \lambda$ ，这里$\lambda_\mathrm{c} = c/f_\mathrm{c}$ ，而 $\Delta \lambda$ 是以波长衡量的谱宽

___

值得一提的是，如果上述过程不进行变量代换 $f \rightarrow f + f_\mathrm{c}$ ，将无法得到不确定性关系式。这是因为这种情况下，对易不等式的放缩过度了

___

# 强度的谱分解

由于实际的光电探测往往无法捕捉以极高频率变化的电场，而是将光强转化为正比于其的电流或者电压输出，因此光电探测器也被称为一个**平方律器件** ，那么对光强进行谱分解时，经由与对电场分解完全相同的形式，得到
$$
\mathrm{E}\left[\mathcal{I}(f)\mathcal{I}^\ast(\nu) \right] = S_\mathrm{I}(f) \delta(f-\nu)
$$
这里 $\mathcal{I}(f) = \mathcal{F}[I(t)] = (c/8\pi)\mathcal{F}[e(t)e^\ast(t)]$ ，因而该式实际上应当展开写为
$$
\begin{aligned}
S_\mathrm{I}(f) \delta(f-\nu) &= \mathrm{E}\left\{ \left[\frac{c}{8\pi}\int_{-\infty}^{+\infty} \iint_0^\infty E(\alpha)E^\ast(\beta) e^{j2\pi(\alpha - \beta - f)t} \mathrm{d}\alpha\mathrm{d}\beta\mathrm{d}t \right] \right. \\[12pt] 
& \left. \quad\ \cdot \left[\frac{c}{8\pi}\int_{-\infty}^{+\infty} \iint_0^\infty E(\alpha')E^\ast(\beta') e^{j2\pi(\alpha' - \beta' - \nu)t'} \mathrm{d}\alpha'\mathrm{d}\beta'\mathrm{d}t' \right]^\ast\right\} \\[12pt]
&= \frac{c^2}{64\pi^2} \iint_0^\infty \mathrm{E}\left[ E(\alpha)E^\ast(\alpha - f)E^\ast(\alpha')E(\alpha' - \nu) \right]\mathrm{d}\alpha\mathrm{d}\alpha'
\end{aligned}
$$
由于在自发辐射中，对于任意一个频率，发光原子的数目都是巨大的，因此按照**中心极限定理（CLT）**，$E$ 总是一个复值的 Gauss 型随机变量，而针对此存在 Isserlis 定理，详细的叙述与证明参照 B. Alexander, Found. Comput. Math., vol. 7, pp. 229-244. 这里要用到的是该定理的四阶矩推论，它表明对于 复值 Gauss 型随机变量存在 $\mathrm{E}[X_1Y_1X_2^\ast Y_2^\ast] = \mathrm{E}[X_1X_2^\ast]\mathrm{E}[Y_1 Y_2^\ast] + \mathrm{E}[X_1Y_2^\ast]\mathrm{E}[Y_1X_2^\ast]$ ，那么上式就成为
$$
\begin{aligned}
S_\mathrm{I}(f) \delta(f-\nu) &= \iint_0^\infty \frac{c}{8\pi} \mathrm{E}\left[ E(\alpha)E^\ast(\alpha')\right] \cdot \frac{c}{8\pi}\mathrm{E}\left[ E(\alpha' - \nu)E^\ast(\alpha - f) \right] \mathrm{d}\alpha\mathrm{d}\alpha' \\[12pt]
&\quad\ + \iint_0^\infty \frac{c}{8\pi} \mathrm{E}\left[ E(\alpha)E^\ast(\alpha - f)\right] \cdot \frac{c}{8\pi}\mathrm{E}\left[ E(\alpha' - \nu)E^\ast(\alpha') \right] \mathrm{d}\alpha\mathrm{d}\alpha' \\[12pt]
&= \iint_0^\infty S(\alpha)S(\alpha-f) \delta(\alpha - \alpha')\delta(f - \nu - \alpha + \alpha') \mathrm{d}\alpha\mathrm{d}\alpha' \\[12pt]
&\quad\ +\iint_0^\infty S(\alpha)S(\alpha') \delta(f)\delta(\nu) \mathrm{d}\alpha\mathrm{d}\alpha'
\end{aligned}
$$
对 Dirac 函数积分后，就得到
$$
S_\mathrm{I}(f) \delta(f-\nu) = \delta(f-\nu) \int_0^\infty S(\alpha)S(\alpha-f) \mathrm{d}\alpha + I^2\delta(f)\delta(\nu)
$$
事实上，从积分的效果上看，$\delta(f)\delta(\nu) = \delta(f)\delta(f-\nu)$ ，所以就得到

{% note primary %}

**强度的谱分解** ：对于光谱为 $S(f)$ 的一束光 $e(t)$ ，其光强 $I(t) = |e(t)|^2$ 的功率谱密度服从
$$
S_\mathrm{I}(f) = \int_0^\infty S(\alpha)S(\alpha-f) \mathrm{d}\alpha + I^2\delta(f)
$$

这里 $I$ 是直流光强

{% endnote %}

___

对一个二元函数 $H(f, \nu)$ 与 $\delta(f)\delta(f-\nu)$ 积分，结果是
$$
\int_{-\infty}^{+\infty}\int_{-\infty}^{+\infty} H(f,\nu) \delta(f)\delta(f-\nu) \mathrm{d}f\mathrm{d}\nu = \int_{-\infty}^{+\infty} H(f,f) \delta(f)\mathrm{d}f = H(0,0)
$$
对 $H(f,\nu)\delta(f)\delta(\nu)$ 积分的结果是一样的。这是因为遍及整个平面积分，只有在两个 Dirac 函数绘出的轨迹的交点处才有非零的值

___
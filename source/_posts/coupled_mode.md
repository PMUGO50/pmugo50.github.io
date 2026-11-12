---
title: 振幅缓变近似的耦合方程
date: 2026-03-25 13:57:29
categories: 光和电磁的描述
mathjax: true
description: 对Maxwell方程组应用WKB近似得到耦合模方程
---

# 正交本征模

以下全程采用 Gauss 单位制，因而真空介电常数和真空磁导率都是 1 ，所有过程都可以参照 <span style="color:#417FBE; font-style:italic">M. Gorodetsky and V. Ilchenko, "Optical microsphere resonators: optimal coupling to high-Q whispering-gallery modes," J. Opt. Soc. Am. B, vol. 16, pp. 147(1999).</span> 

忽略非线性效应和折射率的时变，即只考虑**互易定理得到满足**的情况。将 Maxwell 方程组的第三式（Faraday 电磁感应定律）和第四式（Ampere-Maxwell 环路定律）互相代入，并利用无源条件下的第一式（Gauss 定律），就得到波方程
$$
\nabla \times (\nabla \times \boldsymbol{E}) + \frac{\varepsilon}{c^2}\frac{\partial^2 \boldsymbol{E}}{\partial t^2} = 0
$$
在理想情况下，通过谐振腔的边界约束可以解出一系列正交的本征模式 $\boldsymbol{E}_i(\boldsymbol{r},t) = b_i e^{-j\omega_i t} \boldsymbol{e}_i(\boldsymbol{r})$ ，并且由于是驻波，所以所有的 $\boldsymbol{e}_i$ 都可以是实的。至于所谓正交是指方向向量满足
$$
\int_V \boldsymbol{e}_k(\boldsymbol{r}) \cdot \varepsilon_b\boldsymbol{e}_i(\boldsymbol{r}) \mathrm{d}V = \delta_{ik}
$$

这里 $\varepsilon_b(\boldsymbol{r}) = 1 + \chi_b(\boldsymbol{r})$ 是只有腔时的介电常数分布，$\chi_b$ 是腔的极化率分布

> 这种以积分作为内积定义的正交性具有很大的灵活性。比如说即使偏振方向相同，但由于角量子数（纵向量子数）不同，不同的纵模之间也可以是正交的

当然，总的 $\boldsymbol{E}_b(\boldsymbol{r},t) = \sum_i \boldsymbol{E}_i(\boldsymbol{r},t)$ ，由于模式的正交性，那么代入到波方程中就得到
$$
\nabla \times (\nabla \times \boldsymbol{e}_i) - \frac{\varepsilon_b\omega_i^2}{c^2}\boldsymbol{e}_i = 0
$$

同样地，在全空间中只有热池光而没有谐振腔的情况下，热池光可以写为
$$
\boldsymbol{E}_a(\boldsymbol{r},t) = \int_0^\infty a(\omega) e^{-j(\omega t-\beta x)} \boldsymbol{e}_a(\boldsymbol{r}) \mathrm{d}\omega
$$
这里设定了光传播方向为 $x$ ，行波的意义由 $e^{j\beta x}$ 表出，所以 $\boldsymbol{e}_a$ 也是实的。另外它也要满足波方程，也就是说 $\boldsymbol{e}_a$ 同样也满足
$$
\nabla \times (\nabla \times \boldsymbol{e}_a) - \frac{\varepsilon_a\omega^2}{c^2}\boldsymbol{e}_a = 0
$$
 $\varepsilon_a(\boldsymbol{r}) = 1 + \chi_a(\boldsymbol{r})$ 是只有热池时的介电常数分布，$\chi_a$ 是热池的极化率分布。另外同样地，设置其满足归一化条件
$$
\int_S \boldsymbol{e}_a(\boldsymbol{r}) \cdot \varepsilon_a\boldsymbol{e}_a(\boldsymbol{r}) \mathrm{d}S = 1
$$

# 耦合扰动

现在将热池与谐振腔在空间上靠近形成耦合，此时全空间的介电常数分布是 $\varepsilon(\boldsymbol{r}) = 1 + \chi_b(\boldsymbol{r}) + \chi_a(\boldsymbol{r})$ ，这里 $\chi_b,\chi_a$ 各自分别只在腔体内和热池内具有非零值。总的电场仍然始终服从波方程
$$
\nabla \times [\nabla \times (\boldsymbol{E}_b + \boldsymbol{E}_a)] + \frac{1 + \chi_b + \chi_a}{c^2}\frac{\partial^2}{\partial t^2}(\boldsymbol{E}_b + \boldsymbol{E}_a) = 0
$$
在经典场论中，同一个对象无法同时作为场源和响应方，比如如果考虑电荷辐射出电磁场，就不能同时考虑它受电磁场的影响。所以现在做两个假设：

1. 当热池的光耦入腔内时，热池的光作为场源，不考虑腔内光对其的影响，所以
   $$
   \nabla \times (\nabla \times \boldsymbol{E}_a) - \frac{1+\chi_a}{c^2} \frac{\partial^2}{\partial t^2}\boldsymbol{E}_a = 0
   $$

2. 当腔内的光耦回热池时，腔内的光成为场源，不考虑热池中光对其的影响，所以
   $$
   \nabla \times (\nabla \times \boldsymbol{E}_b) - \frac{1+\chi_b}{c^2} \frac{\partial^2}{\partial t^2}\boldsymbol{E}_b = 0
   $$

## 耦入过程

考虑光的耦入时，以腔内光作为观察对象，上述方程给出
$$
\nabla \times (\nabla \times \boldsymbol{E}_b) + \frac{1 + \chi_b + \chi_a}{c^2}\frac{\partial^2 \boldsymbol{E}_b}{\partial t^2} = -\frac{\chi_b}{c^2}\frac{\partial^2 \boldsymbol{E}_a}{\partial t^2}
\tag{1}
$$
在扰动下，谐振模式和热池光分别写为
$$
\boldsymbol{E}_k(\boldsymbol{r},t) = b_k(t) \boldsymbol{e}_k(\boldsymbol{r}) \quad,\quad \boldsymbol{E}_a(\boldsymbol{r},t) = \int_0^\infty a(\omega, t)e^{j\beta x} \boldsymbol{e}_a(\boldsymbol{r}) \mathrm{d}\omega
$$
将这一结果代入，并利用 $\nabla \times (\nabla \times \boldsymbol{e}_k) - \omega_k^2(1+\chi_b)\boldsymbol{e}_k/c^2 = 0$ 得到
$$
\sum_k \left[(1+\chi_b)\omega_k^2b_k(t) + (1 + \chi_b + \chi_a)\frac{\partial^2 b_k}{\partial t^2}\right]\boldsymbol{e}_k(\boldsymbol{r}) = -\chi_b \int_0^\infty\frac{\partial^2 a}{\partial t^2} e^{j\beta x}\boldsymbol{e}_a(\boldsymbol{r}) \mathrm{d}\omega
\tag{2}
$$

{% note danger %}

**振幅缓变近似**：记一个收到微扰的简谐振动为 $b_k(t) = B_k(t)e^{-j\omega_k t}$ ，那么其各阶导数的振幅相对于相位都是缓变的，特别地对于前两阶导数有
$$
\left| \frac{\partial B_k}{\partial t} \right| \ll \left| \omega_k B_k \right| \quad,\quad \left| \frac{\partial^2 B_k}{\partial t^2} \right| \ll \left| \omega_k \frac{\partial B_k}{\partial t} \right|
$$
{% endnote %}

在振幅缓变近似下将二阶时间偏导展开得到
$$
\frac{\partial^2 b_k}{\partial t^2} = \left(\frac{\partial^2 B_k}{\partial t^2} - 2j\omega_k \frac{\partial B_k}{\partial t} - \omega_k^2B_k \right)e^{-j\omega_k t} \approx -\left( 2j\omega_k \frac{\partial B_k}{\partial t} + \omega_k^2B_k \right)e^{-j\omega_k t}
$$
然后利用 $(\partial B_k/\partial t) e^{-j\omega_k t} = \partial b_k/\partial t + j\omega_k b_k$ 得到
$$
\frac{\partial^2 b_k}{\partial t^2} \approx -2j\omega_k \frac{\partial b_k}{\partial t} + \omega_k^2 b_k(t)
$$
将这个结果代回式 (2) 之后，并对方程两侧同时对 $\boldsymbol{e}_i$ 作内积，就可以得到
$$
\begin{aligned}
& \sum_k \left[2j\omega_k\frac{\partial b_k}{\partial t}\int_V \boldsymbol{e}_i \cdot (1 + \chi_b + \chi_a)\boldsymbol{e}_k \mathrm{d}V - \omega_k^2 b_k\int_V \boldsymbol{e}_i \cdot [2(1+\chi_b)+\chi_a]\boldsymbol{e}_k \mathrm{d}V\right] \\[12pt]
=& -\int_0^\infty\frac{\partial^2 a}{\partial t^2} \left(\int_V \boldsymbol{e}_i \cdot \chi_be^{j\beta x}\boldsymbol{e}_a \mathrm{d}V\right) \mathrm{d}\omega
\end{aligned}
$$

利用正交性定义 $\int_V \boldsymbol{e}_i \cdot (1+\chi_b)\boldsymbol{e}_k \mathrm{d}V = \delta_{ik}$ 得到
$$
\sum_k \left[2j\omega_k\frac{\partial b_k}{\partial t} \left( \delta_{ik} + \int_V \boldsymbol{e}_i \cdot \chi_a\boldsymbol{e}_k \mathrm{d}V \right) - \omega_k^2 b_k\left( 2\delta_{ik} + \int_V \boldsymbol{e}_i \cdot \chi_a\boldsymbol{e}_k \mathrm{d}V \right) \right] = -\int_0^\infty\frac{\partial^2 a}{\partial t^2} \left(\int_V \boldsymbol{e}_i \cdot \chi_b e^{j\beta x}\boldsymbol{e}_a \mathrm{d}V\right) \mathrm{d}\omega
$$

将 $i = k$ 的项单独提出来就有
$$
\frac{\partial b_i}{\partial t} + j\omega_ib_i + \sum_{k \neq i} \left( \int_V \boldsymbol{e}_i \cdot \chi_a\boldsymbol{e}_k \mathrm{d}V \right) \left(\frac{\omega_k}{\omega_i}\frac{\partial b_k}{\partial t} - \frac{\omega_k^2}{j2\omega_i}b_k\right) = -\frac{1}{j2\omega_i}\int_0^\infty\frac{\partial^2 a}{\partial t^2} \left(\int_V \boldsymbol{e}_i \cdot \chi_b e^{j\beta x}\boldsymbol{e}_a \mathrm{d}V\right) \mathrm{d}\omega
$$
最后，再利用一次振幅缓变近似 $\partial b_k/\partial t = -j\omega_k b_k + (\partial B_k/\partial t) e^{-j\omega_k t} \approx -j\omega_k b_k$ ，就可以得到最终的形式
$$
\frac{\partial b_i}{\partial t} + j\omega_ib_i - j\sum_{k \neq i} \left( \int_V \boldsymbol{e}_i \cdot \chi_a\boldsymbol{e}_k \mathrm{d}V \right) \frac{\omega_k^2}{2\omega_i}b_k = -\frac{1}{j2\omega_i}\int_0^\infty\frac{\partial^2 a}{\partial t^2} \left(\int_V \boldsymbol{e}_i \cdot \chi_b e^{j\beta x}\boldsymbol{e}_a \mathrm{d}V\right) \mathrm{d}\omega
\tag{Cavity}
$$

## 耦回过程

考虑光的耦回时，以热池光作为观察对象，方程 (1) 给出
$$
\nabla \times (\nabla \times \boldsymbol{E}_a) + \frac{1 + \chi_b + \chi_a}{c^2}\frac{\partial^2 \boldsymbol{E}_a}{\partial t^2} = -\frac{\chi_a}{c^2}\frac{\partial^2 \boldsymbol{E}_b}{\partial t^2}
$$
同样地，选定 $\omega$ 之后，进行和上面完全一样的操作，与 $e^{j\beta x}\boldsymbol{e}_a$ 做内积就得到
$$
2j\omega\frac{\partial a}{\partial t}\left( 1 + \int_V \boldsymbol{e}_a \cdot \chi_b \boldsymbol{e}_a \mathrm{d}V \right) - \omega^2 a \left( 2 + \int_V \boldsymbol{e}_a \cdot \chi_b \boldsymbol{e}_a \mathrm{d}V \right)  = -\int_V e^{-j\beta x}\boldsymbol{e}_a \cdot \chi_a\frac{\partial^2 \boldsymbol{E}_b}{\partial t^2} \mathrm{d}V
\tag{3}
$$
这是一个一阶微分方程，所以将 $\boldsymbol{E}_b$ 展开，并利用常数变易法或者通解公式即可得到
$$
a(\omega, t) = e^{-j\omega' t}\left[ a_\mathrm{in}(\omega) - \frac{1}{j2\omega}\sum_{k}\left(\int_V e^{-j\beta x}\boldsymbol{e}_a \cdot \chi_a\boldsymbol{e}_k \mathrm{d}V\right) \int_0^t \frac{\partial^2 b_k}{\partial \tau^2}e^{j\omega' \tau}  \mathrm{d}\tau \right]
$$
这里 $\omega'$ 是 $\omega$ 的函数，并很接近 $\omega$ 本身
$$
\omega' = \frac{2 + \int_V \boldsymbol{e}_a \cdot \chi_b \boldsymbol{e}_a \mathrm{d}V}{2 + 2\int_V \boldsymbol{e}_a \cdot \chi_b \boldsymbol{e}_a \mathrm{d}V}\omega \approx \omega
$$
代入之前利用振幅缓变近似得到的两个结果，上式成为
$$
a(\omega, t) = e^{-j\omega' t}\left[ a_\mathrm{in}(\omega) + \frac{1}{j2\omega}\sum_{k} \left(\int_V e^{-j\beta x}\boldsymbol{e}_a \cdot \chi_a\boldsymbol{e}_k \mathrm{d}V\right) \int_0^t \omega_k^2 b_ke^{j\omega' \tau}  \mathrm{d}\tau \right]
\tag{Bath}
$$
___

将式 (Bath) 代回 (Cavity) ，重点就落在了对 $\partial a^2/\partial t^2$ 的计算上。这只需要按部就班即可，事实上一阶导直接由常微分方程本身给出
$$
\frac{\partial a}{\partial t} = -j\omega'a(\omega, t) + \frac{1}{2j\omega} \sum_k \frac{\int_V e^{-j\beta x}\boldsymbol{e}_a \cdot \chi_a \boldsymbol{e}_k \mathrm{d}V}{1 + \int_V \boldsymbol{e}_a \cdot \chi_b \boldsymbol{e}_a \mathrm{d}V} \omega_k^2 b_k
$$
所以二阶导就是对上式求一次导数，然后将 $\partial a/\partial t$ 再代入即可
$$
\frac{\partial^2 a}{\partial t^2} = -\omega'^2 a - \frac{\omega_k^2(\omega_k+\omega')}{2\omega} \sum_k \frac{\int_V e^{-j\beta x}\boldsymbol{e}_a \cdot \chi_a \boldsymbol{e}_k \mathrm{d}V}{1 + \int_V \boldsymbol{e}_a \cdot \chi_b \boldsymbol{e}_a \mathrm{d}V} b_k
$$
___

将上式代回 (Cavity) 得到一个庞大的**耦合方程**
$$
\begin{aligned}
& \frac{\partial b_i}{\partial t}
+ j\omega_i\left[1 + \frac{\omega_i+\omega'}{4\omega}\frac{\left(\int_V \boldsymbol{e}_i \cdot \chi_b e^{j\beta x}\boldsymbol{e}_a \mathrm{d}V \right) \left(\int_V e^{-j\beta x}\boldsymbol{e}_a \cdot \chi_a \boldsymbol{e}_i \mathrm{d}V \right)}{1 + \int_V \boldsymbol{e}_a \cdot \chi_b \boldsymbol{e}_a \mathrm{d}V}\right] b_i \\[12pt]
+& \int_0^\infty \frac{\omega'^2}{4\omega_i\omega} \left(\int_V \boldsymbol{e}_i \cdot \chi_b e^{j\beta x}\boldsymbol{e}_a \mathrm{d}V \right) \left(\int_V e^{-j\beta x}\boldsymbol{e}_a \cdot \chi_a \boldsymbol{e}_i \mathrm{d}V \right) \int_0^t  \omega_i^2 b_i e^{-j\omega'(t-\tau)} \mathrm{d}\tau \mathrm{d}\omega \\[12pt]
-& j\sum_{k \neq i} \frac{\omega_k^2}{2\omega_i}\left( \int_V \boldsymbol{e}_i \cdot \chi_a\boldsymbol{e}_k \mathrm{d}V \right) \left[ 1 - \frac{\omega_k+\omega'}{2\omega}\frac{\left(\int_V \boldsymbol{e}_i \cdot \chi_b e^{j\beta x}\boldsymbol{e}_a \mathrm{d}V \right) \left(\int_V e^{-j\beta x}\boldsymbol{e}_a \cdot \chi_a \boldsymbol{e}_k \mathrm{d}V \right)}{1 + \int_V \boldsymbol{e}_a \cdot \chi_b \boldsymbol{e}_a \mathrm{d}V} \right] b_k \\[12pt]
+& \sum_{k \neq i} \int_0^\infty \frac{\omega'^2}{4\omega_i\omega} \left(\int_V \boldsymbol{e}_i \cdot \chi_b e^{j\beta x}\boldsymbol{e}_a \mathrm{d}V \right) \left(\int_V e^{-j\beta x}\boldsymbol{e}_a \cdot \chi_a \boldsymbol{e}_k \mathrm{d}V \right) \int_0^t \omega_k^2 b_k e^{-j\omega'(t-\tau)} \mathrm{d}\tau \mathrm{d}\omega \\[12pt]
=& \int_0^\infty \frac{\omega'^2}{j2\omega_i} \left(\int_V \boldsymbol{e}_i \cdot \chi_b e^{j\beta x}\boldsymbol{e}_a \mathrm{d}V\right) a_\mathrm{in}(\omega) e^{-j\omega' t}\mathrm{d}\omega
\end{aligned}
$$

# 标准形式化

## Markov 近似

首先，耦合方程的右侧的积分
$$
\int_V \boldsymbol{e}_i \cdot \chi_b e^{j\beta x}\boldsymbol{e}_a \mathrm{d}V
\tag{4}
$$
对于模式 $i$ 来讲，$\boldsymbol{e}_i$ 中一定有一个角向因子 $\cos(m_i\varphi)$ ，这里 $m_i$ 是角量子数，$\varphi$ 是经度角。所以如果 $\beta$ 与 $m_iR_i$ 相距甚远（$R_i$ 是模式距盘心的平均半径），那么这一项积分就成为对一个快变的正弦函数求均值，结果是零

由于 $\beta, m_i$ 本身是 $\omega, \omega_i$ 的函数，因此这就要求：只有在 $\omega' \approx \omega_i$ 的时候才有明显的耦合，因此方程右侧可以加以简化为
$$
-j\frac{\omega_i}{2} \left(\int_V \boldsymbol{e}_i \cdot \chi_b e^{j\beta_i x}\boldsymbol{e}_a \mathrm{d}V\right) \int_0^\infty a_\mathrm{in}(\omega) e^{-j\omega' t}\mathrm{d}\omega = -j\frac{\omega_i}{2} \left(\int_V \boldsymbol{e}_i \cdot \chi_b e^{j\beta_i x}\boldsymbol{e}_a \mathrm{d}V\right) a_\mathrm{in}(t)
$$
这里 $\beta_i = m_i R_i$ ，不妨将这个系数就记为 $\zeta_i$ ，也就是
$$
\zeta_i = \frac{\omega'^2}{j2\omega_i} \left(\int_V \boldsymbol{e}_i \cdot \chi_b e^{j\beta_i x}\boldsymbol{e}_a \mathrm{d}V\right) \approx -j\frac{\omega_i}{2} \left(\int_V \boldsymbol{e}_i \cdot \chi_b e^{j\beta_i x}\boldsymbol{e}_a \mathrm{d}V\right)
$$

## 互易近似

另外，耦合方程的左侧出现了多次“两个积分相乘”，这两个积分可以重新写为
$$
\begin{aligned}
& \int_V \boldsymbol{e}_i \cdot \chi_b e^{j\beta x}\boldsymbol{e}_a \mathrm{d}V = \int_{-\infty}^{+\infty} \left(\int_{S(x)} \boldsymbol{e}_i \cdot \chi_b \boldsymbol{e}_a \mathrm{d}S\right)e^{j\beta x} \mathrm{d}x \\[12pt]
& \int_V e^{-j\beta x}\boldsymbol{e}_a \cdot \chi_a \boldsymbol{e}_k \mathrm{d}V = \int_{-\infty}^{+\infty} \left(\int_{S(x)} \boldsymbol{e}_a \cdot \chi_a \boldsymbol{e}_k \mathrm{d}S\right)e^{-j\beta x} \mathrm{d}x
\end{aligned}
$$
在一定的条件下它们几乎是共轭的。为此要用到“广义互易定理”，以下这部分见 <span style="color:#417FBE; font-style:italic">S. Chuang, "A coupled mode formulation by reciprocity and a variational principle," J. Lightw. Technol., vol. 5, pp. 5(1987).</span> 

考虑两个特殊情况：在空间中只有腔时，腔内只存在一个模式 $\boldsymbol{E}_b = b_ke^{-j\omega_k t}\boldsymbol{e}_k$ ；在空间中只有热池时，热池中只有单色光 $\boldsymbol{E}_a = a(\omega_k)e^{-j(\omega_k t-\beta_k x)} \boldsymbol{e}_a$ ；现在写出互易关系式，就有
$$
\begin{aligned}
& \nabla \cdot \left( \boldsymbol{E}_a \times \boldsymbol{H}_b - \boldsymbol{E}_b \times \boldsymbol{H}_a \right) \\[12pt]
=& \left(\nabla \times \boldsymbol{E}_a\right) \cdot \boldsymbol{H}_b - \left(\nabla \times \boldsymbol{H}_b\right) \cdot \boldsymbol{E}_a - \left(\nabla \times \boldsymbol{E}_b\right) \cdot \boldsymbol{H}_a + \left(\nabla \times \boldsymbol{H}_a\right) \cdot \boldsymbol{E}_b
\end{aligned}
$$
应用 Maxwell 方程组的第三式和第四式，并考虑到在目前两束光都是同频、单色的情况下，对时间的偏导成为 $-j\omega_k$ 的数乘。另外，从一开始就假定了折射率是非时变的，所以不参与对时间求导，上式就成为

{% note primary %}

**广义互易定理**：记两束光（以 $a,b$ 标记）的频率都是 $\omega_k$ ，它们分别感应到 $\chi_a, \chi_b$ 的极化率，那么它们始终服从
$$
\nabla \cdot \left( \boldsymbol{E}_a \times \boldsymbol{H}_b - \boldsymbol{E}_b \times \boldsymbol{H}_a \right) = -j\omega_k(\chi_a-\chi_b)\boldsymbol{E}_a \cdot \boldsymbol{E}_b
$$
{% endnote %}

对这个定理两侧取 $yz$ 平面上的积分，就有
$$
\frac{\partial}{\partial x} \int_{S(x)} \left( \boldsymbol{E}_a \times \boldsymbol{H}_b - \boldsymbol{E}_b \times \boldsymbol{H}_a \right) \cdot \hat{\boldsymbol{x}}\mathrm{d}S = -j\omega_k a(\omega_k)b_ke^{j\beta_k x} \left( \int_{S(x)} \boldsymbol{e}_a \cdot \chi_a \boldsymbol{e}_k \mathrm{d}S - \int_{S(x)} \boldsymbol{e}_k \cdot \chi_b \boldsymbol{e}_a \mathrm{d}S \right)
$$
如此一来要是左式为零，也就是说左式中的积分沿着热池光的传播方向守恒，那么右式中的两个积分就将相等。至于左式中这个积分为何能（至少在所关心的耦合区域内）守恒，个人感觉可以用光功率守恒的角度解释

{% note danger %}

**互易近似**：记腔和热池单独存在时的电场分布分别是 $\boldsymbol{E}_b = b_ke^{-j\omega_k t}\boldsymbol{e}_k$ ，$\boldsymbol{E}_a = a(\omega_k)e^{-j(\omega_k t-\beta_k x)} \boldsymbol{e}_a$ ，它们分别感应到 $\chi_a, \chi_b$ 的极化率，假设沿着 $x$ 始终有守恒关系
$$
\frac{\partial}{\partial x} \int_{S(x)} \left( \boldsymbol{E}_a \times \boldsymbol{H}_b - \boldsymbol{E}_b \times \boldsymbol{H}_a \right) \cdot \hat{\boldsymbol{x}}\mathrm{d}S = 0
$$
那么就存在
$$
\int_{S(x)} \boldsymbol{e}_a \cdot \chi_a \boldsymbol{e}_k \mathrm{d}S = \int_{S(x)} \boldsymbol{e}_k \cdot \chi_b \boldsymbol{e}_a \mathrm{d}S
$$
{% endnote %}

现在来看方程左边的一项
$$
\int_0^\infty \frac{\omega'^2}{4\omega_i\omega} \left(\int_V \boldsymbol{e}_i \cdot \chi_b e^{j\beta x}\boldsymbol{e}_a \mathrm{d}V \right) \left(\int_V e^{-j\beta x}\boldsymbol{e}_a \cdot \chi_a \boldsymbol{e}_k \mathrm{d}V \right) \int_0^t \omega_k^2 b_k e^{-j\omega'(t-\tau)} \mathrm{d}\tau \mathrm{d}\omega
\tag{5}
$$
和上一节一样的理由，只有当 $\beta_k$ 很接近 $\beta_i$ ，也即 $\omega_k$ 很接近 $\omega_i$ 时，这一项才有明显的非零值。此时代入上一节要求的 $\omega' \approx \omega \approx \omega_i$ ，并应用互易近似，该式成为
$$
\frac{\omega_i\omega_k}{4} \left(\int_V \boldsymbol{e}_i \cdot \chi_b e^{j\beta_i x}\boldsymbol{e}_a \mathrm{d}V \right) \left(\int_V \boldsymbol{e}_k \cdot \chi_b e^{j\beta_k x} \boldsymbol{e}_a \mathrm{d}V \right)^\ast \int_0^\infty \left(\int_0^t b_k e^{-j\omega'(t-\tau)} \mathrm{d}\tau\right) \mathrm{d}\omega
$$
那么不妨记
$$
\kappa_{ik} = \frac{\omega_i\omega_k}{4} \left(\int_V \boldsymbol{e}_i \cdot \chi_b e^{j\beta_i x}\boldsymbol{e}_a \mathrm{d}V \right) \left(\int_V \boldsymbol{e}_k \cdot \chi_b e^{j\beta_k x} \boldsymbol{e}_a \mathrm{d}V \right)^\ast = \zeta_i\zeta_k^\ast
$$
如此一来方程左侧的这一项最终成为 $\kappa_{ik} b_k(t)/2$ ，这里 1/2 是因为对 $\tau$ 从 $0$ 积分到 $t$ 是一个半边 Fourier 变换，因此产生了一个额外的折半系数

特别地，对于方程左侧 $i=k$ 的这一项，上面的关系自然甚至不需要近似就成立，此时 $\kappa_{ii} = |\zeta_i|^2$ ，对应的项也就是 $\kappa_{ii}b_i/2$ 

然后来看方程左侧的一个中括号里面的项
$$
\frac{\omega_k+\omega'}{2\omega}\frac{\left(\int_V \boldsymbol{e}_i \cdot \chi_b e^{j\beta x}\boldsymbol{e}_a \mathrm{d}V \right) \left(\int_V e^{-j\beta x}\boldsymbol{e}_a \cdot \chi_a \boldsymbol{e}_k \mathrm{d}V \right)}{1 + \int_V \boldsymbol{e}_a \cdot \chi_b \boldsymbol{e}_a \mathrm{d}V} \approx \frac{4\kappa_{ik}/\omega_i\omega_k}{1 + \int_V \boldsymbol{e}_a \cdot \chi_b \boldsymbol{e}_a \mathrm{d}V}
\tag{6}
$$
它的效果是使得本征谐振频率发生偏移。不过这个偏移量是极其之小的，因为 $\kappa_{ii}$ 与模式 $i$ 的半宽（FWHM）在同一量级（这一点在完全形式化后对耦合方程进行 Fourier 变换得到谐振谱线就可以看出），而光学谐振腔的品质因子 $Q_i = \omega_i/\mathrm{FWHM}_i$ 一般很难低于 $10^5$ ，所以上式几乎为零

## 形式化方程

经过以上近似，还剩下唯一没有处理的积分就是
$$
\frac{\omega_k^2}{2\omega_i}\int_V \boldsymbol{e}_i \cdot \chi_a\boldsymbol{e}_k \mathrm{d}V
\tag{7}
$$
将其记为 $g_{ik}$ ，那么就可以将耦合方程彻底形式化地写为
$$
\frac{\partial b_i}{\partial t} = -\left( j\omega_i + \frac{\kappa_{ii}}{2}\right) b_i - \sum_{k\neq i} \left( jg_{ik} + \frac{\kappa_{ik}}{2} \right) b_k + \zeta_i a_\mathrm{in}
$$
这是**无损的第一个耦合模方程**

> 在这里需要强调的是：从上面的推导过程可以看出，只有模式 $k$ 与模式 $i$ 的谐振频率相距甚近时， $g_{ik}, \kappa_{ik}$ 才会有明显的非零值

另外，现在回到式 (Bath) ，它是式 (3) 的一个通解。不过通过改变积分号的上下限，还有另一种形式的通解
$$
a(\omega, t) = e^{-j\omega' t}\left[ a_\mathrm{thr}(\omega) - \frac{1}{j2\omega}\sum_{k} \left(\int_V e^{-j\beta x}\boldsymbol{e}_a \cdot \chi_a\boldsymbol{e}_k \mathrm{d}V\right) \int_t^\infty \omega_k^2 b_ke^{j\omega' \tau}  \mathrm{d}\tau \right]
$$
改变积分上下限的操作相当于反演光的注入方向，式 (Bath) 代表了光从热池注入腔，而该式代表了光从腔注入回热池。沿用上面所有的形式记号，上式将可以给出
$$
\frac{\partial b_i}{\partial t} = -\left( j\omega_i - \frac{\kappa_{ii}}{2}\right) b_i - \sum_{k\neq i} \left( jg_{ik} - \frac{\kappa_{ik}}{2} \right) b_k - \zeta_i a_\mathrm{thr}
$$
将它与第一个耦合模方程相减，并考虑到一般 $\kappa_{ik} \ll \kappa_{ii}$ ，且 $\kappa_{ii} = |\zeta_i|^2$ ，就得到**第二个耦合模方程** 
$$
a_\mathrm{thr}(t) = a_\mathrm{in}(t) - \zeta_i^\ast b_i(t)
\tag{CMT2}
$$
最后，在经典理论中，Maxwell 方程默认电磁波的传输是无损耗的。在量子理论中，可以通过非 Hermitian 算子引入损耗，不过在这里，只能在形式上添加一个本征损耗，对于模式 $i$ ，记它为 $\gamma_i$ ，那么得到完整的**第一个耦合模方程** 
$$
\frac{\partial b_i}{\partial t} = -\left( j\omega_i + \frac{\gamma_i + \kappa_{ii}}{2}\right) b_i - \sum_{k\neq i} \left( jg_{ik} + \frac{\kappa_{ik}}{2} \right) b_k + \zeta_i a_\mathrm{thr}
\tag{CMT1}
$$
不难看出来，由于改变光的注入方向并不影响 $\gamma_i$ 的符号，因此这一项并不影响第二个耦合模方程的成立

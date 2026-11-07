---
title: 振幅缓变近似的耦合方程
date: 2026-03-25 13:57:29
categories: 光和电磁的描述
mathjax: true
description: 对Maxwell方程组应用WKB近似得到耦合模方程
---

# 正交本征模

以下全程采用 Gauss 单位制，因而真空介电常数和真空磁导率都是 1 

忽略非线性效应和折射率的时变，即只考虑**互易定理得到满足**的情况。将 Maxwell 方程组的第三式（Faraday 电磁感应定律）和第四式（Ampere-Maxwell 环路定律）互相代入，并利用无源条件下的第一式（Gauss 定律），就得到波方程
$$
\nabla \times (\nabla \times \boldsymbol{E}) + \frac{\varepsilon}{c^2}\frac{\partial^2 \boldsymbol{E}}{\partial t^2} = 0
$$
在理想情况下，通过谐振腔的边界约束可以解出一系列正交的本征模式 $\boldsymbol{E}_i(\boldsymbol{r},t) = b_i e^{-j\omega_i t} \boldsymbol{e}_i(\boldsymbol{r})$ ，所谓正交是指方向向量满足
$$
\int_V \boldsymbol{e}^\ast_k(\boldsymbol{r}) \cdot \varepsilon_b\boldsymbol{e}_i(\boldsymbol{r}) \mathrm{d}V = \delta_{ik}
$$

这里 $\varepsilon_b(\boldsymbol{r}) = 1 + \chi_b(\boldsymbol{r})$ 是只有腔时的介电常数分布，$\chi_b$ 是腔的极化率分布

> 这种以积分作为内积定义的正交性具有很大的灵活性。比如说即使偏振方向相同，但由于角量子数（纵向量子数）不同，不同的纵模之间也可以是正交的

当然，总的 $\boldsymbol{E}_b(\boldsymbol{r},t) = \sum_i \boldsymbol{E}_i(\boldsymbol{r},t)$ ，由于模式的正交性，那么代入到波方程中就得到
$$
\nabla \times (\nabla \times \boldsymbol{e}_i) - \frac{\varepsilon_b\omega_i^2}{c^2}\boldsymbol{e}_i = 0
$$

同样地，在全空间中只有热池光而没有谐振腔的情况下，热池光可以写为
$$
\boldsymbol{E}_a(\boldsymbol{r},t) = a(\omega) e^{-j\omega t} \boldsymbol{e}_a(\omega,\boldsymbol{r})
$$
自然它也要满足波方程，也就是说 $\boldsymbol{e}_a$ 同样也满足
$$
\nabla \times (\nabla \times \boldsymbol{e}_a) - \frac{\varepsilon_a\omega^2}{c^2}\boldsymbol{e}_a = 0
$$
 $\varepsilon_a(\boldsymbol{r}) = 1 + \chi_a(\boldsymbol{r})$ 是只有热池时的介电常数分布，$\chi_a$ 是热池的极化率分布。另外同样地，设置其满足归一化条件
$$
\int_V \boldsymbol{e}^\ast_a(\boldsymbol{r}) \cdot \varepsilon_a\boldsymbol{e}_a(\boldsymbol{r}) \mathrm{d}V = 1
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
将扰动下的谐振模式写为 $\boldsymbol{E}_k(\boldsymbol{r},t) = b_k(t) \boldsymbol{e}_k(\boldsymbol{r})$ ，将这一结果代入，并利用 $\nabla \times (\nabla \times \boldsymbol{e}_k) - \omega_k^2(1+\chi_b)\boldsymbol{e}_k/c^2 = 0$ 得到
$$
\sum_k \left[(1+\chi_b)\omega_k^2b_k(t) + (1 + \chi_b + \chi_a)\frac{\partial^2 b_k}{\partial t^2}\right]\boldsymbol{e}_k(\boldsymbol{r}) = -\chi_b \boldsymbol{E}_a(\omega,\boldsymbol{r})
\tag{2}
$$

{% note danger %}

**振幅缓变近似**：记一个收到微扰的模式为 $b_k(t) = B_k(t)e^{-j\omega_k t}$ ，那么其各阶导数的振幅相对于相位都是缓变的，特别地对于前两阶导数有
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
\sum_k \left[2j\omega_k\frac{\partial b_k}{\partial t}\int_V \boldsymbol{e}^\ast_i \cdot (1 + \chi_b + \chi_a)\boldsymbol{e}_k \mathrm{d}V - \omega_k^2 b_k\int_V \boldsymbol{e}^\ast_i \cdot [2(1+\chi_b)+\chi_a]\boldsymbol{e}_k \mathrm{d}V\right] = \int_V \boldsymbol{e}^\ast_i \cdot \chi_b\boldsymbol{E}_a \mathrm{d}V
$$

利用正交性定义 $\int_V \boldsymbol{e}^\ast_i \cdot (1+\chi_b)\boldsymbol{e}_k \mathrm{d}V = \delta_{ik}$ 得到
$$
\sum_k \left[2j\omega_k\frac{\partial b_k}{\partial t} \left( \delta_{ik} + \int_V \boldsymbol{e}^\ast_i \cdot \chi_a\boldsymbol{e}_k \mathrm{d}V \right) - \omega_k^2 b_k\left( 2\delta_{ik} + \int_V \boldsymbol{e}^\ast_i \cdot \chi_a\boldsymbol{e}_k \mathrm{d}V \right) \right] = \int_V \boldsymbol{e}^\ast_i \cdot \chi_b\boldsymbol{E}_a \mathrm{d}V
$$

将 $i = k$ 的项单独提出来就有
$$
\frac{\partial b_i}{\partial t} + j\omega_ib_i + \sum_{k \neq i} \left( \int_V \boldsymbol{e}^\ast_i \cdot \chi_a\boldsymbol{e}_k \mathrm{d}V \right) \left(\frac{\omega_k}{\omega_i}\frac{\partial b_k}{\partial t} - \frac{\omega_k^2}{j2\omega_i}b_k\right) = \frac{1}{j2\omega_i}\int_V \boldsymbol{e}^\ast_i \cdot \chi_b\boldsymbol{E}_a \mathrm{d}V
$$
最后，再利用一次振幅缓变近似 $\partial b_k/\partial t = -j\omega_k b_k + (\partial B_k/\partial t) e^{-j\omega_k t} \approx -j\omega_k b_k$ ，就可以得到最终的形式
$$
\frac{\partial b_i}{\partial t} + j\omega_ib_i - j\sum_{k \neq i} \left( \int_V \boldsymbol{e}^\ast_i \cdot \chi_a\boldsymbol{e}_k \mathrm{d}V \right) \frac{\omega_k^2}{2\omega_i}b_k = \frac{1}{j2\omega_i}\int_V \boldsymbol{e}^\ast_i \cdot \chi_b\boldsymbol{E}_a \mathrm{d}V
\tag{Cavity}
$$

## 耦回过程

考虑光的耦回时，以热池光作为观察对象，方程 (1) 给出
$$
\nabla \times (\nabla \times \boldsymbol{E}_a) + \frac{1 + \chi_b + \chi_a}{c^2}\frac{\partial^2 \boldsymbol{E}_a}{\partial t^2} = -\frac{\chi_a}{c^2}\frac{\partial^2 \boldsymbol{E}_b}{\partial t^2}
$$
同样地，将热池光写为 $\boldsymbol{E}_a(\boldsymbol{r},t) = a(\omega, t)\boldsymbol{e}_a(\omega,\boldsymbol{r})$ ，进行和上面完全一样的操作，与 $\boldsymbol{e}_a$ 做内积就得到
$$
2j\omega\frac{\partial a}{\partial t}\left( 1 + \int_V \boldsymbol{e}^\ast_a \cdot \chi_b \boldsymbol{e}_a \mathrm{d}V \right) - \omega^2 a \left( 2 + \int_V \boldsymbol{e}^\ast_a \cdot \chi_b \boldsymbol{e}_a \mathrm{d}V \right)  = \int_V \boldsymbol{e}^\ast_a \cdot \chi_a\boldsymbol{E}_b \mathrm{d}V
$$

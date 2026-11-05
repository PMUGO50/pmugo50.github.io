---
title: 振幅缓变近似的耦合方程
date: 2026-11-03 13:57:29
categories: 光和电磁的描述
mathjax: true
description: 对Maxwell方程组应用WKB近似得到耦合模方程
---

# 正交本征模

忽略非线性效应和折射率的时变，即只考虑**互易定理得到满足**的情况。将 Maxwell 方程组的第三式（Faraday 电磁感应定律）和第四式（Ampere-Maxwell 环路定律）互相代入，并利用无源条件下的第一式（Gauss 定律），就得到波方程
$$
\nabla \times (\nabla \times \boldsymbol{E}) + \frac{\varepsilon}{c^2}\frac{\partial^2 \boldsymbol{E}}{\partial t^2} = 0
$$
在理想情况下，通过谐振腔的边界约束可以解出一系列正交的本征模式 $\boldsymbol{E}_i(\boldsymbol{r},t) = b_i e^{-j\omega_i t} \boldsymbol{e}_i(\boldsymbol{r})$ ，所谓正交是指方向向量满足
$$
\int_V \boldsymbol{e}^\ast_k(\boldsymbol{r}) \cdot \varepsilon\boldsymbol{e}_i(\boldsymbol{r}) \mathrm{d}V = \delta_{ik}
$$
当然，总的 $\boldsymbol{E}(\boldsymbol{r},t) = \sum_i \boldsymbol{E}_i(\boldsymbol{r},t)$ ，由于模式的正交性，那么代入到波方程中就得到
$$
\nabla \times (\nabla \times \boldsymbol{e}_i) - \frac{\varepsilon\omega_i^2}{c^2}\boldsymbol{e}_i = 0
$$

同样地，在全空间中只有热池光而没有谐振腔的情况下，热池光可以写为 $\boldsymbol{E}_b(\boldsymbol{r},t) = a e^{-j\omega t} \boldsymbol{e}_b(\boldsymbol{r})$ ，而且 $\boldsymbol{e}_b$ 同样也满足上式

# 施加微扰

不论是通过热池耦合，或是材料折射率分布不均，总之都可以写为 $\varepsilon'(\boldsymbol{r}) = \varepsilon(\boldsymbol{r}) + \delta\varepsilon(\boldsymbol{r})$ ；将微扰下的谐振模式写为 $\boldsymbol{E}_i(\boldsymbol{r},t) = a_i(t) \boldsymbol{e}_i(\boldsymbol{r})$ ，这里 $a_i(t) = A_i(t)e^{-j\omega_i t}$ ，且 $|\partial A_i/\partial t| \ll \omega_i$ ，即振幅相对于相位是缓变的

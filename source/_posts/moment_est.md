---
title: 矩估计与正态检验
date: 2026-10-16 21:25:37
categories: 时间序列处理
mathjax: true
description: 对各阶矩的估计以及对分布是否正态的检验
---

# 矩的估计

## 样本均值

假定按时间顺序获得了一系列对数收益率样本 $r_i(\Delta t_i)$ ，且它们**互相独立**，包围的时间范围也不重合。记样本总长度是 $n$ ，所有 $\Delta t_i$ 的和记为总时长 $T$ ，那么对数收益率均值的估计值是

$$
\bar{r} = \frac{1}{T} \sum_{i=1}^n r_i(\Delta t_i)
$$

求样本均值的期望

$$
\mathrm{E}(\bar{r}) = \frac{1}{T} \sum_{i=1}^n \mathrm{E}(r_i) = \mu'\frac{1}{T} \sum_{i=1}^n \Delta t_i = \mu'
$$

随着 $T \rightarrow \infty$ ，方差趋向于零。因而由 Khinchin 大数定律，$\bar{r} \overset{\mathrm{p}}{\rightarrow} \mu$ 

## 样本方差

在以 $\bar{r}$ 作为期望的估计值的情况下，样本方差的估计值是
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
这意味着该估计是无偏的。随着 $T \rightarrow \infty$ ，同样由 Khinchin 大数定律 $s^2 \overset{\mathrm{p}}{\rightarrow} \sigma^2$ ，这意味着这样对方差的估计也是相合的

{% note primary %}

**期望和波动的无偏估计**：如果样本 $\{r_i(\Delta t_i)\}$ 满足独立同分布，那么由样本矩对总体矩进行的无偏估计是
$$
\bar{r} = \frac{1}{T} \sum_{i=1}^n r_i(\Delta t_i) \overset{\mathrm{p}}{\longrightarrow} \mu \quad,\quad s^2 = \frac{1}{\Theta} \sum_{i=1}^n [r_i(\Delta t_i) - \bar{r}\Delta t_i]^2 \overset{\mathrm{p}}{\longrightarrow} \sigma^2
$$
这里 $T$ 和 $\Theta$ 分别是
$$
T = \sum_{i=1}^n \Delta t_i \quad,\quad \Theta = T - \frac{1}{T}\sum_{i=1}^n (\Delta t_i)^2
$$

{% endnote %}

特别地，如果记 $T_\mathrm{Annual}$ 是一年的时长，那么年化期望和年化波动就可以估计为

$$
\mu_\mathrm{Annual} = \mu T_\mathrm{Annual} \approx  \bar{r}\ T_\mathrm{Annual} \quad,\quad \sigma_\mathrm{Annual} = \sigma\sqrt{T_\mathrm{Annual}} \approx s \sqrt{T_\mathrm{Annual}}
$$


## 高阶样本矩

对于更高阶的归一化中心矩，在已经准确得知期望的情况下，用样本矩进行估计的值是

$$
b_m = \frac{1}{T} \sum_{i=1}^n \left[ \frac{r_i(\Delta t_i) - \bar{r}\Delta t_i}{\sqrt{\Delta t_i}} \right]^m \Delta t_i
$$

特别地，偏度和峰度估计为

$$
\hat{\kappa}_3 = \frac{b_3}{s^3} \quad,\quad \hat{\kappa}_4 = \frac{b_4}{s^4}
$$
超额峰度就是 $\hat{\kappa}_{4,\mathrm{Ex}} = \hat{\kappa}_4 -3$ 

# 正态性检验

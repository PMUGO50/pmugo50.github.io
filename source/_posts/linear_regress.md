---
title: 方差最小化的线性回归
date: 2026-12-02 22:49:56
categories: 时间序列处理
mathjax: true
description: 以方差最小化为目标的线性回归
---

# 最小二乘回归

## 参数估计

假定随机变量 $y$ 可以由随机向量 $\boldsymbol{x}$ 各分量的线性组合来表示，即 $y = \boldsymbol{\beta}^T \boldsymbol{x} + \alpha + u$ ，这里 $u$ 是一个代表线性关系无法解释的误差随机变量；那么对于样本序列 $\{ \boldsymbol{x}_i \}$ 和 $\{y_i\}$ ，理论上应当满足
$$
y_i = \boldsymbol{\beta}^T \boldsymbol{x}_i + \alpha + u_i
$$
从已知的样本序列来估计 $\boldsymbol{\beta}, \alpha$ 的问题被称为线性回归。回归模型给出预测值 $\hat{y} = \hat{\boldsymbol{\beta}}^T \boldsymbol{x} + \hat{\alpha}$ ，对每一个样本来说就是
$$
\hat{y}_i = \hat{\boldsymbol{\beta}}^T \boldsymbol{x}_i + \hat{\alpha}
$$
预测值与实际值之间的误差被称为残差 $e_i = y_i - \hat{y}_i$ ；特别地，如果满足以下条件：

1. **线性解释是充分的**，即 $u$ 与 $\boldsymbol{x}$ 之间已经没有相关性，且在已知 $\boldsymbol{x_i}$ 的情况下 $\{ u_i \}$ 是一个白噪声序列
2. **线性解释变量的选择是必要的**，即对于 $\{ \boldsymbol{x}_i \}$ 中任意一个分量，其无法始终被另一个分量线性表出

此时回归的解法被称为普通最小二乘（OLS），它的做法是令总残差的平方和最小化。首先，残差的平方和是
$$
E^2 = \sum_{i=1}^n e_i^2 = \sum_{i=1}^n \left( y_i - \hat{\boldsymbol{\beta}}^T \boldsymbol{x}_i - \hat{\alpha} \right)^2
$$
它是关于估计值 $\hat{\boldsymbol{\beta}}, \hat{\alpha}$ 的二次函数。因而应当只有一个极值点，极值条件由梯度为零给出，即
$$
\begin{aligned}
& \frac{\partial E^2}{\partial \hat{\boldsymbol{\beta}}} = 2\sum_{i=1}^n \left(\hat{\boldsymbol{\beta}}^T \boldsymbol{x}_i + \hat{\alpha} - y_i\right)\boldsymbol{x}_i^T = 0 \\[12pt]
& \frac{\partial E^2}{\partial \hat{\alpha}} = 2\sum_{i=1}^n \left(\hat{\boldsymbol{\beta}}^T \boldsymbol{x}_i + \hat{\alpha} - y_i\right) = 0
\end{aligned}
$$

将上面两式分别做转置之后，成为两个线性方程组
$$
\left(  \sum_{i=1}^n \boldsymbol{x}_i \boldsymbol{x}_i^T \right)\hat{\boldsymbol{\beta}} + \left( \sum_{i=1}^n \boldsymbol{x}_i \right)\hat{\alpha} = \sum_{i=1}^n y_i \boldsymbol{x}_i \quad,\quad \left(  \sum_{i=1}^n \boldsymbol{x}_i^T \right)\hat{\boldsymbol{\beta}} + n\hat{\alpha} = \sum_{i=1}^n y_i
$$
由第二式得到 $\hat{\alpha} = \bar{y} - \bar{\boldsymbol{x}}^T\hat{\boldsymbol{\beta}}$ ，代入第一式，就可以写出
$$
\hat{\boldsymbol{\beta}} = \left( \sum_{i=1}^n \boldsymbol{x}_i \boldsymbol{x}_i^T - n\bar{\boldsymbol{x}}\bar{\boldsymbol{x}}^T \right)^{-1} \left( \sum_{i=1}^n y_i \boldsymbol{x}_i - n\bar{y}\bar{\boldsymbol{x}} \right)
$$
因而先求得 $\hat{\boldsymbol{\beta}}$ ，进而就可以得到 $\hat{\alpha}$ 

## 估计的无偏性

将 $\hat{\boldsymbol{\beta}}$ 表达式中的 $y_i$ 写为 $y_i = \boldsymbol{x}_i^T \boldsymbol{\beta} + \alpha + u_i$ ，那么同样有 $\bar{y} = \bar{\boldsymbol{x}}^T \boldsymbol{\beta} + \alpha + \bar{u}$ ，就得到
$$
\begin{aligned}
\hat{\boldsymbol{\beta}} &= \left( \sum_{i=1}^n \boldsymbol{x}_i \boldsymbol{x}_i^T - n\bar{\boldsymbol{x}}\bar{\boldsymbol{x}}^T \right)^{-1} \left[ \left(\sum_{i=1}^n \boldsymbol{x}_i\boldsymbol{x}_i^T \boldsymbol{\beta} - n\bar{\boldsymbol{x}} \bar{\boldsymbol{x}}^T\boldsymbol{\beta}\right) + \left(\sum_{i=1}^n u_i\boldsymbol{x}_i - n\bar{u}\bar{\boldsymbol{x}}\right) \right] \\[12pt]
&= \boldsymbol{\beta} + \left( \sum_{i=1}^n \boldsymbol{x}_i \boldsymbol{x}_i^T - n\bar{\boldsymbol{x}}\bar{\boldsymbol{x}}^T \right)^{-1} \left(\sum_{i=1}^n u_i\boldsymbol{x}_i - n\bar{u}\bar{\boldsymbol{x}}\right)
\end{aligned}
$$
序列 $\{\boldsymbol{x}_i\}$ 是已经通过测量得知的，所以对上式求条件期望
$$
\mathrm{E}(\hat{\boldsymbol{\beta}}|\{\boldsymbol{x}_i\}) = \boldsymbol{\beta} + \left( \sum_{i=1}^n \boldsymbol{x}_i \boldsymbol{x}_i^T - n\bar{\boldsymbol{x}}\bar{\boldsymbol{x}}^T \right)^{-1}\left(\sum_{i=1}^n \mathrm{E}(u_i|\{\boldsymbol{x}_i\})\boldsymbol{x}_i - n\mathrm{E}(\bar{u}|\{\boldsymbol{x}_i\})\bar{\boldsymbol{x}}\right)
$$
由于前面假定已知 $\boldsymbol{x_i}$ 的情况下 $\{ u_i \}$ 是一个白噪声序列，所以样本均值满足
$$
\mathrm{E}(\bar{u}|\{\boldsymbol{x}_i\}) = \frac{1}{n}\sum_{i=1}^n \mathrm{E}(u_i|\{\boldsymbol{x}_i\}) = 0
$$
这就给出了 $\mathrm{E}(\hat{\boldsymbol{\beta}}|\{\boldsymbol{x}_i\}) = \boldsymbol{\beta}$ ；于是立刻有
$$
\mathrm{E}(\hat{\alpha}|\{\boldsymbol{x}_i\}) = \bar{\boldsymbol{x}}^T \mathrm{E}[(\boldsymbol{\beta} - \hat{\boldsymbol{\beta}})|\{\boldsymbol{x}_i\}] + \alpha + \mathrm{E}(\bar{u}|\{\boldsymbol{x}_i\}) = \alpha
$$
最后应用
$$
\mathrm{E}(\hat{\boldsymbol{\beta}}) = \int \hat{\boldsymbol{\beta}} p(\hat{\boldsymbol{\beta}}, \boldsymbol{x}_1, \cdots, \boldsymbol{x}_n) \mathrm{d}\hat{\boldsymbol{\beta}}\mathrm{d}\boldsymbol{x}_1\cdots\mathrm{d}\boldsymbol{x}_n = \int  \left[ \int \hat{\boldsymbol{\beta}} \frac{p(\hat{\boldsymbol{\beta}}, \boldsymbol{x}_1, \cdots, \boldsymbol{x}_n)}{p_x(\boldsymbol{x}_1, \cdots, \boldsymbol{x}_n)} \mathrm{d}\hat{\boldsymbol{\beta}} \right]  p_x(\boldsymbol{x}_1, \cdots, \boldsymbol{x}_n)\mathrm{d}\boldsymbol{x}_1\cdots\mathrm{d}\boldsymbol{x}_n
$$

这里 $p_x = \int_{-\infty}^{+\infty} p \mathrm{d}\hat{\boldsymbol{\beta}}$ 是序列 $\{ \boldsymbol{x}_i \}$ 的联合边际概率；事实上上式就是
$$
\mathrm{E}(\hat{\boldsymbol{\beta}}) = \mathrm{E}_x[ \mathrm{E}(\hat{\boldsymbol{\beta}}|\{\boldsymbol{x}_i\}) ] = \mathrm{E}_x( \boldsymbol{\beta} ) = \boldsymbol{\beta}
$$
对 $\hat{\alpha}$ 也是同样的道理，所以就有了

{% note primary %}

**普通最小二乘估计的无偏性**：如果用随机变量 $\boldsymbol{x}$ 对随机变量 $y$  进行线性解释的关系式为 $y = \boldsymbol{\beta}^T \boldsymbol{x} + \alpha + u$ ，且在已知 $\boldsymbol{x_i}$ 的情况下 $\{ u_i \}$ 是白噪声序列，那么用普通最小二乘估计出的参数 $\hat{\boldsymbol{\beta}}, \hat{\alpha}$ 都是无偏的，即 $\mathrm{E}(\hat{\boldsymbol{\beta}}|\{\boldsymbol{x}_i\}) = \boldsymbol{\beta}$ ；$\mathrm{E}(\hat{\alpha}|\{\boldsymbol{x}_i\}) = \alpha$ 

{% endnote %}

## 估计的相合性

将 $\hat{\boldsymbol{\beta}}$ 写为
$$
\hat{\boldsymbol{\beta}} - \boldsymbol{\beta} = \left( \frac{1}{n}\sum_{i=1}^n \boldsymbol{x}_i \boldsymbol{x}_i^T - \bar{\boldsymbol{x}}\bar{\boldsymbol{x}}^T \right)^{-1} \left( \frac{1}{n}\sum_{i=1}^n u_i\boldsymbol{x}_i - \bar{u}\bar{\boldsymbol{x}}\right)
$$
按照 Khinchin 大数定律，只要对应的期望存在，那么当 $n \rightarrow \infty$ 时，上式依概率收敛到
$$
\hat{\boldsymbol{\beta}} - \boldsymbol{\beta} \overset{\mathrm{p}}{\longrightarrow} \mathrm{Cov}(\boldsymbol{x}, \boldsymbol{x})^{-1} \mathrm{Cov}(u, \boldsymbol{x})
$$
这里两个协方差矩阵（向量）的行列分量分别是
$$
\begin{aligned}
& \mathrm{Cov}(\boldsymbol{x}, \boldsymbol{x})_{pq} = \mathrm{Cov}(x_p, x_q) = \mathrm{E}(x_px_q) - \mathrm{E}(x_p)\mathrm{E}(x_q) \\[6pt]
& \mathrm{Cov}(u, \boldsymbol{x})_q = \mathrm{Cov}(u, x_q) = \mathrm{E}(ux_q) - \mathrm{E}(u)\mathrm{E}(x_q)
\end{aligned}
$$
线性解释的充分假设已经表明 $u$ 与 $\boldsymbol{x}$ 之间没有相关性，即 $\mathrm{Cov}(u, \boldsymbol{x}) = 0$ ，所以 $\hat{\boldsymbol{\beta}} - \boldsymbol{\beta} \overset{\mathrm{p}}{\longrightarrow} 0$ 

对于 $\hat{\alpha}$ 则有 $\hat{\alpha} - \alpha = \bar{\boldsymbol{x}}^T (\boldsymbol{\beta}-\hat{\boldsymbol{\beta}}) + \bar{u}$ ，大数定律给出
$$
\hat{\alpha} - \alpha \overset{\mathrm{p}}{\longrightarrow} \mathrm{E}(\boldsymbol{x}) (\boldsymbol{\beta}-\hat{\boldsymbol{\beta}}) + \mathrm{E}(u) \overset{\mathrm{p}}{\longrightarrow} \mathrm{E}(u)
$$
这里 $\mathrm{E}(u) = \mathrm{E}_x[\mathrm{E}(u|\boldsymbol{x})] = 0$ ，这来源于白噪声假设。所以

 {% note primary %}

**普通最小二乘估计的无偏性**：如果用随机变量 $\boldsymbol{x}$ 对随机变量 $y$  进行线性解释的关系式为 $y = \boldsymbol{\beta}^T \boldsymbol{x} + \alpha + u$ ，且 $\mathrm{E}(u|\boldsymbol{x}) = 0$ ，$\mathrm{Cov}(u, \boldsymbol{x}) = 0$ ，那么用普通最小二乘估计出的参数 $\hat{\boldsymbol{\beta}}, \hat{\alpha}$ 都是相合的，即 $\hat{\boldsymbol{\beta}} - \boldsymbol{\beta} \overset{\mathrm{p}}{\longrightarrow} 0$ ；$\hat{\alpha} - \alpha \overset{\mathrm{p}}{\longrightarrow} 0$ 

{% endnote %}

# 假设检验

---
title: 方差最小化的线性回归
date: 2026-12-02 22:49:56
categories: 时间序列处理
mathjax: true
description: 以方差最小化为目标的线性回归
---

# 最小二乘回归

## 参数估计

假定随机变量 $y$ 可以由随机向量 $\boldsymbol{x}$ 各分量的线性组合来表示，即 $y = \boldsymbol{x}^T \boldsymbol{\beta}  + \alpha + u$ ，这里 $u$ 是一个代表线性关系无法解释的误差随机变量；那么对于测量得到的样本序列 $\{ \boldsymbol{x}_i \}$ 和 $\{y_i\}$ ，理论上应当满足
$$
\left[\begin{matrix} y_1 \\ \vdots \\ y_n \end{matrix}\right] = \left[\begin{matrix} \boldsymbol{x}_1^T & 1 \\ \vdots & \vdots \\ \boldsymbol{x}_n^T & 1 \end{matrix}\right] \left[\begin{matrix} \boldsymbol{\beta} \\ \alpha \end{matrix}\right] + \left[\begin{matrix} u_1 \\ \vdots \\ u_n \end{matrix}\right] \quad
$$
将其记为 $\boldsymbol{Y} = \boldsymbol{X}\boldsymbol{\theta} + \boldsymbol{U}$ ，从已知的样本序列来估计 $\boldsymbol{\beta}, \alpha$ 的问题被称为线性回归。特别地，如果满足以下条件：
1. **线性解释是充分的**：即 $u$ 与 $\boldsymbol{x}$ 之间已经没有相关性，且在已知 $\boldsymbol{X}$ 的情况下 $\boldsymbol{U}$ 是一个白噪声序列
2. **线性解释变量的选择是必要的**：即对于 $\boldsymbol{X}$ 中任意一列，其无法被其它列线性表出，也就是说 $\boldsymbol{X}$ 是满秩的

此时回归的解法被称为普通最小二乘（OLS），它的思路是找到**欧氏空间**中的正交投影

首先从上式可以看出：

- 实际的采样值 $\boldsymbol{Y}$ 构成一个 $n$ 维向量
- 采样值 $\boldsymbol{X}$ 则构成一个 $n$ 行的矩阵，每一列就是一个 $n$ 维向量；这个向量组将张成 $n$ 维空间中的一个子空间，维数为 $\mathrm{dim}(\boldsymbol{x})+1$ 

向量组的一个线性组合同样位于这个子空间内，距离实际的采样值 $\boldsymbol{Y}$ 最近的那个线性组合就被称为是 OLS 的预测值 $\hat{\boldsymbol{Y}}$ ，将线性组合系数记为 $\hat{\theta}$ ，也就是说 $\hat{\boldsymbol{Y}} = \boldsymbol{X}\hat{\boldsymbol{\theta}}$ ；测量得到的值和回归模型预测值之间的误差用欧氏距离来度量，也就是
$$
E^2 = \left| \boldsymbol{Y} - \hat{\boldsymbol{Y}} \right|^2 = \left( \boldsymbol{Y} - \hat{\boldsymbol{Y}} \right)^T \left( \boldsymbol{Y} - \hat{\boldsymbol{Y}} \right) = \left( \boldsymbol{Y} - \boldsymbol{X}\hat{\boldsymbol{\theta}}\right)^T \left( \boldsymbol{Y} - \boldsymbol{X}\hat{\boldsymbol{\theta}} \right)
$$
它是关于估计值 $\hat{\boldsymbol{\theta}}$ 的二次函数，因而应当只有一个极值点。且由于其作为欧氏距离的意义，一定是一个极小值点。极值条件由梯度为零给出，即
$$
\frac{\partial E^2}{\partial \hat{\boldsymbol{\theta}}} = 2\boldsymbol{X}^T\left( \boldsymbol{X}\hat{\boldsymbol{\theta}} - \boldsymbol{Y} \right) = 0
$$

直接得到 OLS 的估计结果 $\hat{\boldsymbol{\theta}} = (\boldsymbol{X}^T\boldsymbol{X})^{-1} (\boldsymbol{X}^T\boldsymbol{Y})$ 

## 估计的无偏性

将 $\hat{\boldsymbol{\theta}}$ 表达式中的 $\boldsymbol{Y}$ 写为 $\boldsymbol{X}\boldsymbol{\theta} + \boldsymbol{U}$ ，那么就得到
$$
\hat{\boldsymbol{\theta}} = (\boldsymbol{X}^T\boldsymbol{X})^{-1} [(\boldsymbol{X}^T\boldsymbol{X})\boldsymbol{\theta} + \boldsymbol{X}^T\boldsymbol{U}] = \boldsymbol{\theta} + (\boldsymbol{X}^T\boldsymbol{X})^{-1}\boldsymbol{X}^T\boldsymbol{U}
$$
矩阵 $\boldsymbol{X}$ 是已经通过测量得知的，所以对上式求条件期望
$$
\mathrm{E}(\hat{\boldsymbol{\theta}}|\boldsymbol{X}) = \boldsymbol{\theta} + (\boldsymbol{X}^T\boldsymbol{X})^{-1}\boldsymbol{X}^T\mathrm{E}(\boldsymbol{U}|\boldsymbol{X})
$$
由于前面假定已知 $\boldsymbol{X}$ 的情况下 $\boldsymbol{U}$ 是一个白噪声序列，所以 $\mathrm{E}(\boldsymbol{U}|\boldsymbol{X}) = 0$ ，这就给出了 $\mathrm{E}(\hat{\boldsymbol{\theta}}|\boldsymbol{X}) = \boldsymbol{\theta}$ 

然后应用
$$
\mathrm{E}(\hat{\boldsymbol{\theta}}) = \int \hat{\boldsymbol{\theta}} \cdot p(\hat{\boldsymbol{\theta}}, \boldsymbol{x}_1, \cdots, \boldsymbol{x}_n) \mathrm{d}\hat{\boldsymbol{\theta}}\mathrm{d}\boldsymbol{x}_1\cdots\mathrm{d}\boldsymbol{x}_n = \int  \left[ \int \hat{\boldsymbol{\theta}} \frac{p(\hat{\boldsymbol{\theta}}, \boldsymbol{x}_1, \cdots, \boldsymbol{x}_n)}{p_x(\boldsymbol{x}_1, \cdots, \boldsymbol{x}_n)} \mathrm{d}\hat{\boldsymbol{\theta}} \right]  p_x(\boldsymbol{x}_1, \cdots, \boldsymbol{x}_n)\mathrm{d}\boldsymbol{x}_1\cdots\mathrm{d}\boldsymbol{x}_n
$$

这里 $p_x = \int_{-\infty}^{+\infty} p \mathrm{d}\hat{\boldsymbol{\theta}}$ 是序列 $\{ \boldsymbol{x}_i \}$ 的联合边际概率；事实上上式就是
$$
\mathrm{E}(\hat{\boldsymbol{\theta}}) = \mathrm{E}_x[ \mathrm{E}(\hat{\boldsymbol{\theta}}|\boldsymbol{X}) ] = \mathrm{E}_x( \boldsymbol{\theta} ) = \boldsymbol{\theta}
$$
所以就有了

{% note primary %}

**普通最小二乘估计的无偏性**：如果用随机变量 $\boldsymbol{x}$ 对随机变量 $y$  进行线性解释的关系式为 $y = \boldsymbol{\beta}^T \boldsymbol{x} + \alpha + u$ ，且在已知 $\boldsymbol{x_i}$ 的情况下 $\{ u_i \}$ 是白噪声序列，那么用普通最小二乘估计出的参数 $\hat{\boldsymbol{\beta}}, \hat{\alpha}$ 都是无偏的，即 $\mathrm{E}(\hat{\boldsymbol{\beta}}) = \boldsymbol{\beta}$ ；$\mathrm{E}(\hat{\alpha}) = \alpha$ 

{% endnote %}

## 估计的相合性

将 $\hat{\boldsymbol{\theta}}$ 写为 $\hat{\boldsymbol{\theta}} - \boldsymbol{\theta} = (\boldsymbol{X}^T\boldsymbol{X})^{-1}\boldsymbol{X}^T\boldsymbol{U}$ ，不妨展开写为
$$
\hat{\boldsymbol{\theta}} - \boldsymbol{\theta} = \left[\begin{matrix} \sum_{i=1}^n x_{1i}^2/n & \cdots & \sum_{i=1}^n x_{1i}/n \\ \vdots & \ddots & \vdots \\ \sum_{i=1}^n x_{1i}/n & \cdots & 1 \end{matrix}\right]^{-1} \left[\begin{matrix} \sum_{i=1}^n u_ix_{1i}/n \\ \vdots \\ \sum_{i=1}^n u_i/n \end{matrix}\right]
$$
按照 Khinchin 大数定律，只要对应的期望存在，那么当 $n \rightarrow \infty$ 时，上式依概率收敛到
$$
\hat{\boldsymbol{\theta}} - \boldsymbol{\theta} \overset{\mathrm{p}}{\longrightarrow} \left[\begin{matrix} \mathrm{Cov}(\boldsymbol{x}, \boldsymbol{x}) & \mathrm{E}(\boldsymbol{x}) \\ \mathrm{E}(\boldsymbol{x}^T) & 1 \end{matrix}\right]^{-1} \left[\begin{matrix} \mathrm{Cov}(u, \boldsymbol{x}) \\ \mathrm{E}(u) \end{matrix}\right]
$$
线性解释的充分假设已经表明 $u$ 与 $\boldsymbol{x}$ 之间没有相关性，即 $\mathrm{Cov}(u, \boldsymbol{x}) = 0$ ，且白噪声假设表明 $\mathrm{E}(u|\boldsymbol{x})=0$ ，所以
$$
\mathrm{E}(u) = \mathrm{E}_x[ \mathrm{E}(u|\boldsymbol{x}) ] = 0
$$
这就给出

 {% note primary %}

**普通最小二乘估计的无偏性**：如果用随机变量 $\boldsymbol{x}$ 对随机变量 $y$  进行线性解释的关系式为 $y = \boldsymbol{\beta}^T \boldsymbol{x} + \alpha + u$ ，且 $\mathrm{E}(u|\boldsymbol{x}) = 0$ ，$\mathrm{Cov}(u, \boldsymbol{x}) = 0$ ，那么用普通最小二乘估计出的参数 $\hat{\boldsymbol{\beta}}, \hat{\alpha}$ 都是相合的，即 $\hat{\boldsymbol{\beta}} - \boldsymbol{\beta} \overset{\mathrm{p}}{\longrightarrow} 0$ ；$\hat{\alpha} - \alpha \overset{\mathrm{p}}{\longrightarrow} 0$ 

{% endnote %}

# 假设检验

## 残差的分布

利用 $\hat{\boldsymbol{Y}} = \boldsymbol{X}\hat{\boldsymbol{\theta}}$ 以及 OLS 给出的 $\hat{\boldsymbol{\theta}} = (\boldsymbol{X}^T\boldsymbol{X})^{-1} (\boldsymbol{X}^T\boldsymbol{Y})$ ，将有
$$
\hat{\boldsymbol{Y}} = \left[\boldsymbol{X} (\boldsymbol{X}^T\boldsymbol{X})^{-1} \boldsymbol{X}^T \right]\boldsymbol{Y}
$$

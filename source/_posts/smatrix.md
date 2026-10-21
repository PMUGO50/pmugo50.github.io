---
title: 器件的散射矩阵
date: 2025-09-20 09:01:01
categories: 光和电磁的描述
mathjax: true
description: 散射矩阵以及可能的约束
---

## 散射矩阵

散射矩阵（scattering matrix）是一种描述线性光学系统的很有意思的工具。可以认为它来源于这样一个类比：以波导作为输入输出的光学系统往往有多个端口，对任何一个端口注入光，其它端口可能会有光射出，这就好似散射一样，因为散射正是一束输入光激励出多个输出光的过程

在定常且线性的情况下，考虑单色光，空间中的散射光与入射光的关系可以由下式描述（*M.Born and E.Wolf, "Principles of optics," 7th ed., Chap. 13, Sec. 1.*）
$$
A^{(s)}(\boldsymbol{r}) = g\left( \boldsymbol{m}_0^{(s)}, A^{(i)} \right) e^{-j2\pi m_0r}
$$
这里 $A^{(i)}, A^{(s)}$ 分别是入射光和散射光的复振幅，$\boldsymbol{m}_0^{(s)}$ 是散射光在真空中的线波矢，其幅值为真空线波数 $m_0=f/c$ ，方向指向光线传播的方向。在一阶近似下函数 $g$ 是
$$
g\left( \boldsymbol{m}_0^{(s)}, A^{(i)} \right) = \frac{1}{r}\iiint_V F(\boldsymbol{\xi})A^{(i)}(\boldsymbol{\xi})e^{j2\pi \boldsymbol{m}_0^{(s)} \cdot \boldsymbol{\xi}} \mathrm{d}V
$$
这里 $F$ 是散射势，是个只取决于散射物体折射率和入射光频率的量。总之上式总可以抽象地看成：对于给定折射率和形状的散射物体，散射光将取决于输入光和出射方向。那么在波导作为输入输出端口的光学系统中就是：**对于给定的线性定常光学系统，输出光将取决于输入光和输出的端口**，或者用数学形式来表达就是
$$
A^{(s)}_i = \sum_{k} S_{ik} A^{(i)}_k
$$
这里沿用了之前的上标，$\{S_{ik}\}$​​ 被称为散射矩阵，它表示了光学系统本身的特性

> 这里无需要求波导是单模的，对于多模波导，只需把同一个端口的不同模式等效为不同的端口即可

## 互易性约束

### 互易定理

互易性的一个经典表述来源于 H. A. Lorentz ，这个表述的推导参见 *H. Haus, "Waves and Fields in Optoelectronics," Chap. 3, Sec. 1.* 这里可以进行一个简要的复写，写出**惯性系中**没有自由电荷和电流源时 Maxwell 方程组的两个时变方程

$$
\nabla \times \boldsymbol{E} = -\frac{1}{c}\frac{\partial \boldsymbol{B}}{\partial t} \quad,\quad \nabla \times \boldsymbol{B} = \frac{1}{c}\frac{\partial \boldsymbol{E}}{\partial t}
$$
**在介电常数非时变的情况下**，考虑单色光，上式成为
$$
\nabla \times \boldsymbol{E} = -j2\pi \frac{f}{c} \boldsymbol{B} \quad,\quad \nabla \times \boldsymbol{B} = j2\pi \frac{f}{c} \boldsymbol{E}
$$
对于两束光（以 $i,k$ 区分），它们的电磁互能密度分别是
$$
\begin{aligned}
& \frac{n^2}{8\pi} \boldsymbol{E}_i \cdot \boldsymbol{E}_k = n^2\frac{c/8\pi}{j2\pi f} \boldsymbol{E}_i \cdot (\nabla \times \boldsymbol{B}_k) = n^2\frac{c/8\pi}{j2\pi f} \boldsymbol{E}_k \cdot (\nabla \times \boldsymbol{B}_i) \\[12pt]
& \frac{1}{8\pi} \boldsymbol{B}_i \cdot \boldsymbol{B}_k = -\frac{c/8\pi}{j2\pi f} \boldsymbol{B}_i \cdot (\nabla \times \boldsymbol{E}_k) = -\frac{c/8\pi}{j2\pi f} \boldsymbol{B}_k \cdot (\nabla \times \boldsymbol{E}_i)
\end{aligned}
$$
那么会有
$$
\boldsymbol{E}_i \cdot (\nabla \times \boldsymbol{B}_k) - 
 \boldsymbol{B}_k \cdot (\nabla \times \boldsymbol{E}_i) = \boldsymbol{E}_k \cdot (\nabla \times \boldsymbol{B}_i) - \boldsymbol{B}_i \cdot (\nabla \times \boldsymbol{E}_k)
$$
反用矢量微分恒等式 $\nabla \cdot (\boldsymbol{X}\times\boldsymbol{Y}) = \boldsymbol{Y} \cdot (\nabla \times \boldsymbol{X}) - \boldsymbol{X} \cdot (\nabla \times \boldsymbol{Y})$ ，就得到**微分形式的 Lorentz 互易定理**
$$
\nabla\cdot(\boldsymbol{E}_i \times \boldsymbol{B}_k) = \nabla\cdot(\boldsymbol{E}_k \times \boldsymbol{B}_i)
$$

直接应用微积分的 Gauss-Ostrograsky 公式，即可得到**积分形式的 Lorentz 互易定理** 
$$
\oint_S(\boldsymbol{E}_i \times \boldsymbol{B}_k)\cdot\mathrm{d}\boldsymbol{\sigma} = \oint_S(\boldsymbol{E}_k \times \boldsymbol{B}_i)\cdot\mathrm{d}\boldsymbol{\sigma}
$$

### 矩阵形式的互易性

对于以波导作为输入输出的光学系统来说，端口 $i$ 输出的电场将是 $A^{(s)}_i = \sum_k S_{ik} A^{(i)}_{k}$ 

至于磁场强度，对于单色波来讲，计算 $\nabla \times \boldsymbol{E} = -j2\pi (f/c) \boldsymbol{B}$ 的结果是 $n\boldsymbol{k}_0 \times \boldsymbol{E} = (2\pi f/c) \boldsymbol{B}$ ，这里 $\boldsymbol{k}_0$ 是角波矢,方向指向光的传播方向。那么磁场的幅度将满足 $B^{(s)}_i = nA^{(s)}_i$，$B^{(i)}_k = - nA^{(i)}_k$ ，负号来源于输入/输入光波方向的相反性

现在选择闭合曲面 $S$ 包围系统，并令曲面在波导的输入/输出端口处垂直于光的输入/输出方向。现在向系统中给两束光，分别在上标中以 $1,2$ 表示，那么 Lorentz 互易定理成为
$$
n \sum_i \left[A^{(s,1)}_i + A^{(i,1)}_i\right]\cdot\left[A^{(s,2)}_i - A^{(i,2)}_i\right] = n \sum_i \left[A^{(s,2)}_i + A^{(i,2)}_i\right]\cdot\left[A^{(s,1)}_i - A^{(i,1)}_i\right]
$$
这里假设了所有端口的有效截面积是相同的，这并不失一般性，因为总可以将面积等效到散射矩阵 $S_{ik}$ 中。约去方程等号两侧相等的部分，并移项得到
$$
\sum_i A^{(i,1)}_iA^{(s,2)}_i = \sum_i A^{(i,2)}_iA^{(s,1)}_i
$$
代入散射矩阵，上式成为
$$
\sum_{i,k} A^{(i,1)}_i S_{ik} A^{(i,2)}_k = \sum_{i,k} A^{(i,2)}_i S_{ik} A^{(i,1)}_k
$$
$i,k$ 遍历了同样的指标集合，所以交换等号右侧的指标即得到
$$
\sum_{i,k} A^{(i,1)}_i S_{ik} A^{(i,2)}_k = \sum_{i,k} A^{(i,2)}_k S_{ki} A^{(i,1)}_i
$$
由于两束光 $A^{(i,1)}_i, A^{(i,2)}_i$ 是任选的，所以上式成立的充要条件就成为**散射矩阵的互易性约束**，即对于线性定常的光学系统，其散射矩阵 $\{S_{ik}\}$ 必定满足
$$
S_{ik} = S_{ki}
$$
这是说散射矩阵是对称的。某种程度上可以说，互易性约束使得光学系统的任意两个端口之间传输的光路都是可逆的。或者也可以说（尽管略有一些武断），光路可逆的系统才是互易的

### 非惯性系对互易性的破坏

由推导互易定理的前提条件可以看出，要破坏互易性，可以破坏两个条件：“线性”和“定常”

- 非线性效应，即令极化强度 $\boldsymbol{P} = \chi_0 \varepsilon_0 \boldsymbol{E} + g(\boldsymbol{E})$ ，函数 $g$ 是非线性部分
- 非定常：令介电常数 $\epsilon=\epsilon(t)$ ，从而破坏光路的时间反演性；或者切换非惯性系，也同样可以破坏光路的时间反演性 

关于第二点，比较典型的例子是环形干涉仪。当干涉仪静止在真空中的惯性系时，光路是互易的，此时干涉仪的散射矩阵是
$$
S = \left[\begin{matrix} 0 & e^{-j2\pi f \tau_r} \\ e^{-j2\pi f \tau_r} & 0 \end{matrix}\right]
$$
这里 $\tau_r = L/c$ ，这确是个对称矩阵。但是当干涉仪相对于惯性系发生转动时，尽管介质仍然是线性定常的，但光路却不再互易，因为与干涉仪固连的参考系是非惯性的。从相对论的角度看，固连系的时空度规将被旋转的角速度调制。尤其是角速度是一个时间导数，在进行时间反演时会反向

在固连系中，沿两束相反方向传播的光将会存在时差（*L.Landau and E.Lifshitz, "The Classical Theory of Fields," 7th ed., Sec. 89.*）
$$
\Delta \tau = 2\oint \frac{\Omega r^2 \mathrm{d}\varphi}{1-\Omega^2 r^2/c^2} \approx \frac{4\sigma}{c^2}\Omega
$$
这里 $\sigma$ 是环路面积。那么此时散射矩阵成为
$$
S = \left[\begin{matrix} 0 & e^{-j2\pi f(\tau_r + \Delta \tau/2)} \\ e^{-j2\pi f(\tau_r - \Delta \tau/2)} & 0 \end{matrix}\right]
$$
散射矩阵成为非对称的形式，系统的互易性被转动带来的调制所破坏。这种由参考系转动带来的非互易现象也被称为 Sagnac 效应

## 无损约束

散射矩阵另一个可能存在的约束是无损约束，对于许多器件，这约束（至少在一定的近似程度上）是很有用的，往往可以利用其推出一些器件的理想性质

无损即是说对于从任意的端口 $k$ 输入的光，从所有端口 $q$ 输出的光的功率总和等于输入光的功率，即
$$
\sum_q A^{(s)}_{q}A^{(s)\ast}_{q} = \sum_k A^{(i)}_{k}A^{(i)\ast}_{k}
$$
代入散射矩阵，就是
$$
\sum_{i,k,q} A^{(i)}_{k}S_{qk} S_{qi}^\ast A^{(i)\ast}_{i} = \sum_k A^{(i)}_{k}A^{(i)\ast}_{k}
$$
由于输入的 $A^{(i)}_k$ 都是任选的，所以如果记 Kronecker 记号为 $\delta_{kq}$ ，上式给出**散射矩阵的无损约束**，即对于无损的光学系统，其散射矩阵必定满足
$$
\sum_q S_{qk}S_{qi}^\ast = \delta_{ik}
$$
这则是说散射矩阵与其 Hermitian 共轭之积是单位矩阵，或者其 Hermitian 共轭即为其逆矩阵




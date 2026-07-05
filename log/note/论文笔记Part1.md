# 论文笔记 Part 1：引言部分

> Motwani R, Naor A, Panigrahy R. Lower bounds on locality sensitive hashing[C]//Proceedings of the Twenty-second Annual Symposium on Computational Geometry. New York: ACM, 2006: 154-157.

## 1. 研究背景

论文研究的问题是：在高维空间中，使用局部敏感哈希 LSH 解决近似最近邻问题时，算法性能存在怎样的理论下界。

最近邻问题是：给定度量空间中的 n 个数据点，先进行预处理。之后给定一个查询点，希望快速找到数据集中距离它最近的点。在高维空间中，精确最近邻通常会受到维数灾难影响，因此实际中更常研究近似最近邻。

## 2. 近似最近邻问题

给定近似因子 c，允许返回一个距离查询点不超过真实最近邻距离 c 倍的数据点。

真实最近邻距离为：
$$
\min_{x \in P} d(q,x)
$$
c 近似最近邻允许返回某个点 x，使得：
$$
d(q,x) \le c \cdot \min_{y \in P} d(q,y)
$$
当 c 等于 1 时，是精确最近邻；而当 c 大于 1 时，是近似最近邻，且 c 越大，问题越容易。

## 3. LSH 的基本定义

设有度量空间：
$$
(X,d_X)
$$
其中 r 和 R 是距离阈值，p 和 q 是碰撞概率参数。

一个随机哈希函数族：
$$
H:X\to \mathbb{N}
$$
称为：
$$
(r,R,p,q)\text{-sensitive hash family}
$$
如果对任意两点 x 和 y，满足以下两条性质。

近点更容易碰撞：
$$
d_X(x,y)\le r
\Rightarrow
\Pr_H[H(x)=H(y)]\ge p
$$
远点不容易碰撞：
$$
d_X(x,y)>R
\Rightarrow
\Pr_H[H(x)=H(y)]\le q
$$
直观理解：距离近的点应该更容易哈希到同一个桶，距离远的点应该不容易哈希到同一个桶。

在近似最近邻问题中，通常取：
$$
R=cr
$$
于是研究的是：
$$
(r,cr,p,q)\text{-sensitive hash family}
$$

## 4. 核心参数 $$\rho$$

LSH 算法性能由参数 rho 控制：
$$
\rho=\frac{\log(1/p)}{\log(1/q)}
$$
其中：

- p 是近点碰撞概率下界，越大越好
- q 是远点碰撞概率上界，越小越好
- $$\rho$$ 越小，算法越快

论文定义：
$$
\rho_X(c,q)
=
\sup_{r>0}
\inf
\left\{
\frac{\log(1/p)}{\log(1/q)}
:
\exists H:X\to\mathbb{N}
\text{ is an }(r,cr,p,q)\text{-sensitive hash family}
\right\}
$$
含义：在空间 X 中，对于近似因子 c 和远点碰撞概率 q，LSH 能达到的最优复杂度指数。

**Indyk 和 Motwani 的定理说明：**

如果存在：
$$
(r,cr,p,q)\text{-sensitive hash family}
$$
并令：
$$
\rho=\frac{\log(1/p)}{\log(1/q)}
$$
那么可以构造随机算法解决近邻问题。

空间复杂度为：
$$
O(dn+n^{1+\rho})
$$
查询时间主要包括：
$$
O(n^\rho)
$$
次距离计算，以及：
$$
O(n^\rho \log_{1/q}n)
$$
次哈希函数计算。

因此 $$\rho$$ 是 LSH 查询复杂度中的核心指数。

例如：

- $$\rho$$ 越接近 1，越接近线性扫描
- $$\rho$$ 小，查询越快
- 构造更小 $$\rho$$ 的哈希族，就能得到更快的近似最近邻算法

## 5. 高维 $$\ell_s$$ 空间

论文重点研究：
$$
X=\ell_s^d
$$
也就是 d 维实数空间配上 $$\ell_s$$ 范数：
$$
|(x_1,\dots,x_d)|_s
=
\left(
|x_1|^s+\cdots+|x_d|^s
\right)^{1/s}
$$
常见例子：

- s 等于 1：曼哈顿距离
- s 等于 2：欧氏距离
- s 等于无穷：最大坐标距离

论文定义高维渐近参数：
$$
\rho_s(c)
=
\sup_{0<q<1}
\limsup_{d\to\infty}
\rho_{\ell_s^d}(c,q)
$$

描述的是当维度 d 趋于无穷时，$$\ell_s$$ 空间中 LSH 参数 $$\rho$$ 的最优渐近表现。

## 6. 已知上界

已有 LSH 构造给出了如下上界。

对于 $$\ell_1$$ 空间：
$$
\rho_1(c)\le \frac{1}{c}
$$
对于 $$\ell_2$$ 空间：
$$
\rho_2(c)\le \frac{1}{c^2}
$$
对于一般的 s，已有最好上界为：
$$
\rho_s(c)\le \max\left\{\frac{1}{c},\frac{1}{c^s}\right\}
$$

## 7. 本文主要结果

论文证明：已有上界基本不能再显著改进。

主定理为：
$$
\rho_s(c)
\ge
\frac{e^{1/c^s}-1}{e^{1/c^s}+1}
\ge
\frac{e-1}{e+1}\cdot \frac{1}{c^s}
\ge
\frac{0.462}{c^s}
$$
因此：
$$
\rho_s(c)=\Omega\left(\frac{1}{c^s}\right)
$$
这说明：

在 $$\ell_s$$ 空间中，LSH 的复杂度指数不可能低于常数倍的：
$$
\frac{1}{c^s}
$$

## 8. 对 $$\ell_1$$ 的意义

对于 $$\ell_1$$，论文证明：
$$
\rho_1(c)
\gtrsim
\frac{1}{2c}
$$
已有上界是：
$$
\rho_1(c)\le \frac{1}{c}
$$
因此：
$$
\frac{1}{2c}
\lesssim
\rho_1(c)
\le
\frac{1}{c}
$$
这说明 $$\ell_1$$ 中的 LSH 上下界只差一个常数因子。

换句话说：Indyk 和 Motwani 的 LSH 构造在 $$\ell_1$$ 空间中已经接近最优。

## 9. 对 $$\ell_2$$ 的意义

对于 $$\ell_2$$，论文证明：
$$
\rho_2(c)
\ge
\Omega\left(\frac{1}{c^2}\right)
$$
已有上界是：
$$
\rho_2(c)\le \frac{1}{c^2}
$$
因此从渐近阶来看：
$$
\rho_2(c)=\Theta\left(\frac{1}{c^2}\right)
$$
这说明 $$\ell_2$$ 情况下，LSH 的理论性能已经基本被刻画清楚。

## 10. 引言核心逻辑

论文引言的逻辑链是：

$$
高维最近邻困难
\\
\Downarrow
\\
转向近似最近邻
\\
\Downarrow
\\
LSH 是重要方法
\\
\Downarrow
\\
LSH 性能由 \rho 控制
\\
\Downarrow
\\
已有构造给出 \rho 的上界
\\
\Downarrow
\\
本文证明 \rho 的下界
$$

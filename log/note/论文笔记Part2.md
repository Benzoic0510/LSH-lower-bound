# 论文笔记 Part 2：主定理证明部分

## 1. 本节目标

第 2 节的目标是证明论文的主定理：
$$
\rho_s(c)
\ge
\frac{e^{1/c^s}-1}{e^{1/c^s}+1}
\ge
\frac{0.462}{c^s}
$$
也就是说，在高维 $$\ell_s$$ 空间中，任意 LSH 方法的复杂度指数 $$\rho$$ 都不可能低于常数倍的：
$$
\frac{1}{c^s}
$$
因此已有的 LSH 上界基本是最优的。

## 2. 证明的基本思想

作者先不直接研究整个高维空间 $$\ell_s^d$$，而是研究它的一个特殊子集：
$$
\{0,1\}^d
$$
这个集合称为 Hamming 立方体。

对于两个点：
$$
x,y\in \{0,1\}^d
$$
它们的 $$\ell_1$$ 距离等于它们不同坐标的个数：
$$
|x-y|_1
=
\text{Hamming distance}(x,y)
$$
证明的核心思想是：

1. 随机选择一个点 $$x\in\{0,1\}^d$$；
2. 考虑所有和 $$x$$ 哈希到同一个值的点；
3. 这些点组成一个哈希桶：$$A=H^{-1}(H(x))$$
4. 远点碰撞概率小，说明这个桶不能太大；
5. 如果桶很小，那么随机游走从桶中出发，走 $$r$$ 步后还留在桶里的概率也不会太大；
6. 但近点碰撞概率要求这个概率至少为 $$p$$；
7. 两者比较，就能推出 $$p$$ 的上界，从而推出 $$\rho$$ 的下界。

即：
$$
\text{远点条件限制桶大小}
\quad+\quad
\text{随机游走限制留在桶内的概率}
\quad\Rightarrow\quad
\text{近点碰撞概率 }p\text{ 不能太大}
$$

## 3. Hamming 立方体

Hamming 立方体是：
$$
\{0,1\}^d
$$
它包含所有长度为 $$d$$ 的 0-1 向量，例如当 $$d=3$$ 时：
$$
000,\ 001,\ 010,\ 011,\ 100,\ 101,\ 110,\ 111
$$
总共有：
$$
2^d
$$
个点。

两个点之间的 Hamming 距离是它们不同坐标的个数。

例如：
$$
x=(1,0,1,1),\quad y=(1,1,0,1)
$$
它们在第 2 个和第 3 个坐标不同，所以：
$$
|x-y|_1=2
$$
在 Hamming 立方体中，半径为 $$R$$ 的球的大小是：
$$
\sum_{k=0}^{\lfloor R\rfloor}
\binom{d}{k}
$$
原因是：距离中心点恰好为 $$k$$ 的点，就是从 $$d$$ 个坐标中选出 $$k$$ 个坐标翻转，因此数量为：
$$
\binom{d}{k}
$$
所以距离不超过 $$R$$ 的点数为：
$$
\sum_{k=0}^{\lfloor R\rfloor}
\binom{d}{k}
$$

## 4. 命题 2.1：核心命题

设 $$H$$ 是 Hamming 立方体：
$$
(\{0,1\}^d,|\cdot|_1)
$$
上的一个：
$$
(r,R,p,q)\text{-sensitive hash family}
$$
并假设：
$$
r\text{ 是奇整数},\quad R<\frac d2
$$
则有：
$$
p
\le
\left(
q+
\exp\left(
-\frac{1}{d}
\left(
\frac d2-R
\right)^2
\right)
\right)^{
\frac{e^{2r/d}-1}{e^{2r/d}+1}
}
$$
这个命题是整篇论文证明的核心。

它说明：如果远点碰撞概率 $$q$$ 很小，那么近点碰撞概率 $$p$$ 也不能任意大。

换句话说，LSH 的两个要求之间存在冲突：

近点要求：
$$
\Pr[H(x)=H(y)]\ge p
$$
远点要求：
$$
\Pr[H(x)=H(y)]\le q
$$
命题 2.1 把这种冲突量化成了一个不等式。

## 5. 引理 2.2：哈希桶大小的期望上界

固定一个点：
$$
x\in\{0,1\}^d
$$
它所在的哈希桶是：
$$
H(x)
$$
其中：
$$
H^{-1}(H(x))
$$
表示所有被哈希到 $$H(x)$$ 这个值的点，即：
$$
H^{-1}(H(x))
=
{u\in\{0,1\}^d:H(u)=H(x)}
$$

引理 2.2 说明：
$$
\mathbb{E}|H^{-1}(H(x))|
\le
\sum_{k=0}^{\lfloor R\rfloor}
\binom dk
+
q
\sum_{k=\lfloor R\rfloor+1}^{d}
\binom dk
$$
解释如下。

对于每个点：
$$
u\in\{0,1\}^d
$$
它是否属于桶 $$H^{-1}(H(x))$$，取决于事件：
$$
H(u)=H(x)
$$
所以桶大小的期望是：
$$
\mathbb{E}|H^{-1}(H(x))|
=
\sum_{u\in{0,1}^d}
\Pr[H(u)=H(x)]
$$
把所有点 $$u$$ 分成两类。

**第一类：距离 $$x$$ 不超过 $$R$$ 的点**

这些点满足：
$$
|u-x|_1\le R
$$
它们的数量是：
$$
\sum_{k=0}^{\lfloor R\rfloor}
\binom dk
$$
对这些点，远点条件不能使用，所以只能用最粗略的上界：
$$
\Pr[H(u)=H(x)]\le 1
$$
因此这一部分贡献最多为：
$$
\sum_{k=0}^{\lfloor R\rfloor}
\binom dk
$$
**第二类：距离 $$x$$ 大于 $$R$$ 的点**

这些点满足：
$$
|u-x|_1>R
$$
由 LSH 的远点条件可知：
$$
\Pr[H(u)=H(x)]\le q
$$
这些点数量是：
$$
\sum_{k=\lfloor R\rfloor+1}^{d}
\binom dk
$$
所以这一部分贡献最多为：
$$
q
\sum_{k=\lfloor R\rfloor+1}^{d}
\binom dk
$$
合并得到引理 2.2。  

## 6. 推论 2.3：归一化桶大小的上界

整个 Hamming 立方体的大小是：
$$
2^d
$$
所以可以把桶大小除以 $$2^d$$，得到桶的相对密度：
$$
\frac{|H^{-1}(H(x))|}{2^d}
$$
推论 2.3 说明，当：
$$
R<\frac d2
$$
时，有：
$$
\mathbb{E}
\frac{|H^{-1}(H(x))|}{2^d}
\le
q+
\exp\left(
-\frac{1}{d}
\left(
\frac d2-R
\right)^2
\right)
$$
证明依赖于 Hamming 球体积的标准估计：
$$
\sum_{k\le d/2-a}
\binom dk
\le
2^d e^{-a^2/d}
$$
令：
$$
a=\frac d2-R
$$
则：
$$
\sum_{k\le R}
\binom dk
\le
2^d
\exp\left(
-\frac{1}{d}
\left(
\frac d2-R
\right)^2
\right)
$$
因此：
$$
\mathbb{E}
\frac{|H^{-1}(H(x))|}{2^d}
\le
q+
\exp\left(
-\frac{1}{d}
\left(
\frac d2-R
\right)^2
\right)
$$
记：
$$
\beta
=
q+
\exp\left(
-\frac{1}{d}
\left(
\frac d2-R
\right)^2
\right)
$$
则推论可以简写为：
$$
\mathbb{E}
\frac{|H^{-1}(H(x))|}{2^d}
\le
\beta
$$
其中 $$\beta$$ 可以理解为：

> 随机点所在哈希桶的平均密度上界。

## 7. 随机游走引理

设：
$$
\emptyset\ne B\subseteq {0,1}^d
$$
先从 $$B$$ 中均匀随机选择一个点 $$z$$，然后从 $$z$$ 开始执行 $$r$$ 步随机游走，最终得到随机点：
$$
Q_B
$$
随机游走引理说明：
$$
\Pr[Q_B\in B]
\le
\left(
\frac{|B|}{2^d}
\right)^{
\frac{e^{2r/d}-1}{e^{2r/d}+1}
}
$$
记：
$$
\alpha
=
\frac{e^{2r/d}-1}{e^{2r/d}+1}
$$

则：
$$
\Pr[Q_B\in B]
\le
\left(
\frac{|B|}{2^d}
\right)^\alpha
$$
直观含义：

> 如果集合 $$B$$ 很小，那么从 $$B$$ 中随机出发，走 $$r$$ 步后仍然落在 $$B$$ 中的概率也不会太大。

这正好可以用于哈希桶。

如果把 $$B$$ 看成一个哈希桶，那么这个引理告诉我们：
$$
\text{桶越小}
\Rightarrow
\text{随机游走后还留在桶里的概率越小}
$$

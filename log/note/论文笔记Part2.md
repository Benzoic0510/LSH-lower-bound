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

## 8. 随机游走引理为什么成立

这一部分使用了 Hamming 立方体上的 Fourier 分析。

主要工具包括：

1. Walsh 函数；
2. Fourier 展开；
3. Parseval 恒等式；
4. Bonami-Beckner 不等式；
5. 随机游走转移矩阵的特征值分析。

证明核心逻辑如下。

对集合 $$B$$ 的示性函数：

$$
1_B
$$

进行 Fourier 展开：

$$
1_B
=
\sum_{S\subseteq {1,\dots,d}}
\widehat{1_B}(S)W_S
$$

随机游走转移矩阵 $$P$$ 作用在 Walsh 函数上时，有：

$$
PW_S
=
\left(
1-\frac{2|S|}{d}
\right)W_S
$$
所以 $$W_S$$ 是随机游走矩阵 $$P$$ 的特征向量，特征值为：
$$
1-\frac{2|S|}{d}
$$
经过 $$r$$ 步随机游走后，对应特征值变成：
$$
\left(
1-\frac{2|S|}{d}
\right)^r
$$
于是随机游走从 $$B$$ 出发后仍落在 $$B$$ 中的概率，可以写成 Fourier 系数的加权和。

最后利用 Bonami-Beckner 不等式控制这些 Fourier 系数，得到：
$$
\Pr[Q_B\in B]
\le
\left(
\frac{|B|}{2^d}
\right)^{
\frac{e^{2r/d}-1}{e^{2r/d}+1}
}
$$
## 9. 为什么要求 $$r$$ 是奇整数

在随机游走引理中，作者要求：
$$
r\text{ 是奇整数}
$$
原因出现在特征值项：
$$
\left(
1-\frac{2|S|}{d}
\right)^r
$$
当：
$$
|S|>\frac d2
$$
时，
$$
1-\frac{2|S|}{d}<0
$$
如果 $$r$$ 是奇数，那么：
$$
\left(
1-\frac{2|S|}{d}
\right)^r<0
$$
这些项是负项。

在证明上界时，可以直接丢弃这些负项，因为去掉负项只会让整体变大，从而得到一个合法的上界。

所以 $$r$$ 是奇整数这个条件主要是为了方便处理 Fourier 展开中的负特征值项。

## 10. 命题 2.1 的证明

定义：

$$
\alpha
=
\frac{e^{2r/d}-1}{e^{2r/d}+1}
$$

以及：

$$
\beta
=
q+
\exp\left(
-\frac{(d/2-R)^2}{d}
\right)
$$

对任意：
$$
x\in{0,1}^d
$$
令：
$$
W_r(x)
$$
表示从 $$x$$ 出发进行 $$r$$ 步随机游走后得到的点。

由于每一步只翻转一个坐标，所以 $$r$$ 步之后：
$$
|x-W_r(x)|_1\le r
$$
由 LSH 的近点条件可知：
$$
\Pr[H(W_r(x))=H(x)]\ge p
$$
对所有 $$x$$ 取平均：
$$
p
\le
\mathbb{E}_x
\Pr[H(W_r(x))=H(x)]
$$
接下来按哈希桶分解。设哈希值为 $$k$$ 的桶是：

$$
H^{-1}(k)
$$
若：
$$
x\in H^{-1}(k)
$$
那么事件：
$$
H(W_r(x))=H(x)
$$
等价于：
$$
W_r(x)\in H^{-1}(k)
$$
即随机游走后仍然留在原桶中。

于是可以对每一个桶使用随机游走引理：
$$
\Pr[Q_{H^{-1}(k)}\in H^{-1}(k)]
\le
\left(
\frac{|H^{-1}(k)|}{2^d}
\right)^\alpha
$$
因此：
$$
p
\le
\mathbb{E}_H
\mathbb{E}_x
\left(
\frac{|H^{-1}(H(x))|}{2^d}
\right)^\alpha
$$
由于：
$$
0<\alpha<1
$$
函数：
$$
t^\alpha
$$
是凹函数。

使用 Jensen 不等式：
$$
\mathbb{E}_H
\left(
\frac{|H^{-1}(H(x))|}{2^d}
\right)^\alpha
\le
\left(
\mathbb{E}_H
\frac{|H^{-1}(H(x))|}{2^d}
\right)^\alpha
$$
再由推论 2.3：
$$
\mathbb{E}_H
\frac{|H^{-1}(H(x))|}{2^d}
\le
\beta
$$
所以：
$$
p
\le
\beta^\alpha
$$
即：
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
这正是命题 2.1。

## 11. 从命题 2.1 推出 $$\ell_1$$ 情形

命题 2.1 给出：
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
为了得到主定理，作者选择：
$$
R\approx \frac d2-\sqrt{d\log d}
$$
于是：
$$
\frac d2-R
\approx
\sqrt{d\log d}
$$
所以：

$$
\exp\left( -\frac{1}{d} \left( \frac d2-R \right)^2 \right) \approx \exp(-\log d)
=
\frac 1d
$$

当：
$$
d\to\infty
$$
时，
$$
\frac 1d\to 0
$$
因此：
$$
q+
\exp\left(
-\frac{1}{d}
\left(
\frac d2-R
\right)^2
\right)
\to q
$$
又选择：
$$
r\approx \frac{R}{c}
$$
由于：
$$
R\approx \frac d2
$$
所以：
$$
\frac{2r}{d}
\approx
\frac{1}{c}
$$
于是：
$$
\frac{e^{2r/d}-1}{e^{2r/d}+1}
\to
\frac{e^{1/c}-1}{e^{1/c}+1}
$$
命题 2.1 在极限下变成：
$$
p
\le
q^{
\frac{e^{1/c}-1}{e^{1/c}+1}
}
$$
两边取对数：
$$
\log p
\le
\frac{e^{1/c}-1}{e^{1/c}+1}\log q
$$
因为 $$0<p,q<1$$，所以：
$$
\log p<0,\quad \log q<0
$$
等价地写成：
$$
\log(1/p)
\ge
\frac{e^{1/c}-1}{e^{1/c}+1}
\log(1/q)
$$
因此：
$$
\frac{\log(1/p)}{\log(1/q)}
\ge
\frac{e^{1/c}-1}{e^{1/c}+1}
$$
也就是：
$$
\rho_1(c)
\ge
\frac{e^{1/c}-1}{e^{1/c}+1}
$$
这证明了 $$\ell_1$$ 情形。

## 12. 从 $$\ell_1$$ 推广到 $$\ell_s$$

对于：
$$
x,y\in{0,1}^d
$$
如果它们有 $$m$$ 个坐标不同，那么：

$$
|x-y|_1=m
$$

而在 $$\ell_s$$ 距离下：

$$
m^{1/s}
$$
所以：
$$
|x-y|_1^{1/s}
$$

等价地：
$$
|x-y|_s^s
$$
因此，在 $$\ell_s$$ 中距离放大 $$c$$ 倍，对应到 Hamming 距离中就是放大：
$$
c^s
$$
所以把 $$\ell_1$$ 情形中的 $$c$$ 换成 $$c^s$$，得到：
$$
\rho_s(c)
\ge
\frac{e^{1/c^s}-1}{e^{1/c^s}+1}
$$
这就是主定理。

## 13. 本节证明链条总结

整节证明可以概括为以下逻辑链：
$$
\text{LSH 远点条件}
\\
\Downarrow
\\
\text{哈希桶平均密度很小}
\\
\Downarrow
\\
\text{随机游走后仍留在桶内的概率很小}
\\
\Downarrow
\\
\text{但 LSH 近点条件要求该概率至少为}p
\\
\Downarrow
\\
p\text{ 不能太大}
\\
\Downarrow
\\
\frac{\log(1/p)}{\log(1/q)}
\text{ 不能太小}
\\
\Downarrow
\\
\rho_s(c)
\ge
\frac{e^{1/c^s}-1}{e^{1/c^s}+1}
$$
## 14. 核心公式汇总

**哈希桶期望大小**
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

**归一化桶密度上界**
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

**随机游走引理**
$$
\Pr[Q_B\in B]
\le
\left(
\frac{|B|}{2^d}
\right)^{
\frac{e^{2r/d}-1}{e^{2r/d}+1}
}
$$

**命题 2.1**
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

**$$\ell_1$$ 下界**
$$
\rho_1(c)
\ge
\frac{e^{1/c}-1}{e^{1/c}+1}
$$

**$$\ell_s$$下界**
$$
\rho_s(c)
\ge
\frac{e^{1/c^s}-1}{e^{1/c^s}+1}
$$

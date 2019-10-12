title: dp
speaker: Smeow
prismTheme: solarizedlight
plugins:

    - echarts
    - katex
    - mermaid
<slide class="bg-black-blue aligncenter" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">

# dp {.text-landing.text-shadow}

By Smeow {.text-intro}

[:fa-github: Github](https://github.com/ksky521/nodeppt){.button.ghost}

<slide>

# BM算法体系：

<slide>

### 壹.主要思路

$1.$我们根据矩阵的特征多项式和凯莱-哈密尔顿定理，可以证明任意矩阵乘法都可以转化成线性递推。

$2.$如果我们知道矩阵的特征值和特征向量（各有$n$个，通常无法知道特征值，而一个特征值可以推出一个特征向量）那么我们可以实现：

矩阵对角化。（这与$BM$算法无关，我们不做研究）

求矩阵对应的线性递推的通项公式。

$3.$ 根据特征向量+特征值可得通项公式的固定形式我们可以知道线性递推的通项公式恒是什么形式。

$4.$ 我们利用BM和多项式取模可以$\Theta(m^2+m\log m\log k)$得到递推公式有$m$项的线性递推数列的第$k$项是什么。

<slide>

### 贰."特征"家族：

$1.$**特征值和特征向量**定义：设$A$是一个矩阵，$\lambda$是一个数，如果对于一个非$0$向量$x$有$Ax=\lambda x$那么$\lambda$为$A$的特征值，$x$是$A$的特征向量。

$2.$$|\lambda I-A|=0$。

证明：
$$
\lambda x=Ax\\
\lambda x-Ax=0\\
(\lambda I-A)x=0\\
由于x是非零向量所以|\lambda I-A|=0
$$
感性理解一下，把$(\lambda I-A)x$看作是把矩阵$\lambda I-A$的第$i$个行向量乘上$x_i$再加和，答案$=0$说明这些行向量线性相关（因为$x$不为$0$），所以$\lambda I-A$不满秩，所以$|\lambda I-A|=0$。

$3. $由于$|\lambda I-A|$是个关于$\lambda$的$n$次多项式$f(\lambda)$（行列式展开可证），所以$\lambda$有$n$种取值，$x$也有$n$个。所以$A$有$n$个特征值，$n$个特征向量。

$4.$**特征多项式：** 上述$f(\lambda)=|\lambda I-A|$就是矩阵$A$的特征多项式。

特征多项式与行列式的关系：$|A|=(-1)^nf(0)$（$f(0)$指常数项）

<slide>

### 叁.凯莱-哈密尔顿定理

$1.$内容：将上式中$f(\lambda)$展开成多项式，再把数字$\lambda$改成矩阵$A$，那么得到的矩阵每一位取值都为$0$。即$f(A)=0$

$2.$应用：每个由矩阵乘法解决的问题都有线性递推式。

假设我们用矩阵乘法求的东西的第$i$项是$A^iM$

根据凯莱-哈密尔顿定理，$\sum_{i=0}^{k}c_iA^i=0$，且$c_k=1$（对角线上才有$\lambda$，且系数都是1啊）

变式得：

对于任意$n$

$\sum_{i=0}^kc_iA^{i+n}=0$

$\sum_{i=0}^kc_iA^{i+n}M=0$

$\sum_{i=0}^kc_i(A^{i+n}M)=0$

设$a_p$是$A^pM$的一行或者一个位置，我们有：

$\sum_{i=0}^{k}c_ia_{n+i}=0$

翻转$c$，同时令$N=n+k$可得：

$\sum_{i=0}^{k}c_{k-i}a_{n+i}=0$（现在$c_0=1$了）

$\sum_{i=0}^{k}c_{i}a_{N-i}=0$

于是得到了一个线性递推数列

现在只要我们知道特征多项式、特征值、特征向量就可以把矩阵乘法转化成线性递推

<slide>

### BM算法

$1.$引入：

由上述多个定理可得，对于任意由矩阵乘法得到的递推式，一定有对应的常系数递推方程。

通常情况下我们得不到特征多项式，但我们仍然可以快速的求线性递推。

事实上，任何使$f(A)=0$的多项式都对应着一个线性递推，而使$f(A)=0$的次数最低的多项式我们叫做最短多项式。最短多项式不一定是特征多项式，但是把矩阵每一行随便乘一个不同的东西，大概率最短多项式就是特征多项式。

$2.$ 假设我们已经通过$BM$算法求得最短多项式$f(x)$：

$A^n$是我们所求的

设$x^k=f(x)g(x)+h(x)$

将$A$带入后，$A^k=f(A)g(A)+h(A)$。由于$f(A)=0$，$A^k=h(A)$。

所以我们要求$h(x)=x^k\mod f(x)$

在此之后，事实上$h$中$x^k$代表的是$A^k$的相应的位置，也就是$a_k$，这样把$x^i$都换成$a_i$再乘前面的系数加起来即为所求。

$3.$ **Berlekamp-Massey：**

用于给定递推数列求它的最短递推式（最小多项式）:

即，求$r_0,r_1,r_2\cdots r_m$，满足

$$\sum_{i=0}^ka_{n-i}r_{i}=0\quad (n\ge k)$$

做法:

**根据习惯，本算法的实现将会在下次复习的时候揭晓**

$4.$**扩展：**

![1567578140301](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1567578140301.png)

### ![1567579301458](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1567579301458.png)

![1567579652553](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1567579652553.png)

![](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1567579732527.png)

<slide>

### 附1.矩阵对角化：

设$x_i$是一个特征向量（列向量）。

$$D=\left[\begin{matrix}\lambda_1&0&0&0&\cdots&0\\0&\lambda_2&0&0&\cdots&0\\0&0&\lambda_3&0&\cdots&0\\0&0&0&\lambda_4&\cdots&0\\\vdots&\vdots&\vdots&\vdots&\ddots&\vdots\\0&0&0&0&\cdots&\lambda_n\end{matrix}\right]$$

$$X=\left[\begin{matrix}x_1&x_2&x_3&x_4&\cdots&x_n\end{matrix}\right]$$

由$\forall i,Ax_i=\lambda_i x_i$可得：

$$AX=XD$$

所以

$$XAX^{-1}=D$$

$$A=XDX^{-1}$$

所以

$$A^k=XDX^{-1}XDX^{-1}\cdots=XD^{k}X^{-1}$$

由于$D$是对角矩阵，所以乘法复杂度是$\Theta(n)$

这是矩阵对角化，作用：对于特殊的矩阵，加快矩阵快速幂。

例题：CF923E

### 附2.已知特征多项式求通项：

由于作者很懒，而且还是附2章，所以我们仅针对一次齐次式（注意，没有常数项）来讨论。

我们可以对一个线性递推求出它的特征多项式。

由特征多项式为$\sum_{i=0}^{k}c_ix^i=0$的矩阵的线性递推是$\sum_{i=0}^{k}c_ia_{n-i}=0$可得：

线性递推$\sum_{i=0}^{k}c_ia_{n-i}=0$的特征多项式是$\sum_{i=0}^{k}c_ix^i=0$（废话）

这有什么用呢？

我们可以求一个递推数列的通项：

假设这个数列的特征多项式是$\sum_{i=0}^{k}c_ix^i$

我们直接求解$x$的$k$个根$x_1\cdots x_n$：

以下$d$是未知常数，需要带入$a_0\cdots a_{k-1}$来解出。

$1.$若$x_i$是**实数**：$a_n+=d_ix_i^n$

$2.$若$x_i,x_j$是**复数**（一定是共轭的两个）：

设$\alpha_{1}=\rho(\cos\theta+i\sin\theta),\alpha_{2}=\overline{\alpha_{1}}=\rho(\cos \theta-i \sin \theta)$

那么我们有：
$$
a_n+=\left(\begin{array}{l}{d_{i} \alpha_{1}^{n}+d_{j} \alpha_{2}^{n}} \\ {=d_{1} \rho^{n}(\cos \theta+i \sin \theta)^{n}+d_{2} \rho^{n}(\cos \theta-i \sin \theta)^{n}} \\ {=d_{1} \rho^{n}(\cos n \theta+i \sin n \theta)+d_{2} \rho^{n}(\cos n \theta-i \sin n \theta)} \\ {=\left(d_{1}+d_{2}\right) \rho^{n} \cos n \theta+i\left(d_{1}-d_{2}\right) \rho^{n} \sin n \theta} \\ {=A \rho^{n} \cos n \theta+B \rho^{n} \sin n \theta}\end{array}\right)
$$
其中$A=d_{1}+d_{2} B=i\left(d_{1}-d_{2}\right)$

$3.$若$x_i\cdots x_{i+m-1}$是出现了$m$次的重根：

$a_n+=(d_i+d_{i+1}n+d_{i+2}n^2+\cdots+d_{i+m-1}n^{m-1})x_i^n$

最后我们就有通项了！！
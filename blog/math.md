## 线性代数

#### 矩阵乘法

$$
\begin{pmatrix}
a_{1, 1} & a_{1, 2} & \cdots & a_{1, k} \\
a_{2, 1} & \ddots   &        & \vdots   \\
\vdots   &          & \ddots & \vdots   \\
a_{n, 1} & \cdots   & \cdots & a_{n, k} 
\end{pmatrix}
*
\begin{pmatrix}
b_{1, 1} & b_{1, 2} & \cdots & b_{1, m} \\
b_{2, 1} & \ddots   &        & \vdots   \\
\vdots   &          & \ddots & \vdots   \\
b_{k, 1} & \cdots   & \cdots & b_{k, m} 
\end{pmatrix}
=
\begin{pmatrix}
\sum_{i=1}^{k}a_{1, i}*b_{i, 1}&\sum_{i=1}^{k}a_{1, i}*b_{i, 2}&\cdots&\sum_{i=1}^{k}a_{1, i}*b_{i, m}\\
\sum_{i=1}^{k}a_{2, i}*b_{i, 1}&\ddots& &\vdots\\
\vdots& &\ddots&\vdots\\
\sum_{i=1}^{k}a_{n, i}*b_{i, 1} & \cdots   & \cdots & \sum_{i=1}^{k}a_{n, i}*b_{i, m} 
\end{pmatrix}
$$

#### 理解1

从定义上理解：

$n*k$ 的矩阵 $A$ 与 $k*m$ 的矩阵 $B$ 相乘得到 $n*m$ 的矩阵 $C$ ，即
$$
A * B = C
$$
其中 $C$ 的第 $i$ 行 第 $j$ 列的值为 $A$ 的第 $i$ 行 与 $B$ 的第 $j$ 列对应位置相乘积的和，即
$$
C_{i, j} = \sum_{p=1}^{k}A_{i,p}*B_{p,j}
$$

#### 理解2

把矩阵 $A$ 与矩阵 $C$ 看作若干个 **$n$ 维列向量**，矩阵 $B$ 看作**系数矩阵** 

示意如下
$$
\left(\begin{array}{c:c:c:cc}
a_{1, 1} & a_{1, 2} & a_{1, 3} & \cdots & a_{1, k} \\
a_{2, 1} & a_{2, 2} & a_{2, 3} &        & a_{2, k} \\
\vdots   & \vdots   & \vdots   & \ddots & \vdots   \\
a_{n, 1} & a_{n, 2} & a_{n, 3} & \cdots & a_{n, k} 
\end{array}\right)
*
\left(\begin{array}{ccccc}
b_{1, 1} & b_{1, 2} & b_{1, 3} & \cdots & b_{1, m} \\
b_{2, 1} & b_{2, 2} & b_{2, 3} &        & b_{2, m} \\
\vdots   & \vdots   & \vdots   & \ddots & \vdots   \\
b_{k, 1} & b_{k, 2} & b_{k, 3} & \cdots & b_{k, m} 
\end{array}\right)
=
\left(\begin{array}{c:c:c:cc}
c_{1, 1} & c_{1, 2} & c_{1, 3} & \cdots & c_{1, m} \\
c_{2, 1} & c_{2, 2} & c_{2, 3} &        & c_{2, m} \\
\vdots   & \vdots   & \vdots   & \ddots & \vdots   \\
c_{n, 1} & c_{n, 2} & c_{n, 3} & \cdots & c_{n, m} 
\end{array}\right)
$$
设 $A$ 的列向量从左到右依次为 $A_1, A_2, A_3,...,A_k$ ，$C$ 的列向量从左到右依次为 $C_1, C_2, C_3, ..., C_m$ ，则有
$$
C_i = \sum_{j=1}^{k}b_{j, i}A_j
$$

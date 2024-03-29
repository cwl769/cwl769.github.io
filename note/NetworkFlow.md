## 网络流

### 基本概念

#### 流网络

​    有源点 $s$ 和汇点 $t$ 的有向图 $G=(V,E)$，边 $(u,v)$ 存在容量 $c(u, v)$，可认为不存在的边容量为 $0$。有向图可以存在环。假定不存在反向边，即 $(u,v)\in E\rightarrow (v,u)\notin E$，但存在也可以转化为没有反向边，因为可以把反向边劈成两个并在中间插入一个虚点。

#### 可行流

​    对于流网络 $G=(V,E)$，可行流是一个函数 $f:E\rightarrow \mathbb{R}$，$f(u,v)$ 称为边 $(u,v)$ 的流量。$f$ 需同时满足下列条件：

1. **容量限制** $0\leq f(u, v) \leq c(u,v)$。
2. **流量守恒** $\forall x\in V-\{s,t\},\sum_{(u,x)\in E}f(u,x) = \sum_{(x,v)\in E}f(x,v)$。

​    定义可行流的流量 $|f| = \sum_{(s,v)\in E}f(s,v) = \sum_{(u,t)}f(u,t)$。**最大流指最大可行流** $|f|_{max}$。

#### 残量网络

​    对于流网络 $G=(V,E)$ 的可行流 $f$，其残量网络记作 $G_f = (V_f, E_f)$，其中 $V_f=V,E_f=E \cup E\text{中的反向边}$。$G_f$ 各边容量定义如下：
$$
c'(u,v) = 
\begin{cases}
c(u,v) - f(u,v) & (u,v)\in E\\
-f(v,u) & (v,u)\in E
\end{cases}
$$
​    注意在残量网络中才考虑反向边。

​    残量网络也存在可行流 $f'$。定义 $f+f'$ 是这样的运算：$\forall (u,v)\in E,(f+f')(u,v)=f(u,v)+f'(u,v)-f'(v,u)$。那么根据定义易证 $f+f'$ 也是 $G$ 的可行流。$|f+f'|=|f|+|f'|$。

#### 增广路径

​    残量网络中从 $s$ 到 $t$ 的一条路径 $p$，满足对于 $p$ 上的任意边 $(u,v)$，$c'(u,v)>0$。增广路径一般指简单路径。存在增广路径表示对应的流 $f$ 不是最大流。

#### 割

​    割 $(S,T)$ 是指对 $V$ 的一个划分，即这样两个集合 $S,T$，满足 $S\cup T=V,S\cap T=\emptyset,s\in S,t\in T$。

#### 割的容量

​    对于一个割定义割的容量 $c(S,T)=\sum_{u\in S}\sum_{v\in T}c(u,v)$。

​    **最小割指割的容量最小**

#### 割的流量

​    对于一个割 $(S,T)$ 和一个可行流 $f$，定义割的流量 $f(S,T)=\sum_{u\in S}\sum_{v\in T}f(u,v) - \sum_{u\in T}\sum_{v\in S}f(u,v)$。

​    $\text{结论1: }\forall \text{割}(S,T),\forall f, f(S,T)\leq c(S,T)$

证明：
$$
\begin{aligned}
\because&& f(u,v)\leq c(u,v)\\
\therefore&& c(u,v)-f(u,v)\geq0\\
\therefore&& \sum_{u\in S}\sum_{v\in T}(c(u,v)-f(u,v))\geq 0\\
\therefore&& \sum_{u\in S}\sum_{v\in T}f(u,v)\leq\sum_{u\in S}\sum_{v\in T}c(u,v)\\
\text{又}\because&& f(u,v)\geq0\\
\therefore&& \sum_{u\in T}\sum_{v\in S}f(u,v)\geq0\\
\therefore&& \sum_{u\in S}\sum_{v\in T}f(u,v)-\sum_{u\in T}\sum_{v\in S}f(u,v)\leq\sum_{u\in S}\sum_{v\in T}c(u,v)\\
\text{即}&&f(S,T)\leq c(S,T)
\end{aligned}
$$

​    $\text{结论2: }\forall \text{割}(S,T),\forall f, f(S,T)=|f|$

证明：

首先定义关于任意两个 $V$ 的子集 $X,Y$ 的流量
$$
f(X,Y)=\sum_{u\in X}\sum_{v\in Y}f(u,v) - \sum_{u\in Y}\sum_{v\in X}f(u,v)
$$
由定义得出如下性质：
$$
f(X,X) = 0\\
f(X,Y) = -f(Y,X)\\
f(X\cup Y, Z) = f(X, Z) + f(Y, Z)\ (X\cap Y=\emptyset)\\
f(Z, X\cup Y) = f(Z,X) + f(Z,Y)\ (X\cap Y=\emptyset)\\
$$
于是有
$$
f(S,V) = f(S,T) + f(S, S)\\
f(S,T) = f(S,V) = f(\{s\}, V) + f(S-\{s\}, V)\\
$$
设 $P = S - \{s\}$，则
$$
f(P,V) = \sum_{u\in P}\sum_{v\in V}f(u,v) - \sum_{v\in V}\sum_{u\in P}f(v,u)\\
= \sum_{u\in P}\Big(\sum_{v\in V}f(u,v)-\sum_{v\in V}f(v,u)\Big)
$$
又由流量守恒知
$$
f(P,V) = 0
$$
所以
$$
f(S,T) = f(\{s\},V) + f(P,V) = f(\{s\},V) = |f|
$$
Q.E.D.

#### 最大流最小割定理

以下三个命题等价：

1. 可行流 $f$ 是最大流
2. 可行流 $f$ 的残量网络中不存在增广路
3. 存在割 $(S,T)$，$|f|=c(S,T)$

证明思路 $1\rightarrow2,2\rightarrow3,3\rightarrow1$，从而能证明三个命题等价。

$1\rightarrow2$：

​    假设存在增广路，那么存在可行流 $f + f'$，$|f+f'|>|f|$。与命题 1 矛盾。得证。

$2\rightarrow3$：

​    在残量网络 $G_f$ 中，从 $s$ 出发沿容量大于 $0$ 的边所能到达的所有点记作 $S$。令 $T = V - S$，由于不存在增广路，所以 $t \notin S, t\in T$，于是构造出一个割 $(S,T)$。

​    在上述割中，有 $\forall u\in S,\forall v\in T,f(u,v)=c(u,v)$，这是由于 $c'(u,v) = 0$。故 $c(S,T) = f(S,T) = |f|$。

$3\rightarrow1$：

​    设最大流流量为 $|f_m|$，则 $|f_m|\leq c(S,T) = |f|$，又由于最大性可知 $|f|\leq |f_m|$，故 $|f| = |f_m|$，即 $f$ 是最大流。

### 算法

#### FF方法

是最大流算法的基础方法，即：

1. 找到一条增广路
2. 用找到的增广路更新残量网络

### 应用

#### 最大权闭合子图

##### 闭合子图

​    是有向图 $G = (V, E)$ 上的一个点集 $V'$，满足 $\forall (u, v) \in E, u\in V'\rightarrow v \in V'$。

##### 简单割

​    满足割边要么从 $s$ 出发，要么指向 $t$ 的割。

---

构造一个网络 $N = (V_N,E_N)$，使 $V_N = V \cup \{s,t\}, E_N = E \cup\{(s, v)|w(v)>0\} \cup \{(u,t)|w(u)<0\}$。
$$
c(u,v) = 
\begin{cases}
+\infty &,(u,v)\in E\\
w(v) &,u = s\\
-w(u) &,v = t
\end{cases}
$$


设 $V_1$ 是 $G$ 的一个闭合子图，$V_2 = V - V_1$，则它一定与一个简单割 $(S,T)$ 对应，其中 $S = \{s\} \cup V_1,T = V_N-S$。
$$
\begin{aligned}
c(S,T) &= c (\{s\}, V_2) + c(V_1, \{t\})\\
&=\sum_{x\in V_2 \and w(x)>0}w(x) + \sum_{x\in V_1 \and w(x)<0}-w(x)\\
w(V_1) &= \sum_{x\in V_1}w(x)\\
&= \sum_{x\in V_1\and w(x)>0}w(x) + \sum_{x\in V_1\and w(x)<0}w(x)\\
\end{aligned}
$$
两式相加得 $c(S,T) + w(V_1) = \displaystyle\sum\limits_{x\in V \and w(x)>0}w(x)$。

等式右侧为一定值，要求最大权 $w_{max}(V_1)$，即求最小割。

#### 最大密度子图

​	这里我们规定无向图 $G=(V,E)$ 的子图 $G'=(V', E')$ 需要满足要求 $(u, v)\in E'\rightarrow u\in V'\and v\in V'$。$G'$ 的密度 $\rho = \frac{|M'|}{|V'|}$。

​	按照分数规划的一般做法，我们二分一个值 $g = \frac{|M'|}{|N'|}$。移项后得到 $|M'| - g|N'| = 0$。判断 $g$ 是否是可行解，我们需要求出 $|M'|-g|N'|$ 的最大值，也即求 $g|N'| - |M'|$ 的最小值。

​	令 $d_x$ 表示点 $x$ 的度，式子转化后可得
$$
\begin{aligned}
g|N'| - |M'| &= \sum_{x\in V'}g - \frac{\sum_{x\in V'}d_x - c(V',\overline{V'})}{2}\\
&= \sum_{x\in V'}(g - \frac{d_x}{2}) + \frac{c(V', \overline{V'})}{2}
\end{aligned}
$$
​	由于我们只需要知道上式和 $0$ 的大小关系，不妨将上式乘 $2$ 后记为 $X$ 得到
$$
X = \sum_{x\in V'}(2g - d_x) + c(V', \overline{V'})
$$
​	新建一个源点 $s$ 和一个汇点 $t$，从 $s$ 向所有点连容量为 $U$ 的边，从所有点向 $t$ 连容量为 $U + 2g - d_x$ 的边。原图中的无向边拆成两条容量为 $1$ 的边。设 $S = \{s\}\cup V', T = \{t\}\cup\overline{V'}$，有
$$
\begin{aligned}
c(S,T) &= c(\{s\}, \overline{V'})+c(V', \{t\}) + c(V', \overline{V'})\\
&= U|\overline{V'}| + U|V'| + \sum_{x\in V'}(2g-d_x) + c(V', \overline{V'})\\
&= U|V| + X
\end{aligned}
$$
​	于是 $X$ 的最小值即在最小割时取到。

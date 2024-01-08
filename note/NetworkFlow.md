## 网络流

### 基本概念

#### 流网络

​	有源点 $s$ 和汇点 $t$ 的有向图 $G=(V,E)$，边 $(u,v)$ 存在容量 $c(u, v)$，可认为不存在的边容量为 $0$。有向图可以存在环。假定不存在反向边，即 $(u,v)\in E\rightarrow (v,u)\notin E$，但存在也可以转化为没有反向边，因为可以把反向边劈成两个并在中间插入一个虚点。

#### 可行流

​	对于流网络 $G=(V,E)$，可行流是一个函数 $f$，$D_f=E$，$f(u,v)$ 称为边 $(u,v)$ 的流量。$f$ 需同时满足下列条件：

1.   **容量限制** $0\leq f(u, v) \leq c(u,v)$。
2.   **流量守恒** $\forall x\in V-\{s,t\},\sum_{(u,x)\in E}f(u,x) = \sum_{(x,v)\in E}f(x,v)$。

​	定义可行流的流量 $|f| = \sum_{(s,v)\in E}f(s,v) = \sum_{(u,t)}f(u,t)$。**最大流指最大可行流** $|f|_{max}$。

#### 残量网络

​	对于流网络 $G=(V,E)$ 的可行流 $f$，其残量网络记作 $G_f = (V_f, E_f)$，其中 $V_f=V,E_f=E \cup E\text{中的反向边}$。$G_f$ 各边容量定义如下：
$$
c'(u,v) = 
\begin{cases}
c(u,v) - f(u,v) & (u,v)\in E\\
-f(v,u) & (v,u)\in E
\end{cases}
$$
​	注意在残量网络中才考虑反向边。

​	残量网络也存在可行流 $f'$。定义 $f+f'$ 是这样的运算：$\forall (u,v)\in E,(f+f')(u,v)=f(u,v)+f'(u,v)-f'(v,u)$。那么根据定义易证 $f+f'$ 也是 $G$ 的可行流。$|f+f'|=|f|+|f'|$。

#### 增广路径

​	残量网络中从 $s$ 到 $t$ 的一条路径 $p$，满足对于 $p$ 上的任意边 $(u,v)$，$c'(u,v)>0$。增广路径一般指简单路径。存在增广路径表示对应的流 $f$ 不是最大流。



#### 割

​	割 $(S,T)$ 是指对 $V$ 的一个划分，即这样两个集合 $S,T$，满足 $S\cup T=V,S\cap T=\emptyset,s\in S,t\in T$。

#### 割的容量

​	对于一个割定义割的容量 $c(S,T)=\sum_{u\in S}\sum_{v\in T}c(u,v)$。

​	**最小割指割的容量最小**

#### 割的流量

​	对于一个割 $(S,T)$ 和一个可行流 $f$，定义割的流量 $f(S,T)=\sum_{u\in S}\sum_{v\in T}f(u,v) - \sum_{u\in T}\sum_{v\in S}f(u,v)$。



​	$\text{结论1: }\forall \text{割}(S,T),\forall f, f(S,T)\leq c(S,T)$

证明：
$$
\because&& f(u,v)\leq c(u,v)\\
\therefore&& c(u,v)-f(u,v)\geq0\\
\therefore&& \sum_{u\in S}\sum_{v\in T}(c(u,v)-f(u,v))\geq 0\\
\therefore&& \sum_{u\in S}\sum_{v\in T}f(u,v)\leq\sum_{u\in S}\sum_{v\in T}c(u,v)\\
\text{又}\because&& f(u,v)\geq0\\
\therefore&& \sum_{u\in T}\sum_{v\in S}f(u,v)\geq0\\
\therefore&& \sum_{u\in S}\sum_{v\in T}f(u,v)-\sum_{u\in T}\sum_{v\in S}f(u,v)\leq\sum_{u\in S}\sum_{v\in T}c(u,v)\\
\text{即}&&f(S,T)\leq c(S,T)
$$


​	$\text{结论2: }\forall \text{割}(S,T),\forall f, f(S,T)=|f|$

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

由上述两个结论可知 $|f|\leq c(S,T)$，即最大流不大于最小割


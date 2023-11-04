### Lucas 定理







### exLucas







### tips
当 $n$ 和 $m$ 都较小的时候，可以考虑直接用组合数公式计算。
$$
\binom{n}{m} = \frac{n!}{m!(n-m)!} \bmod p
$$
我们预处理出 $1$ 到 $n$ 之间的阶乘以及他们的逆元，可以在计算时 $O(1)$ 调用，预处理复杂度 $O(n)$。

然而，若 $m \geq p$ 或 $n-m \geq p$ 时，由于 $m!$ 或 $(n-m)!$ 不存在逆元，所以无法使用上述方法。但是，通过计算 $n!$ 、$m!$ 以及 $(n-m)!$ 质因数分解中 $p$ 的次幂，可以求得组合数在模 $p$ 意义下的结果。

设 $f(x) = \sum_{k=1}^{+\infty}\lfloor \frac{x}{p^k} \rfloor$ ，表示 $x!$ 质因数分解中 $p$ 的指数。
$$
\begin{equation}\label{equation:pissmall}
\binom{n}{m} \bmod p = 
\begin{cases}
0 && f(n)>f(m) + f(n-m) \\
\frac{n!}{p^{f(n)}} \frac{p^{f(m)}}{m!} \frac{p^{f(n-m)}}{(n-m)!} \bmod p && f(n)=f(m) + f(n-m)
\end{cases}
\end{equation}
$$
下面证明不存在 $f(n)<f(m) + f(n-m)$ 的情况：

容易证明 $\lfloor \frac{m}{t} \rfloor + \lfloor \frac{n-m}{t} \rfloor \leq \lfloor \frac{n}{t} \rfloor$ ，代入 $f(x)$ 得到 $f(n) \geq f(m) + f(n-m)$。

根据 $(\ref{equation:pissmall})$ ，我们可以采用如下方法计算组合数：

1.   预处理 $1-n$ 的阶乘及其逆元，注意忽略所有 $p$，并单独统计 $p$ 的指数。
2.   若 $f(n)=f(m)+f(n-m)$ 使用预处理的信息计算答案
3.   否则，返回 $0$。


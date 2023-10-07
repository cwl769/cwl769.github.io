### Lucas 定理







### exLucas







### tips
当 $n$ 和 $m$ 都较小的时候，可以考虑直接用组合数公式计算。
$$
\binom{n}{m} = \frac{n!}{m!(n-m)!} \bmod p
$$
我们预处理出 $1$ 到 $n$ 之间的阶乘以及他们的逆元，可以在计算时 $O(1)$ 调用，预处理复杂度 $O(n)$。

然而，若 $m \geq p$ 或 $n-m \geq p$ 时，由于 $m!$ 或 $(n-m)!$ 不存在逆元，所以无法使用上述方法。但是，通过计算 $n!$ 、$m!$ 以及 $(n-m)!$ 质因数分解中 $p$ 的次幂，可以求得组合数在模 $p$ 意义下的结果。

设 $\mathrm{f}(x) = \sum_{k=1}^{+\infty}\lfloor \frac{x}{p^k} \rfloor$ ，表示 $x!$ 质因数分解中 $p$ 的指数。
$$
\binom{n}{m} \bmod p = 
\begin{cases}
0 && \mathrm{f}(n)>\mathrm{f}(m) + \mathrm{f}(n-m) \\
\frac{n!}{p^{f(n)}} \frac{p^{f(m)}}{m!} \frac{p^{f(n-m)}}{(n-m)!} \bmod p && \mathrm{f}(n)=\mathrm{f}(m) + \mathrm{f}(n-m)
\end{cases}
$$
对于等于的情况，只需要在预处理的时候忽略 $p$ 即可。

下面证明不存在 $\mathrm{f}(n)<\mathrm{f}(m) + \mathrm{f}(n-m)$ 的情况。

容易证明 $\lfloor \frac{m}{t} \rfloor + \lfloor \frac{n-m}{t} \rfloor \leq \lfloor \frac{n}{t} \rfloor$ ，代入 $\mathrm{f}(x)$ 得到 $\mathrm{f}(n) \geq \mathrm{f}(m) + \mathrm{f}(n-m)$。
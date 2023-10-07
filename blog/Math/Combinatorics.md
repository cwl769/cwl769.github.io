$$
\begin{equation}
\binom{n}{m} = \frac{1}{m}\binom{n}{m-1}\binom{n-m+1}{1}
\end{equation}
$$

组合意义理解，先从 $n$ 个物品中选 $m-1$ 个，再选 $1$ 个，最后调整顺序

推导
$$
\begin{align*}
\frac{1}{m}\binom{n}{m-1}\binom{n-m+1}{1} &= \frac{n!(n-m+1)!}{m(m-1)!(n-m+1)!(n-m)!1!}\\
&=\frac{n!}{m(m-1)!(n-m)!}\\
&=\frac{n!}{m!(n-m)!}\\
&=\binom{n}{m}
\end{align*}
$$

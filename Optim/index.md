# Introduction to Mathematical Optimization

\toc

_click on the '&check;' sign to jump to the page_

@@flist
* \goto{/Optim/linesearch/} Line Search Descent Methods,
@@

## Newton-Raphson Algorithm
$$
x^{i+1} = x^i - \dfrac{f'(x^i)}{f''(x^i)}, i = 0, 1, 2, ...
$$

\proof{Newton-Raphson}{$$
  f'(x) \approx f'(x_0) + f''(x_0) (x - x_0) \\
  \text{when } x \rightarrow x^* , f'(x) = 0 \\
  \overset{x \rightarrow x^*}{\Longrightarrow} 0 \approx f'(x_0) + f''(x_0)(x^* - x_0) \\
  \Longrightarrow x^* \approx x_0 - \dfrac{f'(x_0)}{f''(x_0)} \\
  \Longrightarrow x^{i+1} = x^i - \dfrac{f'(x^i)}{f''(x^i)}
  $$
We iterate on $x^i$, until $x^i = x^*$ (where $x^*$ is the minimum point), $f'(x^i) = 0$, i.e. it converges to $x^*$}
# Line Search Descent Methods

\toc

\lineskip

## Unconstrained Optimization
General structure of an unconstrained line search descent method

\algo{}{
1. Initialize a starting point $\mathbf{x}^0$. Set the tolerances to $\varepsilon_1, \varepsilon_2, \varepsilon_3 > 0$. Initialize iteration count $i = 1$.
2. Select a descent direction $\mathbf{u}^i$: $$\frac{df(\mathbf{x}^i)}{d\lambda}|_{\mathbf{u}^{i+1}} = \nabla^T f(\mathbf{x}^i \mathbf{u}^{i+1}) < 0$$
3. Perform a *one-dimensional line search* in direction $\mathbf{u}^i$: $$\min_{\lambda} F(\lambda) = \min_{\lambda} f(\mathbf{x}^{i-1} + \lambda \mathbf{u}^i)$$
4. Set $\mathbf{x}^i = \mathbf{x}^{i-1} + \lambda_i \mathbf{u}^i$
5. Convergence test: $$\text{if } \|\mathbf{x}^i - \mathbf{x}^{i-1}\| < \varepsilon_1 \text{ or } \|\nabla f(\mathbf{x}^i)\| < \varepsilon_2 \text{ or } |f(\mathbf{x}^i) - f(\mathbf{x}^{i-1})| < \varepsilon_3 \\ \text{then, } \mathbf{x}^* \cong \mathbf{x}^i \\ \text{else go to Step 6}$$ 
6. Set $i = i + 1$ and repeat Step 2 till Step 5.
}


### Univariate Function Optimization
**One-dimensional line search** is an important part in Step 2. to find the best learning rate. $$F(\lambda^*) = \min_{\lambda} F(\lambda), \lambda \in \R$$


#### Smooth Functions - Interpolation Methods
**Quadratic Interpolation**

Interpolating quadratic polynomial: 
$$p_2(\lambda) = F(\lambda_0) + F[\lambda_0,\lambda_1](\lambda - \lambda_0) + F[\lambda_0,\lambda_1,\lambda_2](\lambda - \lambda_0)(\lambda - \lambda_1)$$
> **_Divided Difference:_** $$ \begin{align*} & [y_0,y_1] = \dfrac{y_1 - y_0}{x_1 - x_0} \\ & \\ [y_0,y_1,y_2] = & \dfrac{[y_1,y_2] - [y_0,y_1]}{x_2 - x_0} = \dfrac{\dfrac{y_2 - y_1}{x_2 - x_1} - \dfrac{y_1 - y_0}{x_1 - x_0}}{x_2 - x_0} \\ & \cdots \end{align*}$$

\intuition{
$$
\text{To cross } (x_0,y_0), \rightarrow y = y_0 + \phi_1 (x - x_0) \\
\overset{(x_1,y_1)}{\Longrightarrow} \phi_1 = \dfrac{y_1 - y_0}{x_1 - x_0} = [y_0,y_1]
$$
$$
\text{To cross } (x_0,y_0), (x_1,y_1), \rightarrow y = y_0 + \phi_1 (x - x_0) + \phi_2 (x - x_0)(x - x_1)
$$
$$
\overset{(x_2,y_2)}{\Longrightarrow} \begin{cases} y_2 = y_0 + [y_0,y_1] (x_2 - x_0) + \phi_2 (x_2 - x_0)(x_2 - x_1) \\  y_1 = y_0 + [y_0,y_1] (x_1 - x_0) \end{cases}
$$
$$
\Longrightarrow y_2 - y_1 = [y_0, y_1] (x_2 - x_1) + \phi_2 (x_2 - x_0)(x_2 - x_1)
$$
$$
\Longrightarrow \phi_2 = \dfrac{[y_1,y_2] - [y_0,y_1]}{x_2 - x_0} = [y_0,y_1,y_2]
$$

The turning point $\lambda_m$ of fitted polynomial curve is near the minimum madal $\lambda^*$ of ordinary curve: 
$$
\dfrac{dp_2}{d\lambda} = F[\lambda_0,\lambda_1] + 2\lambda F[\lambda_0,\lambda_1,\lambda_2] - F[\lambda_0,\lambda_1,\lambda_2] (\lambda_0 + \lambda_1) = 0\\
\lambda_m = \dfrac{F[\lambda_0,\lambda_1,\lambda_2] (\lambda_0 + \lambda_1) - F[\lambda_0,\lambda_1]}{2F[\lambda_0,\lambda_1,\lambda_2]} \cong \lambda^*
$$
}

**Powell's interpolation algorithm**:

> **_Note:_** starting point $\lambda_0$, step size $h$, tolerance $\varepsilon$, maximum step size $H$

\algo{Powell's Interpolation}{
1. Compute $F(\lambda_0), F(\lambda_0+h)$
2. $\begin{cases} F(\lambda_0) < F(\lambda_0+h) \Rightarrow F(\lambda_0 - h) \\ F(\lambda_0) > F(\lambda_0+h) \Rightarrow F(\lambda_0 + 2h)\end{cases}$ corresponding to both sides of the curve modal.
3. Compute turning point $\lambda_m = \dfrac{F[\lambda_0,\lambda_1,\lambda_2] (\lambda_0 + \lambda_1) - F[\lambda_0,\lambda_1]}{2F[\lambda_0,\lambda_1,\lambda_2]}$
4. If $\lambda_m$ is a minimum point and $|\lambda_m - \lambda_n| > H$, where $\lambda_n$ is the nearest point to $\lambda_m$, then discard the point $\textbf{furthest}$ from $\lambda_m$, and from the point with $\textbf{lowest}$ value take a step of size $H$ in the direction of descent $\rightarrow$ Step 3;<br>
if $\lambda_m$ is a maximum point, then discard the point $\textbf{nearest}$ from $\lambda_m$, and from the point with $\textbf{lowest}$ value take a step of size $H$ in the direction of descent $\rightarrow$ Step 3;<br>
else $\rightarrow$ Step 5.
5. If $|\lambda_m - \lambda_n| < \varepsilon$ then $F(\lambda^*) \cong \min[F(\lambda_m), F(\lambda_n)]$ and Stop;<br>
else $\rightarrow$ Step 6.
6. Discard point with $\textbf{highest}$ $F$ value and replace it by $\lambda_m$, then $\rightarrow$ Step 3.}


**Implementation**
\note{Codes implemented in Python/Julia/Matlab/R can be found in my [Github repository](https://github.com/7lang2yan/Mathematical-Optimization.git)}

Here I use Julia as the implementation language

```julia
function divided_diff(func::Function,x::AbstractVector)
    if length(x) == 1
        return func(x[1])
    else
        return (divided_diff(func, x[2:end]) - divided_diff(func, x[1:end-1])) / (x[end] - x[1])
    end
end

struct Optim_res
	argmin :: Real
	min :: Real
	iter :: Int
end

function _powells_(func::Function, λ0::Real = 0, h::Real = 0.01, ε::Real = 1e-4, H::Real = 2)
    λ1 = λ0 + h
    F0 = func(λ0); F1 = func(λ1)
    (F0 < F1 ? λ2 = λ0 - h : λ2 = λ0 + 2*h)
    λ = [λ0,λ1,λ2]

    i = 0

    while true
        i += 1

        λm = (λ[1] + λ[2]) / 2 - divided_diff(func,[λ[1],λ[2]]) / (2 * divided_diff(func,[λ[1],λ[2],λ[3]]))

        (func(λm) < func(λ[1]) ? minimumcheck = true : minimumcheck = false)

        λn = λ[argmin([abs(x - λm) for x in λ])]
        if minimumcheck
            if abs(λm - λn) > H
                λ[argmax([abs(x - λm) for x in λ])] = λ[argmin([func(x) for x in λ])] + H * sign(func(λ[argmin([func(x) for x in λ])]) - func(λ[argmin([func(x) for x in λ])] + 1e-3))
            elseif abs(λm - λn) < ε
                λstar = [λn,λm]
                F  = [func(λn),func(λm)]

                argMinimum = λstar[argmin(F)]
                Minimum = minimum(F)
                println("Optimization Results")
                println("-----------------------------------")
                println("-----------------------------------")
                println("Algorithm: Powell's Quadratic Interpolation")
                println("Minimum point: $argMinimum")
                println("Minimum: $Minimum")
                println("Iterations: $i")

                return Optim_res(λstar[argmin(F)],minimum(F),i)
            else
                λ[argmax([func(x) for x in λ])] = λm
            end
        else # λm is maximum point
            λ[argmin([abs(x - λm) for x in λ])] = λ[argmin([func(x) for x in λ])] + H * sign(func(λ[argmin([func(x) for x in λ])]) - func(λ[argmin([func(x) for x in λ])] + 1e-3))
        end
    end

end
```

**Example**
```julia-repl
julia> optfunc(x) = x^2 + 100 * x - 4;

julia> _powells_(optfunc)
Optimization Results
-----------------------------------
-----------------------------------
Algorithm: Powell's Quadratic Interpolation
Minimum point: -49.999999999999915
Minimum: -2504.0000000000005
Iterations: 26
Optim_res(-49.999999999999915, -2504.0000000000005, 26)

julia> _powells_(x -> 2x^2 + 3x + 1)
Optimization Results
-----------------------------------
-----------------------------------
Algorithm: Powell's Quadratic Interpolation
Minimum point: -0.7499999999999996
Minimum: -0.125
Iterations: 2
Optim_res(-0.7499999999999996, -0.125, 2)
```


#### Poorly Conditioned Functions - Bracketing Methods
Assuming $F(\lambda)$ is **unimodal** over interval $[a,b]$ and modal at $\lambda^*$.

**Basic idea**: Select $\lambda_1$ and $\lambda_2$, where $a < \lambda_1 < \lambda_2 < b$. If $F(\lambda_1) < F(\lambda_2) \Rightarrow$ new unimodal interval $[a,\lambda_2]$ (set $b=\lambda_2$), else if $F(\lambda_1) > F(\lambda_2) \Rightarrow$ new unimodal interval $[\lambda_1,b]$ (set $a=\lambda_1$). Repeat this recursively until the interval is reduced to $\lambda^*$

##### Fibonacci search method
The most economic method to choose $\lambda_1$ and $\lambda_2$

##### Golden Section method
The near optimum and straightforward method to choose $\lambda_1$ and $\lambda_2$

Golden ratio $r$: $r^2 + r -1 = 0 \Rightarrow r = \dfrac{\sqrt{5} - 1}{2} = 0.618034$

**Basic golden section algorithm**:

> **_NOTE:_**  $L_0 = b - a$ and threshold $\varepsilon$, iteration count $i = 0$

@@flist
1. Set $\lambda_1 = a + r^2L_0$ and $\lambda_2 = a + rL_0$
2. Compute $F(\lambda_1)$ and $F(\lambda_2)$, $i = i+1$
3. $$\begin{cases} 
F(\lambda_1) < F(\lambda_2) \Rightarrow b = \lambda_2, \color{green}{\lambda_2 = \lambda_1}, L_i = b-a, \lambda_1 =  a + r^2L_i \\ 
F(\lambda_1) > F(\lambda_2) \Rightarrow a = \lambda_1, \color{green}{\lambda_1 = \lambda_2}, L_i = b-a, \lambda_2 =  a + rL_i
\end{cases}$$
4. If $L_i < \varepsilon \Rightarrow \lambda^* = \dfrac{b+a}{2}$, otherwise repeat Step 2-4
@@

\note{$\color{green}{\dfrac{r^2L_0}{rL_0} = r}$}

**Implementation**
\note{Codes for Python/Julia/Matlab/R can be found in  my [Github repository](https://github.com/7lang2yan/Mathematical-Optimization.git)}

Here I use Julia as the implementation language

```julia
struct Optim_res
	argmin :: Real
	min :: Real
	iter :: Int
end

function _GoldenSection_(optfunc::Function, interval::AbstractVector, eps=1e-4)
	r = (sqrt(5) - 1) / 2
	# containers
	a, b = interval[1], interval[2]
	L = b - a
	i = 0
	λ₁, λ₂ = a + r^2 * L, a + r * L
	F₁, F₂ = optfunc(λ₁), optfunc(λ₂)

	# initialize result containers
	minimum = nothing
	λ = nothing

	# update containers
	while true
		if F₁ < F₂
			b = λ₂; λ₂ = λ₁; F₂ = F₁; L = r * L; λ₁ = a + r^2 * L; F₁ = optfunc(λ₁)
		else
			a = λ₁; λ₁ = λ₂; F₁ = F₂; L = r * L; λ₂ = a + r * L; F₂ = optfunc(λ₂)
		end

		# iterations count
		i += 1

		if L < eps
			λ = (b + a) / 2
			minimum = optfunc(λ)
			break
		end
	end

	println("Optimization Results")
	println("-----------------------------------")
	println("-----------------------------------")
	println("Algorithm: Golden Section")
	println("Minimum point: $λ")
	println("Minimum: $minimum")
	println("Iterations: $i")
	

	return Optim_res(λ,minimum,i)
end
```

**Example**
```julia-repl
julia> optfunc(x) = x^2 + 4 * x - 4;

julia> _GoldenSection_(optfunc, [-10,10], 1e-8)
Optimization Results
-----------------------------------
-----------------------------------
Algorithm: Golden Section
Minimum point: -1.9999999770027157
Minimum: -8.0
iterations: 45
Optim_res(-1.9999999770027157, -8.0, 45)

julia> _GoldenSection_(x -> 2x^2 + 3x + 1, [-10,10], 1e-8)
Optimization Results
-----------------------------------
-----------------------------------
Algorithm: Golden Section
Minimum point: -0.7499999977519514
Minimum: -0.12500000000000022
iterations: 45
Optim_res(-0.7499999977519514, -0.12500000000000022, 45)
```
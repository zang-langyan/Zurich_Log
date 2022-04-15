# 马尔可夫链蒙特卡洛方法 Markov Chain Monte Carlo Methods

\toc

\lineskip

## Metropolis Algorithm

Suppose a random variable $X$ follows a probability density (or mass) function $P$. Here, it is $\theta$ follows the posterior distribution function $p(\theta|D)$ - step 0: Initialize the random variable $\theta_{cur}$ (posterior case). (For the other cases, it could be any random variable, e.g. r.v. $X$)

\algo{Metropolis}{
- step 1: Generate a random proposed jump $\Delta \theta \sim N(\mu = 0, \sigma)$ (Note: 1. it could be other kind suitable distributions. 2. for the multivariate distributions, generate the jump from a multivariate distribution, e.g. $\Delta \bf{\theta} \sim N(\bf{\mu} = 0, \Sigma)$). $\theta_{pro} = \theta_{cur} + \Delta \theta$ 
- step 2: Computing the moving probability for the random walk: $p_{move} = \min (1, \dfrac{P(\theta_{pro})}{P(\theta_{cur})})$. For the posterior distribution, since it is proportional to the product of prior and likelihood $p(\theta|D) \propto p(D|\theta)p(\theta)$, we have $p_{move} = \min (1, \dfrac{p(D|\theta_{pro})p(\theta_{pro})}{p(D|\theta_{cur})p(\theta_{cur})})$. (Note: if the proposed r.v. $theta$ is outside of its acceptable span, then we set $p_{move}$ to $0$) 
- step 3: Check if the random walk jump is accepted: generate a random number from $U(0,1)$ uniform distribution. If it is less than $p_{move}$, then we accept the jump, otherwise, we stay. 
- step 4: Repeating step 1 to step 3, until the random walks from above generate a markov chain which is representative of our target distribution (posterior distribution)
}

Check Kruschke's book ("[**Doing Bayesian Data Analysis**](https://sites.google.com/site/doingbayesiandataanalysis/)") for the explanation of MCMC representativeness (7.5.1), accuracy (7.5.2) and efficiency (7.5.3)

### Julia/Python/Matlab/R Codes
For python/julia/matlab/r codes, check my [Github repository](https://github.com/7lang2yan/Markov-Chain-Monte-Carlo-MCMC.git). Full documentation can be found in each file.

### Examples

\warn{You must download the codes from the above repository to run the following examples!}

#### Python
```python
>>> import metropolis
>>> from scipy.special import beta
>>> density = lambda x: x**14 * (1-x)**6 / beta(15,7) # Beta(15,7) distribution density function
>>> d = metropolis.MCMC(density, space = [0,1], burnin = 5, seed = 72)
>>> result = d.metropolis(chain = 50, theta_init = 0.1)
>>> result
[0.8176960093922774, 0.6965658096789994, 0.7918980615882665, 0.7742394795862185, 0.7742394795862185, 0.6215414421599866, 0.6215414421599866, 0.6468248283946754, 0.7523307146724492, 0.7523307146724492, 0.7680822171469903, 0.6717376155310788, 0.6717376155310788, 0.8189878864825765, 0.7533257832372013, 0.7648206889254298, 0.7648206889254298, 0.7648206889254298, 0.7648206889254298, 0.7648206889254298, 0.7967947965010628, 0.707139903476412, 0.707139903476412, 0.707139903476412, 0.707139903476412, 0.7949590848197712, 0.48018105229278457, 0.48018105229278457, 0.6163978253871556, 0.6163978253871556, 0.6163978253871556, 0.6241841537823003, 0.6241841537823003, 0.6241841537823003, 0.6241841537823003, 0.6922332333961135, 0.8204027203103916, 0.7521267229207833, 0.7521267229207833, 0.808566620237602, 0.808566620237602, 0.6460288022318678, 0.5452360032045938, 0.5452360032045938, 0.5934357613729606]
```

#### Julia
```julia-repl
julia> p(θ) = Distributions.pdf(Beta(15,7),θ) # define a density function

julia> metropolis(p, 5000, 0.5; jump = Normal(0,0.2), space_min = 0, space_max = 1, burnin = 1000, rng = 42)
4000-element Vector{Float64}:
 0.7254870615709366
 0.5374079418869023
 0.5374079418869023
 0.5161750364065404
 0.7377560837277216
 0.7377560837277216
 0.8127557482035582
 ⋮
 0.7166820320569407
 0.7166820320569407
 0.7166820320569407
 0.7166820320569407
 0.7166820320569407
 0.7586353644413187

julia> @time metropolis(θ -> Distributions.pdf(Beta(15,7),θ), 10000, 0.5; jump = Normal(0,0.5), space_min = 0, space_max = 1, burnin = 1000, rng = nothing)
0.061715 seconds (103.38 k allocations: 3.814 MiB, 88.90% compilation time)
9000-element Vector{Float64}:
 0.6057362032299476
 0.6057362032299476
 0.6057362032299476
 0.6057362032299476
 0.6057362032299476
 0.6057362032299476
 0.6057362032299476
 ⋮
 0.4516105573548591
 0.4516105573548591
 0.4516105573548591
 0.5014099540247418
 0.6720304713998495
 0.6720304713998495
```

#### Matlab
```matlab
pd = makedist('Weibull','A',5,'B',2)
chain = metropolis(@(x) pdf(pd,x),5000,0.8,makedist('Normal','mu',0,'sigma',0.9),[0,Inf],1000,'shuffle');
```

#### R
```r
# Example
# posterior function 
# prior Beta(a = 1, b = 1)
# likelihood Bernoulli: N = 20, z = 14
posterior <- function(theta, a, b, N, z) {
  dbeta(theta, z + a, N - z + b)
}
a = 1
b = 1
N = 20 
z = 14
posterior_func <- function(theta) posterior(theta, a, b, N, z)

dposterior1 <- metropolis(chain = 5000, theta_cur = 0.1, dfunc = posterior_func, space_min = 0, space_max = 1, burnin = 2500)
dposterior2 <- metropolis(chain = 5000, theta_cur = 0.9, dfunc = posterior_func, space_min = 0, space_max = 1, burnin = 2500)
dposterior3 <- metropolis(chain = 5000, theta_cur = 0.5, dfunc = posterior_func, space_min = 0, space_max = 1, burnin = 2500)
```
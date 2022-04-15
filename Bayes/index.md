# 贝叶斯推断 Bayesian Inference

## Bayes's Rules

贝叶斯法则：给定参数($\theta$)条件下的数据(D)的似然函数(likelihood)概率和对于参数($\theta$)的先验(prior)知识共同构成了给定数据(D)时的参数($\theta$)的后验概率分布(posterior) 
$$
p(\theta|D) = \dfrac{p(D|\theta)p(\theta)}{p(D)} = \dfrac{p(D|\theta)p(\theta)}{\sum_{\theta^\star}p(D|\theta^\star)p(\theta^\star)}
$$
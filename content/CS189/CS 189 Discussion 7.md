

$$y_{i} = g(x_{i})+ \epsilon$$


$$\hat{h} = argmin_{h} \mathbb{E}_{g}[(h - g)^{2}]$$
$$\hat{h} = argmin_{h} \mathbb{E}_{g}[(h(X) - y)^{2}]$$
By law of large numbers,

$$
argmin_{h} = \frac{1}{n} \sum_{i=1}^{n} (h(x_{i} - y_{i})^{2}) = RSS  
$$

---


## Q1. Simple Bias-Variance Tradeoff

$Bias(\hat{X}) = \mathbb{E}[\hat{X}] - \mu$

1. $\hat{X} = \frac{1}{n} \sum_{i=1}^{n} x_{i} = \mu$
Bias = 0



c. $E[(\hat{x} - x')^{2}]$ in terms of $\sigma^2$ and the bias and variance of the estimator $\hat{X}$
$E[(\hat{x} - x')^2] = E[\hat{x}^{2} - 2\hat{x}x' + x'^{2}]$

$=E[\hat{x}^2] - 2E[\hat{x}x'] + E[x'^{2}]$

$=Var(\hat{x}) + E[x]^{2} -2E[\hat{x}x'] + E[x']^{2} + Var(x')$

$= E[\hat{x}]^{2} -2E[\hat{x}x'] + \mu^{2} + Var(\hat{x}) + \theta^{2}$



More n0 -> bias becomes larger, Variance becomes smaller

if u -> 0, add more zeros, shrink the estimate to the zero.
if data $\sigma^2$ is high, (noisy data) -> better to reduce variance, add more zeros.


What if the mean is also 0? 






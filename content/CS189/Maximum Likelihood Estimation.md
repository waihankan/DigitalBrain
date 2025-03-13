---
title: Maximum Likelihood Estimation
tags:
  - cs189
  - machine-learning
  - statistics
---
> [!info] How do we use Maximum Likelihood Estimation for Generative Models (Gaussian Discriminant Analysis, QDA, LDA)? 
> In Gaussian Discriminant Analysis, we understood the approach of generative models. Particularly, we want to find the probability distribution of the underlying classes rather than just the decision boundary. However, in real life, we do not know those probability distributions accurately. Therefore, we need some sort of estimation tool to figure out the prior probability and the probability distribution of the feature classes. 
> 
> Equivalently,
> * We model the class conditional probability distributions $P(X|Y=k)$ for each class `k` (often assuming that they're **Gaussian**).
> * Estimate the parameters of these distributions—such as the mean vectors, covariance matrices, and the class priors—from the training data (commonly using methods like **Maximum Likelihood Estimation**).


MLE is used for estimating the parameters of a statistical distribution. In GDA, we need to estimate the normal distribution (continuous) of each class and prior probability (discrete) for each class.

---
##### Coin Flipping Exercise (for prior probability)

Flip a Biased Coin with Head probability `p` and tail `1-p`. Suppose that I flip the coin 10 times, and got 8 heads and 2 tails

**Question**: what is the value of bias `p` that is the most likely to lead to this outcome.

Recall that the number of heads is [binomial distribution](https://data140.org/textbook/content/Chapter_06/01_Binomial_Distribution.html): $X \sim B(n, p)$

$$
P[X = x] = {n \choose x} p^x (1-p)^{n-x}
$$

Thus, the probability of getting 8 heads in 10 flips is 

$$
P[X = 8] = {10 \choose 8}p^8(1-p)^2
$$


$$
P[X = 8] = 45p^8(1-p)^2
$$
Let's define this expression as the "**Likelihood**". We will see why we call likelihood instead of probability when it comes to the continuous distribution case. 

Therefore, the **LIKELIHOOD FUNCTION** is:

$$
\mathcal{L}(p) = 45p^8(1-p)^2
$$

> Optimization problem. $\arg\max_{p} \; \mathcal{L(p)}$

We can solve this by finding the critical point of $\mathcal{L}$
$$
\frac{d\mathcal{L}}{dp} = 0
$$
$$
360p^7(1-p)^2 - 90p^8(1-p) = 0
$$

$$
p = 0.8
$$
Intuitively, this calculation yields what we expected, which is $0.8 = \frac{x}{n} = \frac{8}{10}$

---

#### Estimated Prior Probability

> [!important] 
> Given that we observe an event "**A**" `x` times out of `n` trials and we want to estimate the Prior Probability of event A happening, then the **estimated Prior Probability** is : 
> $$
> \hat{\pi}_{A} = \frac{x}{n}
> $$

Another Definition: Suppose our training set is n points, with x in class C. Then our estimated prior for class C is $\hat{\pi}_{C} = \frac{x}{n}$.

---

### Likelihood of a Gaussian

We have training data (sample points) $X_{1}\dots X_{n}$. We would like to find the **best-fit Gaussian** (meaning find the best $\mu$ and $\sigma^2$ for given training data.)

> [!warning] Difference between Probability and Likelihood
> In Continuous Distribution, the probability of getting **a particular point** is zero. However, in Likelihood, we will ignore this phenomenon and consider that it's not zero. 

Likelihood of Gaussian is defined as:

$$
\mathcal{L}(\mu, \sigma, X_{1}, \dots, X_{n}) = f(X_{1})f(X_{2})\dots f(X_{n})
$$

To simplify the computation, we take the log of this and called it the **Log Likelihood, $l(.)$**

$$
l(\mu, \sigma, X_{1}, \dots X_{n}) = \ln f(X_{1}) + \ln f(X_{2}) + \ln f(X_{n})
$$


Recall the PDF of [[Multivariate Gaussian Distribution]] is:

$$
f(x) = \frac{1}{(\sqrt{ 2\pi} \sigma)^d} \cdot \exp\left(-\frac{\lVert  x - \mu\rVert^2}{2\sigma^2}\right)
$$

Each $f(X_{i})$ is a **Normal Distribution**, thus,

$$
l(\mu, \sigma, X_{1}, \dots X_{n}) = \sum_{i}^n\underbrace{ \left( -\frac{\lVert X_{i} - \mu \rVert ^2}{2\sigma^{2}} - d\ln \sqrt{ 2\pi } - d\ln \sigma\right) }_{ \ln \text{of Gaussian} } 
$$


Similar to the discrete case, we can take the derivative of *log likelihood* to find the critical point (maximum).

> [!important] Estimation of $\hat{\mu}$
> $$
> \nabla _{u} l = \sum _{i=1}^n \frac{X_{i} - \mu}{\sigma^2} = 0 \implies \hat{\mu} = \frac{1}{n}\sum_{i=1}^nX_{i}
> $$
> Note that this expression is the same as the **Sample Mean.**

> [!important] Estimation of Variance $\sigma^{2}$
 > $$
> \frac{ \partial l }{ \partial \sigma }  = \sum_{i=1}^{n} \frac{\lVert X_{i} - \mu \rVert ^{2} - d\sigma^2}{\sigma^3} = 0 \implies \hat{\sigma}^2 = \frac{1}{dn}\sum_{i=1}^{n} \lVert X_{i} - \hat{\mu} \rVert ^{2}
> $$
 > $\hat{\mu}$ is used since we do not know the exact mean of the distribution. Our best is the estimated u ($\hat{\mu}$)

> [!attention] Takeaway
> Use Sample Mean and Sample Variance\* in class `C` to *estimate* the mean and variance of class `C` Gaussian.
> \* Almost Sample Variance except we're using the estimated $\hat{\mu}$.

###  Conclusion:

* **QDA**: Estimate the conditional mean, $\hat{\mu}_{C}$ and conditional variance $\hat{\sigma}^{2}_{C}$ of each class **Separately** and estimate *Prior Probabilities* $\hat{\pi}_{c}$.
* **LDA**: Estimate the conditional mean, $\hat{\mu}_{C}$ and *Prior Probabilities* $\hat{\pi}_{c}$ and **One Variance for all Class**.
* We define "one variance for all class" as: 

$$
\hat{\sigma}^{2} = \frac{1}{dn}\sum_{C}\sum_{i: i \in C}\lVert X_{i} - \mu _{C} \rVert 
$$

>[!cite] Shewchuk
>Notice that although LDA is computing one variance for all the data, each sample point contributes with respect to its own class’s mean. This gives a very different result than if you simply use the global mean! It’s usually smaller than the global variance. We say “within-class” because we use each point’s distance from its class’s mean, but “pooled” because we then pool all the classes together.

* Basically, the mean and prior probability calculation remains the same, whereas for variance, the calculation of variance is different in such a way that we use "`class mean` instead of `overall mean`" in calculating the "one" variance.
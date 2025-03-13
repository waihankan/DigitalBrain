---
title: Continuous Probability
draft: false
tags:
  - probability
---
#probability

Suppose X has a continuous probability density function PDF: f(x)

Area between [x1 and x2] under the curve of f(x) = probability of random variable X.

$$\int_{x_1}^{x_2} f(x)dx = X \in [x1, x2]$$

Integral over the entire f(x) should be 1

$$ \int_{-\infty}^{+\infty} f(x)dx = 1$$
---
#### Expected Value of g(X): 
$$\mathbb{E}\;[g(x)] = \int_{-\infty}^{+\infty}g(x)f(x)dx$$
#### Mean / Expected Value
$$\mu = \mathbb{E}\;[x] = \int_{-\infty}^{+\infty}xf(x)dx$$
#### Linearity of Expectation

> :LiBatteryWarning: Important Fact: Expected Value is a **LINEAR Function** and it holds whether the random variables are independent or not.

$$\mathbb{E}[aX + bY] = a\mathbb{E}[X] + b\mathbb{E}[X]$$

---
#### Variance 
A measure of how spread out the data points are in a set. It indicates how far each data point deviates from the mean (expected value). "Describes the dispersion of all values in the dataset."
$$\sigma^2 = \text{Variance} = \mathbb{E}\,[(X-\mu)^2] = \mathbb{E}\, [X^2] - \mu^2$$

---
title: Building Classifiers
tags:
  - machine-learning
  - cs189
---
### Preface

I will start this note with not much understanding of the models, but by the end of the note, I will revise the different models again, hopefully - with a better understanding of the concept.

### 3 Ways to Build Classifiers 

1. Generative Models (Linear Discriminant Analysis)
> Core Idea: Try to learn the **underlying probability distributions** that generate the data for **each class separately**. Similar to figuring out how "each class" creates its data points.

2. Discriminative Models (Logistic Regression)
> This model **Directly Learn the Decision Boundary** between classes or the conditional probability $P(Y|X)$ without explicitly modeling the individual class distributions. "The Focus is on Finding WHAT separates the classes from each other ."
3. Find Decision Boundary (Support Vector Machine)
> No explicit calculations of **Probabilities**. Directly find the **Optimal Decision Boundary** that separates the classes.

<small style="color:red"> ⭕ Come back to recognize the advantages and disadvantages of different models.</small>

---

### Gaussian Discriminant Analysis 

Caution: Although the name is called Gaussian "Discriminant" Analysis, please note that this model is a **generative model**. The key factor is **"How it Learns"**. The overall technique is:

* Assume Gaussian Distributions for each class.
* Estimate the parameters (mean and covariance) of these distributions.
* Use Bayes' theorem to get $P(Y | X)$, the probability of the class given features.

> First modeling the probability distribution of **each class separately.** And learns $P(X | Y)$ **The probability of observing features given a class** (Hallmark of a generative model). Model $P(X|Y) \rightarrow P(Y|X)$.


#### Fundamental Assumption: each class has a Normal Gaussian Distribution.

$$ X \sim \mathbb{N}(u, \sigma^2)$$
$$f(x) = \frac{1}{(\sqrt{ 2\pi} \sigma)^d} \cdot \exp\left(-\frac{\lVert  x - \mu\rVert^2}{2\sigma^2}\right)$$
$\mu, \sigma \; \text{and} \; \text{x are scalars and d = dimension}$ ^f1b0fd

For each class C, **SUPPOSE** we know $\mu_C$ and variance $\sigma_{C}^2$ which gives us the PDF $f_{X | Y = C}(x)$ and we know prior probability $\pi_{C} = P(Y = C)$.

<p align = "center"> 
	<img src = "Pasted image 20250217214315.png" width="100%">
</p>
🚨 In this example, we are assuming that the **variance is a scalar**, which results in *circular iso-contours* and not ellipses. This is called **isotropic normal distribution** because the variance is the same in all directions. Anisotropic Gaussians = isosurfaces are ellipsoids. Also, the **Bayes Decision boundary is an ellipse**.

From [[Decision Theory]], we know that the optimal classifier $r^*(x)$ should pick the particular class `C` such that it **maximizes the expected loss** : $f_{(X|Y = C)}(x).\pi_{Y = C}$

Since we're considering `0-1 Loss Function`, we can also recall the main principle : **Pick the class with the highest Posterior Probability**

Maximizing $Q_{c}(x) = \ln\left((\sqrt{ 2\pi })^df_{(X|Y = C)}(x).\pi_{Y = C}\right)$ is the same as maximizing $f_{(X|Y = C)}(x).\pi_{Y = C}$. The term $(\sqrt{ 2\pi })^d$ is just to cancel out the term in the Gaussian Distribution [[#^f1b0fd]]. 

Therefore, 

$$Q_{c}(x) =  -\frac{ \left\lVert  x - \mu_{C}  \right\rVert^2}{2\sigma_{C}^2} - d\ln \sigma + \ln \pi_{C}$$



---
title: Understanding Covariance Matrix for GDA
tags:
  - cs189
  - probability
  - machine-learning
---

Suppose we have two random variables -- could be column vectors or scalars.

By definition, Covariance is:
$$
Cov(R, S) = \mathbf{E}\left[(R - \mathbf{E}[R])(S - \mathbf{E}[S]^T)\right] = \mathbf{E}[RS^T] - \mu_{R}\mu_{S}^T
$$

By definition, Variance is:

$$
Var(R) = Cov (R,R)
$$


Suppose R is a vector, the Covariance Matrix of R is:

$$
Var(R) = 
\begin{bmatrix}
Var(R_{1}) & Cov(R_{1}, R_{2})  & \dots  & Cov(R_{1}, R_{d}) \\
Cov(R_{2}, R_{1}) & Var(R_{2}) &  \dots & Cov(R_{2}, R_{d}) \\
 &  &  \ddots{} \\
Cov(R_{d}, R_{1}) & Cov(R_{d}, R_{2}) & \dots & Var(R_{d})
\end{bmatrix}

$$

>[!warning]
>The Covariance matrix is symmetric, and positive semidefinite (PSD).

* If $R_{i} \text{ and } R_{j}$ are independent $\implies$ $Cov(R_{i}, R_{j}) = 0$.
* The reverse is not true.
* Thus, pairwise independent $\implies$ Var(R) is *diagonal*.
* Again, the reverse is not true.

If all features pairwise independent, then we can write the joint normal pdf as $f(x) = f(x_{1})f(x_{2})\dots f(x_{d})$. 

If Var(R) is diagonal, that means the ellipsoid (for $\Sigma^{-1}?$) are axes-aligned with squared radii on diagonal of $\Sigma$.

> [!cite] 
> So when the features are independent, you can write the multivariate Gaussian PDF as a product of univariate Gaussian PDFs. When they aren’t, you can do a change of coordinates to the eigenvector coordinate system, and write it as a product of univariate Gaussian PDFs in eigenvector coordinates. You did something very similar in Q6.2 of Homework 2.


---


---
title: Singular Value Decomposition and PCA
tags:
  - cs189
  - machine-learning
---

Problems with computing PCA using Eigenvalue Decomposition:
* Computing $X^TX$ already takes $O(nd^2)$ time.
* $X^TX$ is poorly conditioned -> numerically inaccurate eigenvalues.

Singular Value Decomposition (SVD)
* Every $X \in \mathbb{R}^{n\times d}$ has a singular value decomposition.
* $X = U\Sigma V^T = UDV^T$
* Full SVD vs Compact SVD.
* Just different ranks.


$$
\text{Full SVD : \quad  }

\begin{bmatrix}
X
\end{bmatrix}^{
n\times d} = \begin{bmatrix}
|  & | &  \dots  & |  \\
u_{1}  & u_{2} &  \dots  & u_{n} \\
|  & | &  \dots  & | 
\end{bmatrix}^{n\times n}

\begin{bmatrix}
\sigma_{1} &  0 &  0 &  0 & 0 \\
0 & \sigma_{2} &  0 & 0 & 0\\
0 & 0  & \ddots  & 0 & 0 \\
0 & 0  & 0 & \sigma_{r}  & 0\\
0 & 0  & 0  & 0 & 0\\
\end{bmatrix}^{n \times d}

\begin{bmatrix}
 & v_{1} &  \\
 & v_{2} &  \\
 & \vdots \\
 & v_{d} &  \\
\end{bmatrix}^{d \times d}
$$

For Full SVD, $U$ and $V$ are **orthogonal matrices**. (i.e have unit length and mutually orthogonal)

$U^TU = UU^T = I$
$U^{-1} = U^T$

$\sigma \geq 0$  are singular values of $X$. (By convention).
$u's$ are known as the **left singular vectors** of $X$.
$v's$ are known as the **right singular vectors** of $X$.

$$
X = \sum_{i=1}^{r}\sigma_{i}u_{i}v_{i}^T
$$


**Claim**: $v_{i}$ is an eigenvector of $X^TX$ with eigenvalues $\sigma^{2}$.

**Proof**: 
$$
\begin{align}
X^TX &= V\Sigma^TU^TU\Sigma V^T \\
&= V\Sigma^{2}V^T \quad \text{
same as eigendecomposition with 
} \Lambda = \Sigma^{2} \\

\text{Therefore eigenvalues of } X^TX \text{ is the same as } \sigma^{2}.
\end{align}
$$

So, if we want to find the singular value of X, 

* Compute $X^TX$
* Find eigenvalues of $X^TX$
* $\sigma_{i} = \sqrt{ |\lambda_{i}|}$

IMPORTANT: Row i of $U\Sigma$ gives the **principal coordinates** of sample $x_{i}$

In practice, there are fast algorithm for finding singular values of any matrix $X$. Therefore, if we want the PCA of $X$, we compute the singular value of $X$, square it to make it eigenvalues of $X^TX$.Once we have eigenvalues, we can find eigenvectors, which are **Principal Components** of $X$. **This is also the same as $v_{1}, v_{2}, \dots, v_{r}$ (rows of the matrix $V^T$) from SVD.**

---

Compact SVD

$$
X^{n \times d} = U^{n \times r}\Sigma^{r \times r} {V^{T}}^{r \times d}
$$

Note: 

* In Compact SVD, $U^TU = I \neq UU^T$.
* Similarly, $V^TV = I \neq VV^T$.
* $\Sigma$ is always full rank in compact SVD.

---

* Claim: We can find the `k` largest singular values and corresponding vectors in $O(ndk)$ time.
* There are also approximate randomized algorithms that work really well for very large dataset. 

---


[[PsuedoInverse]]










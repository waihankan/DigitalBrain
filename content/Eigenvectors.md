---
title: Eigenvectors
draft: 
tags:
  - cs189
  - linear-algebra
---

Given a square matrix A, if $Av = \lambda v$ for some vector v $\neq$ 0, then v is and eigenvector of A and $\lambda$ is the
eigenvalue of A associated with vector.

[Picture]

**Theorem** If v is eigenvector of A with eigenvalue $\lambda$, then v is eigenvector of $A^k$ with eigenvalue $\lambda^k$

Proof: $A^2v = A(Av) = A(\lambda v) = \lambda Av =\lambda^{2}v$

Theorem: If A is invertible, then v is eigenvector of $A^{-1}$ with eigenvalues $\frac{1}{\lambda}$

Proof: $A^{-1}v = A^{-1}\frac{Av}{\lambda} = \frac{1}{\lambda}v$


**Spectral Theorem**: **Every REAL, Symmetric** n x n matrix has **Real** eigenvalues and n eigenvectors that are mutually orthogonal to each other. 

If there is no multiplicity in eigenvalues, the **directions** of the eigenvectors are unique.

```ad-todo
repeated eigenvalues, multiplicity, 
ex. identity matrix
```

We can use them as a basis for $\mathbb{R}^n$


##### Building a matrix with specified eigenvectors

Choose `n` mutually orthogonal **unit** n vectors $v_{1}, \dots, v_{n}$

Let $V = [v_{1} \,  v_{2} \, \dots v_{n}] \in \mathbf{R}^{n\times n}$  then, $V^TV = VV^T = I$

This V is called **orthogonal matrix** (in math) **orthonormal matrix**. Orthonormal matrix acts like a rotation / reflection. 

Choose some eigenvalues $\lambda_{i}$:

$$\Lambda = \begin{bmatrix}
\lambda_{1} &  &  \\
 & \ddots &  \\
 &  & \lambda_{n} 
\end{bmatrix}$$
$$Av_{i} = \lambda_{i}v_{i}$$
$$AV = V\Lambda $$

$$A = V\Lambda V^T$$

**Theorem**: $$A = V\Lambda V^T = \sum_{i}^n\lambda_{i}v_{i}v_{i}^T $$
Note that $v_{i}v_{i}^T$ is a n x n matrix with rank at most 1. 

This is a matrix factorization called Eigen Decomposition. Every real symmetric matrix will have this decomposition (so called **spectral theorem**)

**Practice Exam Problem**

$$
\begin{bmatrix}
\frac{1}{\sqrt{ 2 }} & \frac{1}{\sqrt{ 2 }} \\
\frac{1}{\sqrt{ 2 }} & -\frac{1}{\sqrt{ 2 }}
\end{bmatrix}
\begin{bmatrix}
2 & 0 \\
0 & -\frac{1}{2}
\end{bmatrix}
\begin{bmatrix}
\frac{1}{\sqrt{ 2 }} & \frac{1}{\sqrt{ 2 }} \\
\frac{1}{\sqrt{ 2 }} & -\frac{1}{\sqrt{ 2 }}
\end{bmatrix}
=
\begin{bmatrix}
\frac{3}{4} & \frac{5}{4} \\
\frac{5}{4} & \frac{3}{4}
\end{bmatrix}


$$


Also Note that $$A^2 = V\Lambda^2V^T$$
Same Eigenvectors, different eigenvalues.

$$A^{-1} = V\Lambda^{-1}V^T$$
Same Eigenvectors, different eigenvalues.

---

Given a symmetric **PSD** matrix $\Sigma$, we can find a **Symmetric Square Root** $A = \Sigma^{1/2}$

* compute eigenvectors / values of $\Sigma$
* Take square root of $\Sigma$ eigenvalues
* Reassemble Matrix A
* $\Sigma^{1/2} = V\Lambda^{\frac{1}{2}}V^T$
#### Visualizing Quadratic Form

The **quadratic form of M** (M is symmetric) is $x^TM x$

[quadratic form](https://math.libretexts.org/Bookshelves/Linear_Algebra/Understanding_Linear_Algebra_(Austin)/07%3A_The_Spectral_Theorem_and_singular_value_decompositions/7.02%3A_Quadratic_forms)

picture


---


A symmetric matrix M 
* is positive definite if $w^TMw > 0$ for all w $\neq$ 0 $\iff$ all eigenvalues are **positive**.
* A symmetric matrix M is positive semi-definite if $w^TMw \geq 0$ for all w $\neq$ 0 $\iff$ all eigenvalues are **non-negative**. (Not invertible if eigenvalue 0)
* Indefinite: Saddle -> if positive eigenvalues and negative eigenvalues.
* Invertible if there is no 0 eigenvalue.

[picture]

---

> Every square matrix **has to be** Positive Semidefinite, including $A^{-2}$. If $A^{-2}$ exists, it is **Positive Definite**.

Isosurfaces. 

Singular Means Non-invertible matrix.


Isotropic = Variance is the same in all directions.

--- 
### Anisotropic Gaussians

$X \sim \mathbf{N}(\mu, \Sigma)$

PDF is 
$$\frac{1}{\sqrt{ (2\pi)^2\lvert \Sigma \rvert  }}\exp\left( -\frac{1}{2} (x-\mu)^T \Sigma^{-1}(x - \mu) \right)$$

$\lvert \Sigma \rvert \text{determinant}$

$\Sigma$ is the d x d PSD **Covariance Matrix**
$\Sigma^{-1}$ is the d x d PSD **Precision Matrix**


```
> [!tip] this is a tip
>
> This is the content of the tip
```



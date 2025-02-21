---
title: Singular Value Decomposition
tags:
  - eecs127
  - linear-algebra
  - data-analysis
---


> [!abstract] 
> Singular Value Decomposition is a way of decomposing any matrix and more powerful than eigen value decomposition or spectral decomposition.

[[Miscellaneous Facts about Matrices and Decomposition]]


In Eigenvalue Decomposition, A is diagonalized as $A = U\Lambda U^T$. There are three big problems when it comes to this decomposition. 
* The matrix must be a **square matrix**
* The matrix must be **diagonalizable**. (must have a complete set of eigenvectors) 
* The eigenvectors are **not orthogonal** (problem for spectral decomposition)

> The above conditions are always met for **Symmetric Matrices** ($A = A^T$) and **Normal Matrices** ($AA^T = A^TA$)

🔺 **Solution: Singular Vectors | Singular Value Decomposition**

---

#### Singular Value Decomposition

Suppose $A \in R^{m\times n}$

$$A = U\Sigma V^T$$
There are two sets of singular vectors. For eigenvectors, we only have one.

U = left singular vector $U \in R^{m \times m}$ $AA^T$
V = right singular vector $V \in R^{n \times n}$ = eigenvectors of $A^TA$
$\Sigma$ = diag ($\sigma_{1}, \sigma_{2}, \dots, \sigma_{r}$)

$A^TA$ is symmetric, square (n x n), **positive semi definite** (can be proven using norm property).


$$A^TA = V\Sigma V^T \in R^{n \times n}$$
$$AA^T = U\Sigma U^T \in R^{m \times m}$$

$$ A^TA = V\Sigma^TU^TU\Sigma V = V(\Sigma^T\Sigma) V$$
$$AA^T = U\Sigma V^TV\Sigma^TU^T = U(\Sigma \Sigma^T)U^T$$


>[!Attention] Attention
> * $A^TA \text{ and } AA^T$ has same eigenvalues.
> * $\sigma$ is the $\sqrt{ \lambda(A^TA \text{ or } AA^T) }$
> * $V \in \mathbf{R}^{n}$ is the eigenvector of $A^TA$. 
> * $U \in \mathbf{R}^{m}$ is the eigenvector of $AA^T$. 
> * $U_{r}U_{r}^T$ is the projection onto R(A), Column Space of A
> * $U_{m-r}U_{m-r}^T = U_{\perp}U_{\perp}^T$ is the projection onto $N(A^T) = R(A)^\perp =$ left null space of A
> * $V_{r}V_{r}^T$ is the projection onto $R(A^T) = N(A)^\perp = \text{Row Space of A}$
> * $V_{n-r}V_{n-r}^T = V_{\perp}V_{\perp}^T$  is the projection onto $N(A) = \text{{ Null Space of A}}$

`v_r` lives in the row space and null space of A.
`u_r` lives in the column space and left null space of A.

`v's` and `u's` are orthogonal since $AA^T$ and $A^TA$ are symmetric matrices. 

$$

A\begin{bmatrix}
v_{1}\dots v_{r} 
\end{bmatrix} = 
\begin{bmatrix}
u_{1} \dots u_{r}
\end{bmatrix}
\begin{bmatrix}
\sigma_{1} &  &  \\
 & \ddots &  \\
 &  & \sigma_{r}
\end{bmatrix}
$$

$$ Av_{i} = \sigma_{i}u_{i}$$
$$\text{Thus, } \quad u_{i} = \frac{Av_{i}}{\sigma_{i}} \quad \text{and these u are orthogonal}$$


A can also be written as sum of rank one matrices with $\sigma$ determining the "importance / contribution" of that rank-one matrix (dyad).

$$A = \sum_{i=1}^r \sigma_{i}u_{i}v_{i}^T$$

##### When are U and V the same? 

U and V are the same when they're equal to Q, where Q is the eigenvector matrix of A. When A is **Symmetric** and **Positive Semi Definite** , A can be decomposed using spectral theorem as $A = Q\Lambda Q^T$. Then, we can see that $\mathbf{U} = \mathbf{V}$ will be the same and the $\lambda$ is $\sigma$.

---





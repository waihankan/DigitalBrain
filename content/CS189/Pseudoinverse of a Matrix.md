---
title: Pseudoinverse of a Matrix
tags:
  - cs189
---
> [!tldr] 
> * When a matrix is **square and invertible**, you know you can compute the **inverse** $A^{-1}$. But when the matrix is not square (e.g., *tall or wide*) or *singular* (non-invertible), you can't compute a normal inverse.
> * Instead, you can compute something called a **pseudoinverse**, denoted as $A^{\dagger}$.

---

### Moore-Penrose Conditions

1. $AA^{\dagger}A = A^{\dagger}$
2. $A^{\dagger}AA^{\dagger} = A$
3. $(AA^{\dagger})^T =AA^{\dagger}$
4. $(A^{\dagger}A)^T =A^{\dagger}A$

---

* **Pseudoinverse** exists for **any** matrix, and acts like an inverse "as much as possible."
* It helps solve the least square problem even when $A$ is not *nice*. 
	* If $A$ is a $m \times n$ full column rank matrix and $m > n$, then $A^{\dagger}$ gives the least-squares solution. | overdetermined system.
	* If $A$ is a $m \times n$ full row rank matrix and $n > m$, there are infinitely many solutions; but $A^{\dagger}$ gives the minimum norm solution. | underdetermined system.
	* If A is a square matrix, $A^{\dagger} = A^{-1}$.

---
### Calculating the Pseudoinverse of a Matrix.

For a given $m \times n$ matrix $A$, the pseudo-inverse of it is defined as:
$$
\begin{align}
A &= U\Sigma V^T \\

A^{\dagger} &= V\Sigma ^{\dagger}U^T
\end{align}
$$

$\Sigma ^{\dagger}$ is obtained by taking the reciprocal of each non-zero singular value in $\Sigma$ and transpose it.

If $\Sigma^{m \times n} \to {\Sigma ^{\dagger}}^{n \times m}$

> [!danger] Warning
> If we use compact SVD, then $\Sigma ^{\dagger} = \Sigma^{-1}$.

---

> I am a little bit lazy with typing this. But here is a good summary by ChatGPT regarding the connection between Pseudoinverse and four fundamental subspaces.

![[Pasted image 20250428150237.png]]

![[Pasted image 20250428150634.png]]


> [!danger] Important
> If both $A$ and $AA^{\dagger}$ project onto the column space of $A$, how are they different?

![[Pasted image 20250428151151.png]]

![[Pasted image 20250428151232.png]]


---

* Given a Compact SVD $X = U\Sigma V^T$, 
* Null(X) = Null($V^T$) = Span($V_{o}$).
* Row(X) = Row($V^T$) = Span($V_{r}$).
* $X^{\dagger} = V\Sigma U^T$
* Null($X^{\dagger}$) = Null($U^T$) = Null($X^T$)
* Null($(X^{\dagger})^T$) = Null($V^T$) = Null($X$)
* Row($X^{\dagger}$) = Col($X$)
* Col($X^{\dagger}$) = Row($X$)
* $X, U, V, X^{\dagger}, XX^{\dagger}, X^{\dagger}X$ all have the same rank `r`.

![[Pasted image 20250428153154.png]]

Notice that this expression is the same as eigenvalue decomposition form.

Therefore, we can say that the first `r` columns of $U$ are eigenvectors of $XX^{\dagger}$ with eigenvalues `1`, and the rest are eigenvectors with eigenvalue `0`.

---



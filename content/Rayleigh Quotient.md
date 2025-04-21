

The **Rayleigh quotient** is a scalar value associated with a vector `x` and a square matrix `A`. It’s defined as:

$$
R_{A}(x) = \frac{x^TAx}{x^Tx}
$$

We can think of the **Rayleigh quotient** as a way to "measure" how much a matrix *stretches* a vector in the direction of *that* vector.

---

If $A$ is symmetric (or Hermitian in complex space), the Rayleigh quotient has a few very cool properties:

- If $x$ is an **eigenvector** of $A$, then:

$$
R_{A}(x) = \frac{x^TAx}{x^Tx} = \lambda
$$
	where $\lambda$ is the corresponding **eigenvalue**. So the Rayleigh quotient gives the eigenvalue when you're already on the eigenvector.

* The **maximum** and **minimum** values of the Rayleigh quotient over unit vectors are the largest and smallest eigenvalues of the matrix $A$

> 🔍 **The Rayleigh quotient tells you how "eigen-like" a vector is** and approximates the eigenvalue if it's close to an eigenvector.


---

For an optimization problem over the Rayleigh Quotient: 

$$
max_{\lVert x \rVert = 1}x^TAx
$$
The optimal solution is $x = v_{max} = \lambda_{max}(A)$.

$v_{max}^TAv_{max} = v_{max}^T\lambda_{max}v_{max} = \lambda_{max}$   given that the eigenvectors are unit eigenvectors.



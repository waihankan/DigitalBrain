
Optimization problem with **(Convex) Quadratic Objective.** and linear inequality constraints.

Standard Form: 

Minimize $f_{0}(x) = \frac{1}{2}x^THx + c^Tx + d$ , which is a quadratic function.

subject to $Ax \leq b$

d doesn't change the opimization problem (only change value)

solver = `quadprog(H, c, d, A, b)`

Convexity $\iff$ H is PSD (not just symmetric).

---

### Examples of QP

#### 1. Least Square is a quadratic Programming
$$
min_{x}\lVert  Ax - b \rVert ^2_{2}
$$

expand the objective function.

$$
f_{0}(x) = x^T(A^TA)Ax - 2b^TAx + \lVert b \rVert ^2_{2}

$$
Pattern match with standard form: 

$$
H = A^TA
$$
$$
c = -2A^Tb
$$
No constraints in regular Least Squares.

---

#### 2. Linearly Constraint Least Square.

$$
min_{x}\lVert Ax - b \rVert _{2}^{2} \quad \text{such that} \quad Cx =d 
$$

$$
Cx = d \iff
Cx \leq d \quad \text{and} \quad
-Cx \leq -d
$$

---

#### 3. LASSO (l1 regularized least squares)

$$
min_{x}\lVert Ax - b \rVert ^2_{2}+ \lambda \lVert x \rVert _{1}
$$

Formulate this as quadratic program.

$$

min_{x, t}\lVert Ax-b \rVert ^2_{2} + \lambda \sum_{i} t_{i} \quad s.t \; x_{i} \leq t_{i}, -x_{i}\leq t_{i}

$$

Can convert this into standard form: check picture. 

the $\lambda$ is lagrange mulitpliers. incoporitng constraints into objective functions.

---

> l1 regularization encourages sparsity of the solution. Why? 

$$
min_{x} \lVert  Ax - b \rVert ^2_{2} \quad \text{s.t} \; ca^{(x) \leq k}
$$

cardinality(x) = # of non-zero entries in x vector.
(non-convex constraint and hard to deal with).


so l1 regularization is like solving l2 regularization with the sparsity constraint. (cardinality of x <= k)


$\lambda$ is a Lagrange multiplier - meaning that problem 3 (LASSO) is equivalent to $min_{x}\lVert Ax - b \rVert^2_{2} + \lambda\text{card}(x)$ for suitable $\lambda$. (still hard to solve). -> relaxation.

if lambda is big, cardinality will be small.

if lambda is small (0), don't care cardinility.

lambda = penalizaing / costing for not obeying the constraints.

Holder's inequality: $\lVert x \rVert_{1} \leq \text{card}(x)\lVert x \rVert_{\infty}$

> Relax Constrained problem.






submit homework 3

8.1

8.4 and 8.5 (features)
_Record your optimum prediction rate in your write-up and include your Kaggle_

_username. Don’t forget to use the “submissions” tab or link on Kaggle to select your best_

_submission!_
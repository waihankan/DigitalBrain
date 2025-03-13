---
draft: 
tags:
  - eecs127
---

Departure from Linear Algebra to Optimization

l2 regularization - Least Square
l1 


Problem: How to handle something like: $Ax \leq b$ OR $\min_{x}\lVert Ax - b \rVert_{1}$

New class of problems: Linear Programs (LPs)
	Any LP can be written in "Standard Form":
		(Linear cost and linear inequality constraints) (Most Popular)
		$$
		min_{x}\quad c^Tx 
		$$
		$$\text{such that } Ax \leq b$$

Other (**Equivalent**) Standard Form:
$$min_{x}\quad c^Tx$$
$$\text{such that } Ax = b, x \geq 0$$
"Standard Form" Provides us with a definition of LP = any problem that can be written in "that" form. 
	[Just as with Least squares] Cause it could be written as least squares problem through transformation. Same with Linear Programs. (Can be transformed although it might not be obvious)

> what do these constraints mean. In least square, x is not constraint. In LP, x have inequality constraints.

What does the "**Feasible Set**" of x s.t. $Ax \leq b$ looks like? 

$$Ax \leq b \quad \leftrightarrow \quad a_{i}^Tx \leq b$$

$$\{x : Ax \leq b\} = {x : a_{i}^Tx \leq b_{i}, i = 1, \dots, n}$$
$$\{x : a_{i}^Tx \leq b_{i}\}$$

Intersection form. 

In the space, -> "Half Space <- "Feasible Set"

Intersection of halfspaces is called a  "polytope" (basically the feasible region). Higher dimension of polygon.
Could be empty if the problem is infeasible problem. Empty Set.

[Picture] 


--- 


Examples: 
1. Probability Simplex
		$$\{x \in R^n : x_{i} >- 0, \sum_{i=1}^nx_{i} = 1\}$$
		$$x_{i} \geq 0 \text{for all i} \implies -x \leq 0$$
		$$\sum x_{i} = 1 \implies 1^Tx \leq \vec{1} \quad \text{and} \quad -1^Tx\leq-1$$
		Basically Changing from equality to inequality constraints.
Ex 2:  l1 ball (quite complicated in higher dimension, but nevertheless, an example of Polytope)

---

Generally Speaking, LP is in the form


$$min_{x}f(x) \text{st} x \in P\text{ where P is a polytope = intersection of finite x of }$$

Cost function is affine function $c^Tx + d$

Objective Function $df(x) = c$ points in the direction of max increase.
-> -c points in the direction of max decrease.


[picturessss]


---

Many problems don't look like LPs, but can be cast as such.

SVM is an LP.

Ex: $min_{x} c^Tx + \lambda \lVert x \rVert_{1}\text{such that} Ax \leq b$

The trick is often time Introduce new variables. Slack Variables.
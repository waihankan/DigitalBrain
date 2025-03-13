---
title: Application of LS to Time Series Analysis
tags:
  - eecs127
  - least-square
draft: false
---
Predicting the future based on the past few steps. If we have the weights and if we have the previous positions, can predict next steps.  Assuming the error term is zero mean. e(k) = y(k) - wTphi(k)

**problem** how to learn the weights `w`. How should i go about learning them? 

Idea: sample. training data.
Formulate an optimization problem. given training data, how to estimate the weights.

Problem: 

mine - what are $\phi$ vectors?
Suppose we observed the past history `y(1), y(2), ... y(N)`. This is the same as observing $\phi(1), \dots \phi(N)$


IDEA: $y(k) \sim w^T\phi(k)$
 
Formulate as LS problem: minimize $\lVert \phi w-y \rVert$.  phi is a matrix with phi(1)^T ... phi(N)^T as rows. check picture.

y  = y vector column

Note that all we need is linear in `w`. y(k) can be "not" linear.

Question: what if y(k) is a quadratic function of previous y(k-1) , y(k-1)? 

$$y(k) = w_{1}y(k-1) + w_{2}(k-2)+ w_{3}y(k-1)y(k-2) + w_{4}y(k-1)^2+w_{5}y(k-2)^2 + e(k)$$

The same scheme works for $\phi(k)$

---


#### Linear equations in Engineering. (Friendly Review of Some Examples)

Utility and modeling everything. the focus of 16A. Applied linear algebra. Modelling in real life (engineering sense)

Will become useful later for modeling things. reduce optimization models and so forth.

* Good for modeling **constraints** in Engineering / Science. A lot of things in natural are linear. 

#### Example 1: Tomography (16A) key- take log to make the equations linear


#### Example 2: Network Flows (Direct Graphs)

>**Linear Algebra's Role**

Linear algebra provides the tools to model and solve these problems. Here's how:

1. **Variables:** We use variables to represent the flow through each part of the network (e.g., the amount of water in a pipe, the number of cars on a road).
    
2. **Equations:** **The key principle is _conservation of flow_:**
    
    - **Conservation of flow** : The total flow into the network must equal the total flow out.
    - **Feasible Flow** : At any intersection (node), the flow in must equal the flow out.
	$$\sum_{e \in incoming}x_{e} = \sum_{e \in outgoing}x_{e}$$
	$$\sum_{u \in V st (u, v) \in E}x_{(u, v)} = \sum_{e \in outgoing}x_{e}$$
	$$\begin{matrix}
1 & 3 & 4 & 
\end{matrix}$$


    These principles translate into a system of linear equations.
    
3. **Matrices:** We represent the network and the flow equations using matrices. This allows us to use efficient techniques like Gaussian elimination to solve for the unknown flows.


---


#### Linear equations in optimization


minimize f(x) over x such that $Ax = b$    (Transporting Corn Example, Network Flow) 

$min_{x}f(x)$



Review of Linear Algebra Module. Big Points. What are important according to Tom.

* Linear Algebra = Language of Optimization
* Basic Object: vectors, matrices.
* Important Concepts: Subspaces, Bases, Norms, Inner Products, etc.
	* Fundamentally **Geometric** in Nature.
* Some of the most important Tools: 
	* **Projection (everything of this course {in a sense})**
		* Given Subspace S, solve $$ \pi_{s}(x) = argmin_{x \in S} \; \lvert  s - x \rvert^2 $$
		* Solution characterized by **Orthogonality** 
			* Meaning $x - \pi_{s}(x)$ and $y$ are orthogonal. $(x - \pi_{s}(x))^Ty = 0 \quad \forall y\in S$
		* **IMPORTANT**: Projection is a linear transformation $x \rightarrow \pi_{s}(x)$
		* Applications: 
			* **Gram-Schmidt** (Sequential Projection)
				Input = $x_{1}\dots x_{n}$ and Output $u_{1}\dots u_{n}$
				Application of Gram-Schmidt -> QR Decomposition

	* **Matrices represent Linear Transformation**
		* Four Fundamental Subspaces (The picture)
		* Pseudo-Inverse
		* The picture can sometimes can allow us to better understand / reduce problems.
			* One good example: $min_{x}\lvert Ax -b \rvert^2 \; \text{such that}\; Cx=d$ transform to $min_{z}\lvert Ax_{0} + AN_{z}-b \rvert^2 \text{{where}} \; x_{0} = C^+d$
			* **Basically transform an optimization problem with linear constraint to a least square problem**.
	* Many of the other concepts we saw from optimization problems:
		* Example if A is a symmetric n x n matrix, $\lambda_{max}(A) = max_{x}\frac{x^TAx}{x^Tx}$
		* Repeat to get Spectral Decomposition = $A = U\Lambda U^T = \sum \lambda_{i}u_{i}u_{i}^T$
		* Applications: **PCA** : just take spectral decomposition of $XX^T$
	* Singular Value Decomposition $A = U\Sigma V^T$ Comes from an optimization problem
		* For Generic $A \in R^{m \times n}$
			* $v_{1} = argmax_{v : \lvert v \rvert = 1} \lvert Av \rvert_{2}$
			* $\sigma_{1} = \lvert Av_{1} \rvert_{2}$
			* $u_{1} = \frac{Av_{1}}{\sigma_{1}}$
		* Application: 
			* Low Rank Approximation
			* PseudoInverse $A^+ = V\Sigma^+U^T$ Solving Least Squares, Mapping between Subspaces.



---
title: Vanishing Gradient Problem (ReLUs)
tags:
  - cs189
  - machine-learning
  - neural-net
---
Problem: when unit output `s` is close to 0 or 1 for most training points, $s' = s(1-s)$ is going to be very small making the gradient descent very slow and not efficient. **Unit is Stuck | Slow Training**. (We don't want the data to be in the flat spot). Also, the middle part (non-flat) part is kinda like a linear region. We want neural networks to have non-linear activation functions.


<figure align="center">
<p align="center">
	<img src="Pasted image 20250409233608.png">
</p>
<span>Sigmoid and its issues</span>
</figure>


Solution: Replace Sigmoids with Rectified Linear Units (ReLUs)

### Rectified Linear Units (ReLUs)

$$
\begin{align}
	r(x) &= max(0, x) \\
	r'(x) &= \begin{cases}
	 0 \quad if \; x < 0 \\
	 1 \quad if \; x \geq 0
	\end{cases}
\end{align}
$$

```python
def relu(x):
	return np.maximum(0, x)
```

![[Pasted image 20250409234300.png]]

> Sub-gradient at x = 0 is not a very big problem.

* In ReLU, Exploding Gradient can be a problem in Deep Neural Network. 
* Although it's mostly linear, in practice, it gives enough non-linearity. 
* Most Neural Network today use ReLU Function as Hidden layers.
* ReLUs can get stuck too just like sigmoid functions; but it is rare in practice.

Output Units: chosen to fit the application unlike the hidden layers.
* Regression: Linear regression (Last Layers)
* Classification: Sigmoid (2 class) | Softmax (multi-classes)

---


### Output Units

#### Regression
The activation function is the identity function. Usually trained with squared error loss. So, it is equivalent to doing least squares linear regression on learned features (hidden units).
**Loss function**: Squared Error Loss
#### Binary Classification (Sigmoid)
Given vector `h` of unit values in the last hidden layer, output layer computes pre-activation value $a = Wh$, and the applies sigmoid activation function $s(Wh)$ to obtain the prediction $\hat{y} = s(Wh)$. 

**Loss function**: <u>Logistic Loss</u> | Fixes the vanishing gradients at output (because of the shape of logistic loss). 
#### K-class classification (Softmax)
* $y \in R^k$ be a vector of labels for training point x. 
* Choose training labels so that $\sum_{i=1}^{k}y_{i} = 1$
* One-hot encoding
Given hidden layer `h` output layer computes pre-activation value $a = Wh$ and applies softmax activation to obtain prediction $\hat{y}$ , where

$$
\hat{y}_{i}(a) = \frac{e^{a_{i}}}{\sum_{j=1}^{k}e^{a_{j}} }
$$

**Loss function**: <u>cross-entropy loss</u> | Fixes the vanishing gradient problem at the output.

$$
L(\hat{y}, y) = -\sum_{i=1}^{k}y_{i}\ln \hat{y}_{i}
$$

> Different in a way that the $y_{i}$ are dependent on each other if you compare it to the prediction from sigmoid for example.

---


| output + loss              | linear + squared error                               | sigmoid + logistic loss                                                       | softmax + cross entropy loss                                 |
| -------------------------- | ---------------------------------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------ |
| $\hat{y}$, $L(\hat{y}, y)$ | $\hat{y} = Wh$ ; $L = \lVert \hat{y} - y \rVert^{2}$ | $s(Wh)$ ; $L = -\sum_{i}(y_{i}\ln \hat{y}_{i} + (1-y_{i})\ln(1-\hat{y}_{i}))$ | $\hat{y}= softmax(Wh)$ ; $L = -\sum_{i}y_{i}\ln \hat{y}_{i}$ |
| $\nabla_{w}L$              | $2(\hat{y}- y)h^T$                                   | $(\hat{y} - y)h^T$                                                            | $(\hat{y}- y)h^T$                                            |
| $\nabla_{h}L$              | $2W^T(\hat{y} - y)$                                  | $W^T(\hat{y}-y)$                                                              | $W^T(\hat{y}- y)$ assuming $\sum y_{i} = 1$                  |

---

### Backpropagation



$$
V = \begin{bmatrix}
V_{11} & V_{12}  & V_{13}  \\
V_{21} & V_{22}  & V_{23}  \\
\end{bmatrix}
$$

$$
V_{1} = \begin{bmatrix}
V_{11} \\
V_{21}
\end{bmatrix}
$$

$$
V_{1}^T = \begin{bmatrix}
V_{11} & V_{12}  & V_{13}
\end{bmatrix}
$$


$$
V_{1}^Tx = \begin{bmatrix}
V_{11} & V_{12}  & V_{13}
\end{bmatrix} \begin{bmatrix}
x_{1} \\
x_{2} \\
x_{3}
\end{bmatrix}
$$
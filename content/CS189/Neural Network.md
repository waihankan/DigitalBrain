---
title: Neural Network
tags:
  - cs189
  - machine-learning
  - neural-net
---
<u>The Parity Problem</u>
- Perceptrons - AI Winter

| XOR | 0   | 1   |
| --- | --- | --- |
| 0   | 0   | 1   |
| 1   | 1   | 0   |

<u>Solution</u>:
If you add one new quadratic feature $x_{1}x_{2}$, **XOR** is linearly separable in 3D.

<p align="center">
<img src="Pasted image 20250408225607.png">
</p>

> There is another more powerful way to solve this XOR problem :  Stacking Linear Combos


![[Pasted image 20250408225824.png]]

This is close, but it won't work yet. A linear combo of a linear combo is a linear combo . . . only works for linearly separable points.

We need some non-linearity. 
* Traditional Choice: Logistic Function (*Sigmoid*)  : Smooth, well defined gradients and Hassian.
* RELU


![[Pasted image 20250408230104.png]]


---

### Network with 1 Hidden Layer

Input Layer: $x_{1}, \dots, x_{d}; x_{d+1} = 1$ 
Hidden Layer: $h_{1}, \dots, h_{m}; h_{m+1} = 1$
Output Layer: $\hat{y}_{1}, \dots, \hat{y}_{k}$

Layer 1 weights: $m \times (d+1)$ matrix V;    $V_{i}^T$ is row i; weights into $h_{i}$
Layer 2 weights: $k \times (m + 1)$ matrix W; $W_{i}^T$  is row i; weights into $\hat{y}_{i}$

The weights are in row. (since we want to do $Vx$). So, each $v_{i}$ in the row is gonna be the weight for $x_{1}, x_{2}, \dots$.




---


---
title: Decision Trees
tags:
  - cs189
  - machine-learning
---
> [!abstract] Decision Tree
> A Decision Tree is a **non-linear model** used for both classification and regression. 
> A decision tree consists of two types of nodes:
> * **internal nodes**: test feature values (usually just one) and branch accordingly. (*e.g. is humidity > 75%?*)
> * **leaf nodes:** specify class $h(x)$. (*e.g. 'yes' or 'no'*)

* It breaks the input space into rectangular regions using binary decisions.
* **Interpretable result (inference)**.
* We can think of it as a game of several questions: answering 'yes/no' questions to narrow down the answer.
* The decision boundary can be arbitrarily complicated.

<p align="center">
<img src="Pasted image 20250330192145.png">
</p>

>[!warning]
> * Notice how decision trees are **selective**.
> * A decision tree will only split features that are useful for making decisions. Therefore, not all features will be used in the final tree.
> * Why is that? Because at each node, the tree **greedily picks the best feature and split value - the one that most reduces the entropy**.
> * ✅ **Interpretability**: The tree tells you which features _matter_.
> * ✅ **Feature Selection**: Trees perform implicit feature selection.
> * ⚠️ **Instability**: Small changes in data can change which features are picked — so decision trees can be _unstable_ (but this is fixed with ensembles like random forests).

---

## 🪓 **How Trees Learn (Top-Down Recursive Splitting)**

* Start with all training data `S = {1, 2, ..., n}`.
* At each node:
	* If all labels are the same -> create a leaf (pure).
	* Else:
		* Try all **possible splits over all features and all values**
		* Choose the split that minimizes the impurity (more on this below)
		* Recur on the left and right subsets.

> This is a Greedy Algorithm often known as CART ( Classification and Regression Trees).

---

## 🙋‍♂️ How do we choose the best split?

#### 1. ❌ Bad Cost Function: Misclassification Error
* Label the majority class `C` and define the cost `J(S)` as the # of points not in C.
* **Problem**: Not Sensitive enough. Two splits with very different class balances may have the same cost.

<p align="center">
<img src="Pasted image 20250330193823.png">
</p>
*The issue here is cost_before = cost_after  = ($J(S_{l})+J(S_{r})$)for both left and right figures even though the left split seems to do a better job.*

Weighted Sum of Cost will prefer the right split (which is not a good split).

#### 2. ✅ Good Cost Function: Entropy (Information Theory)

Suppose Y be a random class variable and $P(Y = C) = P_{c}$.

1. **The `Surprise` of Y being in class `C` is:** $-\log_{2}P_{c}$ (which is always a positive number)
	* event with probability 1 gives us *zero* surprise.
	* event with probability 0 gives us *infinity* surprise.


<p align="center">
<img src="Pasted image 20250330195831.png" width="200px" height="250px">
</p>
$\log(x)$ graph. Negative between 0 and 1. $\log(0) = -\infty$.

1. **Entropy of an index set S**: is **the average surprise** when you draw a point at random from S.
$$
H(S) = -\sum_{C}P_{c}\log_{2}P_{c}
$$
* If all points belong to one class: $H(S) = -1\log_{2}1 = 0$
* If half of the points in C and the other half in D: $H(S) = -0.5\log_{2}(0.5) -0.5\log_{2}(0.5) = 1$
* `n` points with all different classes: $-\log_{2}(n) = \log_{2}(n)$

<p align="center"> 
	<img src="Pasted image 20250330201337.png">
</p>
> [!note] Information Gain
> Information gain is defined as: 
> $$
> H(S) - H_{\text{after}}
> $$
> where 
> $$
> H_{\text{after}} = \frac{|S_{l}|H(S_{l})+\lvert S_{r}\rvert H(S_{r})}{\lvert S_{l} \rvert + \lvert S_{r} \rvert}
> $$

---
### Best Split = Split that maximizes the information gain $H(S) - H_{\text{after}}$

<p align="center">
<img src="Pasted image 20250330225732.png">
</p>
> [!danger] Important
> Information gain is always positive except it is `zero` when one child is empty (in which case, it should just be leaf node) or for all C, $P(y_{i} = C | i \in S_{l}) = P(y_{i} = C | i \in S_{r})$}

<p align="center">
<img src="Pasted image 20250330230043.png">
</p>


<p align="center">
<img src="Pasted image 20250330230458.png">
</p>
==Many concave functions work fine as the cost function, including the simple polynomial p(1 − p).==

---

### More Information on Choosing a Split

* For binary feature $x_{i}$, children are $x_{i} = 0 \; \text{and} \; x_{i} = 1$.
	* $S_{left} = { i \in S : x_{i} = 0}$
	* $S_{right} = { i \in S : x_{i} = 1}$
* If $x_{i}$ has 3+ discrete values: split depends on application: (multiway splits or binary splits).
* If $x_{i}$ is quantitative: sort $x_{i}$ values in S; try splitting between each pair of unequal consecutive values.
* If we use `radix sort`, we can sort in linear time O(n). As we can scan sorted list from left to right, we can update entropy in O(1) time per point.
* ![[Pasted image 20250331010534.png]]

#### Algorithm and Running Times:

**Classification**
1. Walk down tree until leaf. Return its label.
2. Worst Case Runtime is `O(tree depth)`.
3. For binary features, tree depth is  $\leq$ d. (Quantitative features may go deeper).
4. Usually (not always) $\leq$ `O(logn)`.

**Training**
1. For binary features, try `O(d)` splits at each node.
2. For quantitative features, try `O(n'd)` splits where n' = points in node.
		* For each feature, we sort the `n'` values and try `n'-1` thresholds.
		* Do this for `d` features -> `O(n'd)` total splits to consider at this node.
		
> Quantitative features are asymptotically just as fast… clever entropy trick”
> 
💡 The clever trick: As you scan from left to right through the sorted feature values, you **update the entropy in O(1) time per split**.
>
> So the **total cost is linear in n′** per feature — no need to recompute from scratch each time.

Each point participate in `O(tree depth)` nodes, costs `O(d)` in each node. So for each sample point, it costs `O(d * depth)`. 

Thus, running time is `O(nd depth)`

`nd` is the size of the matrix X.

---


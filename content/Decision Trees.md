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



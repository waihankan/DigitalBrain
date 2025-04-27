---
title: Ensemble Learning
tags:
  - cs189
  - machine-learning
---
> Decision Trees are fast, simple, interpretable and **invariant** under scaling and translation, robust to irrelevant features.

> Decision Trees are not at prediction. They have high variance although the bias is low. (High variance -> think about how the first split can totally change the entire split).

> Ensemble learning = combining multiple models (often weak ones) to create a stronger overall model.
---

**The average opinion of a bunch of idiots is better than the opinion of one expert. And so it is with learning algorithms.**

We can take the average of output of 
1. Different learning algorithms
2. Same Learning algorithm on many training sets (if we have a bunch of data)
3. <u>Bagging</u>: Same learning algorithm on many random subsamples of one training set
4. <u>Random Forest</u> Randomized decision trees on random subsamples

Metalearner takes test point, feeds it into all T learners, returns majority class or average output.
* Regression: take median or mean output of all the weak learners
* Classification: take majority or average of posterior probabilities

When choosing weak learners, it is preferable to choose learners with **low Bias**. The variance can be high since averaging reduces the variance. Averaging can sometimes reduce the bias and increases flexibility a bit but not reliably. e.g. creating non-linear decision boundary from linear classifiers.

Sometimes the number of trees is said to be a hyperparameter, but it’s not really true. ==When random forests work, usually the variance drops monotonically with the number of trees, and the main limit on the number of trees is computation time.== The stopping conditions are the hyperparameters.

---
## 🧺 Bagging

> ✅ The **same type of model** (e.g., decision tree)  
> 🔁 Trained on **different random subsets** of the data

**It works well with many different learning algorithms except k-nearest neighbors.**

### How Bootstrap Aggregating works

Given n training sample, we generate a random subsample of size `n'` by sampling with **replacement**. Some points maybe be chosen multiple times and some points may not be chosen at all.

If `n = n'`, approximately `63.2%` are chosen on average.

Note: points that get chosen multiple (j) times should be treated respectively:
* Decision Trees: j-time point -> j $\times$ weight in entropy.
* SVMs: j-time point -> j $\times$ penalty to violate margin.
* Regression: j-time point -> j $\times$ loss.

### ✅ Bagging uses **one model type**, not different models

- For example: 100 **decision trees**, all trained independently.
- Each one is trained on a **bootstrapped sample** (randomly sampled _with replacement_ from the training set).
- They might also differ slightly due to random feature choices (like in **random forests**).

You are **not** using:
- SVM + decision tree + neural net + etc. (that’s **stacking**, not bagging)
- Models with different hyperparameters (you can, but that’s not the key idea)

---

## When 🧺 Bagging is not enough -> 🌳 RandomForests

With bagging 🧺 , the decision trees may often look similar. This is because of the presence of a **Strong Predictor**. If we have such one feature, the same feature will be split at the top of the tree, thus resulting in very similar trees even with random subsamples.

> [!cite] 
> For example, if you’re building decision trees to identify spam, the first split might always be “viagra.” Random sampling might not change that. If the trees are very similar, then taking their average doesn’t reduce the variance much.

#### Fix: Random Forests

1. At each node, take random sample of `m` features out of `d`.
2. Choose best split from `m` features. This allows some randomness in the tree split even with a strong predictor feature.
3. $m \approx \sqrt{ d }$ works well for classification and $m \approx \frac{d}{3}$ works well for regression. 
4. `m` is a hyperparameter and need to be tuned although the above is a good starting point. Smaller `m` -> more randomness -> less correlation between decision trees, more bias. *More bias because the split we make is **suboptimal**. Thus, each tree may have higher bias.*

Sometimes test error drops even at 100s or 1,000s of decision trees! 
**Disadvantages**: slow; loses interpretability/inference for the cost of being more accurate.

---
## 💡 Real-life Analogy

Ensemble learning is like:
- Asking multiple doctors for a diagnosis (bagging)
- Getting a tutor who focuses on what you got wrong (boosting)
- Asking specialists, then letting an expert coordinator decide (stacking)

---




---
title: Building Classifiers
tags:
  - machine-learning
  - cs189
---
#### Preface

I will start this note with not much understanding of the models, but by the end of the note, I will revise the different models again, hopefully - with a better understanding of the concept.

#### 3 Ways to Build Classifiers 

1. Generative Models (Linear Discriminant Analysis)
> Core Idea: Try to learn the **underlying probability distributions** that generate the data for **each class separately**. Similar to figuring out how "each class" creates its data points.

2. Discriminative Models (Logistic Regression)
> This model **Directly Learn the Decision Boundary** between classes or the conditional probability $P(Y|X)$ without explicitly modeling the individual class distributions. "The Focus is on Finding WHAT separates the classes from each other ."
3. Find Decision Boundary (Support Vector Machine)
> No explicit calculations of **Probabilities**. Directly find the **Optimal Decision Boundary** that separates the classes.

<small> ⭕ Come back to recognize the advantages and disadvantages of different models.</small>


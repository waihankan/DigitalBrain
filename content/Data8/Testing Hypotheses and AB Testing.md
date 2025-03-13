---
title: Testing Hypotheses and AB Testing
tags:
  - data8
  - data-analysis
---
> [!note]+
> Just Some Quick Notes

* **Model** is a set of assumptions.



### Choose Test Statistic (something that changes)

The statistic has to be able to help us decide between the model and alternative views about the data.

Example: A natural statistic, then, is *the number or count of Black panelists in the sample. Small values of the statistic will favor Robert Swain’s viewpoint.*

Example: Distance between two Distributions `0.5 * sum of abs(difference)`

By a bit of math that we won’t do here, this is true whenever there are just two categories: the TVD is equal to the distance between the two proportions in **one category.**


### Null Hypothesis $H_{0}$vs Alternative Hypothesis $H_{1}$

The **null hypothesis (H₀)** represents the **default assumption** or the **status quo**—it assumes that there is no significant effect, no difference, or no relationship.

- We choose **H₀ as "no improvement"** because we start by assuming that the new method **does not** work better than the traditional one.
- The null hypothesis is what we **test against** using data. We assume it is **true unless the evidence strongly suggests otherwise**.


**Alternative hypothesis** can be **one-sided (directional) or two-sided (non-directional)**

**Common Forms of H₁:**

- Two-tailed test (non-directional):​ (e.g., "There is a difference in mean test scores between two groups.")
- One-tailed test (directional):
    - (e.g., "This drug increases test scores.")
    - (e.g., "This training program reduces reaction time.")

#### One-Tailed vs Two-Tailed Hypothesis
* Two-tailed tests are used when you *just* want to check for any difference (increase or decrease)
* One-tailed tests are used when you expect a specific direction of change (either increase or decrease)
* 
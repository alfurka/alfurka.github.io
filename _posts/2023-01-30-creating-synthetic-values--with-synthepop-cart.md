---
layout: post
title: "Critical CART Hyperparameters in Synthpop"
subtitle: Creating Synthetic Data with R
tags: [synthpop,cart,ctree,R,synthetic-data,hyperparameters,overfitting]
image: /img/brain-neural.png
---

I have been working on a project to create synthetic data for a long while. I have realized that the synthpop package was producing identical values, whereas it was supposed to be producing values from a predictive posterior distribution. I have been using the CART algorithm for its flexibility, but the model must be overfitting, even though I do not have too many variables. So, how can one prevent creating identical values? The answer was given in the article: "synthpop: An R package for generating synthetic versions of sensitive microdata for statistical disclosure control".

Here are some recommendations:

- To create synthetic values from the predicted posterior distribution, set the parameter in synthpop as follows: `proper = T`. (The authors call it the proper way to create synthetic data)
  - In addition to setting `proper = T`, you can also use `control=list(stop.iterations=20)` to set the maximum number of iterations to 20. This can help to prevent overfitting by stopping the CART algorithm before it becomes too complex.
- To prevent overfitting, set `cart.minbucket=5` to make sure that there are at least 5 observations in each node.
  - You can also use `control=list(stop.complexity=0.1)` to set the maximum complexity of the tree to 0.1. This can also help to prevent overfitting by stopping the CART algorithm before it becomes too complex.
- To create smoothed continuous values, set `smoothing='density'` or `smoothing='spline'`. The package creators recommend using `smoothing='spline'`.
  - When using the smoothing parameter, it's important to note that you can also use `smoothing=list(var1='density', var2='spline')` to specify different smoothing methods for different variables.
- It's also important to keep in mind that it's a good practice to evaluate the quality of the synthetic data using various metrics such as the **mean squared error**.

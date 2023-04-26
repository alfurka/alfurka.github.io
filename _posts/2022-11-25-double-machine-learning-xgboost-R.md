---
layout: post
title: "An Implementation of Double Machine Learning with XGboost in R"
subtitle: A Benchmark Estimate
tags: [blog, semiparametric, machine-learning, XGboost, R]
image: /img/avatar-icon.png
---

This is an attempt to estimate [Double Machine Learning](https://onlinelibrary.wiley.com/doi/full/10.1111/ectj.12097) with [XGboost](https://xgboost.readthedocs.io/en/stable/) algorithm in R. The purpose is to create a benchmark estimation with DML. The user can choose various machine learning algorithms, where optimizing hyperparameters can be time-consuming. XGboost is a very useful in this regard. This script can be used to produce substantially accurate preliminary results. Repository is [here](https://github.com/alfurka/dml_xgboost).

XGboost parameters can be specified by the user, separately for the outcome $(y)$ and the variable of interest $(d)$. The notation of the R function parameters is the same with the following equation:

$$
y = \beta d + f(x) + error. 
$$

## Remarks

- Coefficient estimate is the average of cross-validated estimations. 
- Standard errors are calculated. 
- The best number of iteration for XGboost is decided by cross-validation within the train sample.


## Parameters

- `y`: outcome variable (vector)
- `d`: variable of interest (vector)
- `x`: matrix of exogenous regressors
- `k_fold`: The number of cross-validated estimations in DML (default=5 as suggested by the authors)
- `k_fold_validation`: There is another cross-validation done with the train sample to decide the best number of iterations. (default = 10)
- `y.params`: parameters for XGboost fit for `y`. (Default is XGboost default)
- `d.params`: parameters for XGboost fit for `d`. (Default is XGboost default) 
- `verbose`: for XGboost function (default = 0)

## Example

I try to replicate a code that does a similar estimation [here](https://docs.doubleml.org/r/stable/articles/getstarted.html). 

```{r}
> source('dml_xgboost.R')
> df_bonus = read.csv('df_bonus.csv')
> 
> y = df_bonus$inuidur1
> d = df_bonus$tg
> x = df_bonus[,c("female", "black", "othrace", "dep1", "dep2",
+                 "q2", "q3", "q4", "q5", "q6", "agelt35", "agegt54",
+                 "durable", "lusd", "husd")]
> fit = dml_xgboost(y = y, d = d, x = x)
> 
> summary(fit)

Call:
lm(formula = y.preds ~ 0 + d.preds)

Residuals:
    Min      1Q  Median      3Q     Max 
-2.9238 -0.9218  0.3779  1.0788  3.0205 

Coefficients:
     Estimate Std. Error t value Pr(>|t|)  
[1,] -0.07688    0.03565  -2.156   0.0311 *
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 1.216 on 5098 degrees of freedom
Multiple R-squared:  0.0009159,	Adjusted R-squared:  0.0007199 
F-statistic: 4.673 on 1 and 5098 DF,  p-value: 0.03068
```

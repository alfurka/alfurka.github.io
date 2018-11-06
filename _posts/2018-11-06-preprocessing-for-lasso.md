---
layout: post
title: Preprocessing for LASSO
subtitle: Polynomial Features
tags: [Lasso, machine learning, preprocessing, regularization]
image: /img/hoca.jpg
bigimg: /img/blackboard.png
---

I have been working on `Double Machine Learning` methodology at The University of Queensland since three weeks. Therefore, I am spending most of my time with ML estimations and I am trying to enhance their performance. I tried many preprocessing tools for each ML algorithms separately. I had noticed that, adding polynomial features yields too much performance increase for LASSO. In my case, even 1% performance increases matter, but adding polynomial features increases performance between 10-25% depending on the estimation type. I believe adding polynomial features matters especially if data contains more nonlinear covariates. 

You should primarily consider adding polynomial features before using LASSO. Then you may use additional preprocessing tools like normalization or scaling. 

If you are using Python you can do it with pipelines without much effort:

```python
from sklearn.linear_model import Lasso
from sklearn.pipeline import Pipeline

model = Pipeline([('poly', PolynomialFeatures()), 
                  ('model', Lasso(alpha=0.01))])
```

**Note**: You must carefully regularize the`alpha` value. Try to create test functions to find the optimum.  LASSO is very sensitive both to regularization and 

See documentations:

- [PolynomialFeatures](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.PolynomialFeatures.html)
- [Lasso](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.Lasso.html)
---
layout: post
title: Tuning ML Hyperparameters - LASSO and Ridge Examples
subtitle: sklearn.model_selection.GridSearchCV
tags: [blog, machine learning, preprocessing, model selection, lasso, ridge, regularization, hyperparameters]
image: /img/avatar-icon.png
bigimg: /img/learning.jpg
---

As far as I see in articles and in Kaggle competitions, people do not bother to regularize hyperparameters of ML algorithms, except of neural networks. One tests several ML algorithms and pick up the best using cross-validation or other methods.

### Why you should tune hyperparameters? 

Consider the Ordinary Least Squares:


$$
\begin{equation}
\mathcal{L}_{OLS} = ||Y-X^T\beta||^2
\end{equation}
$$


OLS minimizes the $ \mathcal{L} _{OLS}$ function by $\beta$ and solution, $\hat{\beta}$, is the Best Linear Unbiased Estimator (BLUE). However, by construction, ML algorithms are biased which is also why they perform good. For instance, LASSO only have a different minimization function than OLS which penalizes the large $\beta$ values:


$$
\begin{equation}
\mathcal{L}_{LASSO}  = ||Y - X^T\beta||^2 + \lambda ||\beta||
\end{equation}
$$


Ridge Regression have a similar penalty:


$$
\begin{equation}
\mathcal{L}_{Ridge} = ||Y - X^T\beta||^2 + \lambda ||\beta||^2
\end{equation}
$$


In other words, Ridge and LASSO are biased as long as $\lambda > 0$.  And other fancy-ML algorithms have bias terms with different functional forms. But why biased estimators work better than OLS if they are *biased*? Yes simply it is because they are *good* biased. But note that, your bias may lead a worse result as well. And this is the critical point that explains why hyperparameter tuning is very important for ML algorithms. What we mean by it is finding the best bias term, $\lambda$. 

### Example

Let's import our libraries:

```python
import pandas as pd
import numpy as np
from sklearn import metrics
from sklearn import linear_model
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import PolynomialFeatures
from sklearn.model_selection import GridSearchCV, train_test_split
```

I will use David Card's return to years of schooling data. You can import dataset as given below:

```python
df = pd.read_stata('http://fmwww.bc.edu/ec-p/data/wooldridge/card.dta')
```

There are some `NaN` values. I will fill them with medians:

```python
df = df.fillna(df.median())
```

The data contains the given variables:

```python
Index(['id', 'nearc2', 'nearc4', 'educ', 'age', 'fatheduc', 'motheduc',
       'weight', 'momdad14', 'sinmom14', 'step14', 'reg661', 'reg662',
       'reg663', 'reg664', 'reg665', 'reg666', 'reg667', 'reg668', 'reg669',
       'south66', 'black', 'smsa', 'south', 'smsa66', 'wage', 'enroll', 'KWW',
       'IQ', 'married', 'libcrd14', 'exper', 'lwage', 'expersq'],
      dtype='object')
```

For illustration purpose, I will use `educ` for prediction and some as explanatory:

```python
X = ['married', 'exper', 'expersq',
     'nearc2', 'nearc4', 'fatheduc', 'motheduc',
     'weight', 'momdad14', 'sinmom14', 'step14', 
     'reg661', 'reg662', 'reg663', 'reg664', 
     'reg665', 'reg666', 'reg667', 'reg668',
     'south66', 'black', 'smsa', 'south', 'smsa66']
Y = ['educ']
```

Some parameters can cause perfect multicollinearity. Be careful if you do different specifications.

I define my function to test the ML algorithms performances:

```python
def test(models, data, iterations = 100):
    results = {}
    for i in models:
        r2_train = []
        r2_test = []
        for j in range(iterations):
            X_train, X_test, y_train, y_test = train_test_split(data[X], 
                                                                data[Y], 
                                                                test_size= 0.2)
            r2_test.append(metrics.r2_score(y_test,
                                            models[i].fit(X_train, 
                                                         y_train).predict(X_test)))
            r2_train.append(metrics.r2_score(y_train, 
                                             models[i].fit(X_train, 
                                                          y_train).predict(X_train)))
        results[i] = [np.mean(r2_train), np.mean(r2_test)]
    return pd.DataFrame(results)
```

Training and test sizes will be `80%` and `20%`, respectively. I will compare the training and test $R^2$ values. Let's define the algorithms:

```python
models = {'OLS': linear_model.LinearRegression(),
         'Lasso': linear_model.Lasso(),
         'Ridge': linear_model.Ridge(),}
```

Run the test:

```python
test(models, df)
```

|   OLS    |  LASSO   |  Ridge   |
| :------: | :------: | :------: |
| 0.535598 | 0.470294 | 0.536013 |
| 0.529979 | 0.469845 | 0.528149 |

As you see, OLS performs better than both LASSO and Ridge. On the other hand, LASSO performs really bad. Let's do a Grid Search:

```python
lasso_params = {'alpha':[0.02, 0.024, 0.025, 0.026, 0.03]}
ridge_params = {'alpha':[200, 230, 250,265, 270, 275, 290, 300, 500]}

models2 = {'OLS': linear_model.LinearRegression(),
           'Lasso': GridSearchCV(linear_model.Lasso(), 
                               param_grid=lasso_params).fit(df[X], df[Y]).best_estimator_,
           'Ridge': GridSearchCV(linear_model.Ridge(), 
                               param_grid=ridge_params).fit(df[X], df[Y]).best_estimator_,}
```

I search for `alpha` hyperparameter (which is represented as $ \lambda $ above) that performs best. `GridSearchCV`, by default, makes `K=3` cross validation. I do not change anything but `alpha` for simplicity. You should check more about [GridSearchCV](http://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html). On the other hand, you should converge the hyperparameters by yourself. Result:

```python
test(models2, df)
```

|   OLS    |  Lasso   |  Ridge   |
| :------: | :------: | :------: |
| 0.537076 | 0.532037 | 0.531261 |
| 0.523822 | 0.525375 | 0.529110 |

The OLS results are slightly different from the first estimation but it is because of random sample splitting. The difference become less with more iterations. 

Now, both LASSO and Ridge performs better than OLS, but there is no considerable difference. Their performances can be increased by additional regularizations. But, I want to show a way that I mentioned in a article about [Polynomial Features](https://alfurka.github.io/2018-11-06-preprocessing-for-lasso/). I said it is an important preprocessing tool for LASSO but same goes for Ridge: 

```python
lasso_params = {'fit__alpha':[0.005, 0.02, 0.03, 0.05, 0.06]}
ridge_params = {'fit__alpha':[550, 580, 600, 620, 650]}

pipe1 = Pipeline([('poly', PolynomialFeatures()),
                 ('fit', linear_model.LinearRegression())])
pipe2 = Pipeline([('poly', PolynomialFeatures()),
                 ('fit', linear_model.Lasso())])
pipe3 = Pipeline([('poly', PolynomialFeatures()),
                 ('fit', linear_model.Ridge())])

models3 = {'OLS': pipe1,
           'Lasso': GridSearchCV(pipe2, 
                                 param_grid=lasso_params).fit(df[X], df[Y]).best_estimator_ ,
           'Ridge': GridSearchCV(pipe3, 
                                 param_grid=ridge_params).fit(df[X], df[Y]).best_estimator_,}
```

Note that hyperparameters have been changed. You must search for the hyperparameter interval by yourself. 

```python
test(models3, df)
```

|   OLS    |  Lasso   |  Ridge   |
| :------: | :------: | :------: |
| 0.626344 | 0.579805 | 0.586345 |
| 0.529119 | 0.547069 | 0.545070 |

There is approximately $~2\%$ increase in $R^2$ for LASSO and Ridge regressions, but not for OLS. As I have said earlier, LASSO and Ridge regressions perform better with higher dimensional data. 

The prediction performances can be increased with other preprocessing tools and further regularizations. 
---
layout: post
title: Estimating the Poisson Regression Model with Newton's Method - Python Example
subtitle: Newton's Method from Scratch
tags: [r example, Poisson regression, econometrics, asymptotics]
image: /img/hoca.jpg
---

The Poisson Regression model estimates the Poisson population parameter $\lambda _i$ related to the regressor covariate $x_i$. The outcome variable $y_i$ is hence assumed to be drawn from a Poisson distribution. Specifically, the data generating process is:

$$ Prob(Y=y_i | x_i) = \dfrac{e^{-\lambda _i}\lambda _i ^{y_i}}{y_i !}, $$

where $y_i = 0,1,2,...$. The conventional way of modeling $\lambda _i$ is as follows:

$$ \ln \lambda _i = x_i '\beta .$$ 

Note that $x_i'$ is a $k-$dimensional transposed-vector. A weel known feature of the Poisson distribution is that:

$$ E[y_i|x_i] = Var(y_i|x_i) = \lambda _i .$$

Combining the last two equations, we have:

$$ E[y_i|x_i] = Var(y_i|x_i) = \lambda _i = e^{x_i ' \beta }.$$

We can estimate the $\beta$ by maximizing the log-likelihood function:
$$
    \begin{aligned}
        L(\beta ) &= \log \left [ \prod _{i=1} ^n \dfrac{e^{-\lambda _i} \lambda _i ^{y_i}}{y_i!}\right ]\\
        &= - \sum _{i=1} ^n \lambda _i + \sum _{i=1} ^n y_ix_i'\beta + \sum _{i=1} ^n \ln (y_i!)
    \end{aligned}
$$

Log-likelihood can be maximized by FOC:

$$
\dfrac{\partial L(\beta )}{\partial \beta } = \sum _{i=1} ^n (y_i - \lambda _i) x_i = 0\\
$$

The hessian is:

$$
\dfrac{\partial ^2 L(\beta )}{\partial \beta \partial \beta '} = -\sum _{i=1} ^n \lambda _i x_i x_i'\\
$$

We can simply use Newton's method to maximize the Log-likelihood function with Hessian Matrix. 

## Newton's Method

Refer to [Wikipedia](https://en.wikipedia.org/wiki/Newton%27s_method_in_optimization) for Newton's method. We will use it to find the solution for $\beta$. In summary, we will write an iterative algorithm that updates $\beta$ as follows:

$$\beta _{t+1} = \beta _t + \dfrac{\partial ^2 L(\beta )}{\partial \beta \partial \beta '} ^{-1}\dfrac{\partial L(\beta )}{\partial \beta } $$

## Estimation Poisson Regression with Newton's Method - A Python Example

I use the following Python packages:

```{Python}
import pandas as pd
import numpy as np
from scipy.special import factorial
```

I import a data set:

```{Python}
data = pd.read_csv('https://stats.idre.ucla.edu/stat/data/poisson_sim.csv')
data['ones'] = 1
data['prog1'] = pd.get_dummies(data.prog).values[:,0]
data['prog2'] = pd.get_dummies(data.prog).values[:,1]
```

I create a constant variable column called `ones.` `Prog` is a categorical variable. Hence, I create dummies and drop the last one. We will use two dummies in the regression `prog1` and `prog2`. In addition, we have `math` scores that are continuous. We will regress `num_awards` to `ones`, `prog1`, `prog2`, and `math`. 

The first ten observations in the data set:

||    id | num_awards|  prog|  math  |ones|  prog1|  prog2|
|--|    ------ | ----|  --|  --  |--|  --|  --|
|0  | 45 |          0   |  3|    41  |   1|      0      |0|
|1  |108  |         0   |  1  |  41  |   1   |   1    |  0|
|2  | 15   |        0   |  3  |  44  |   1  |    0   |   0|
|3  | 67    |       0   |  3  |  42  |   1  |    0    |  0|
|4  |153     |      0   |  3  |  40  |   1    |  0   |   0|
|5  | 51      |     0   |  1  |  42  |   1   |   1    |  0|
|6  |164      |     0   |  3  |  46  |   1 |     0   |   0|
|7  |133      |     0  |   3  |  40  |   1  |    0   |   0|
|8 |   2       |    0 |    3  |  33 |    1  |    0   |   0|
|9|   53       |    0|     3  |  46|     1   |   0   |   0|

I define matrix $X$ and $y$ as follows:

```{Python}
regressor_list = ['ones','prog1','prog2','math']

X = data[regressor_list].values
y = data.values[:,1]
```

I now define four functions, respectively, for $\lambda _i$, $LL(\beta)$, $\dfrac{\partial L(\beta )}{\partial \beta }$, and $\dfrac{\partial ^2 L(\beta )}{\partial \beta \partial \beta '}$:

```{Python}
# For Lambda_i
def poisson_lambda(x,beta): return np.exp(-np.dot(x, beta))

# Log-likelihood function
def LL(x, y, beta): 
    return np.sum(-poisson_lambda(x, beta) + y * np.dot(x, beta) - np.log(factorial(y)))

# Gradient
def LL_D(x, y, beta):
    return np.dot((y-poisson_lambda(x, beta)), x)

# Hessian 
def LL_D2(x, y, beta):
    return -np.dot(np.transpose(poisson_lambda(x,beta).reshape(-1,1) * x), x)
```
Implementing Newton's algorithm described above:

```{Python}
max_iter = 100 # Maximum number of iterations
beta = np.array([0,0,0,0]) # Initial guess for beta vector

max_iter = 100
beta = np.array([0,0,0,0])
for i in range(max_iter):
    beta_new = beta + np.dot(np.linalg.inv(LL_D2(X,y,beta)), LL_D(X,y,beta)) # Newton's update rule
    print(beta_new)

    # stopping criteria
    if np.sum(np.absolute(beta-beta_new)) < 1e-12:
        # Update beta
        beta = beta_new
        break
    beta = beta_new
```

The result:

```
[ 2.98299806  0.2125061  -0.26610685 -0.0478888 ]
[ 4.36817904  0.30562538 -0.53865713 -0.06496803]
[ 4.83114677  0.35664338 -0.6906736  -0.06979827]
[ 4.8767946   0.36938328 -0.71369546 -0.07014979]
[ 4.87731508  0.36980898 -0.71404984 -0.0701524 ]
[ 4.87731517  0.36980923 -0.71404992 -0.0701524 ]
[ 4.87731517  0.36980923 -0.71404992 -0.0701524 ]
```

Solution is:

```{Python}
solution = pd.DataFrame(beta.reshape(1,-1), columns=regressor_list)
solution
```

||    ones | prog1|  prog2|  math  |
|--|    ------ | ----|  --|  --  |
| 0 | 4.877315|  0.369809| -0.71405| -0.070152|

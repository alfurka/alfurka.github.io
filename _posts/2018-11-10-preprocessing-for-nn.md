---
layout: post
title: Preprocessing for Neural Networks - Normalization Techniques
subtitle: Scaling, standardization, and so on
tags: [machine learning, neural networks, preprocessing, standardization, normalization]
image: /img/brain-neural.png
bigimg: /img/brain-background.jpg
---

I mentioned about a critical preprocessing tool for [Lasso](https://alfurka.github.io/2018-11-06-preprocessing-for-lasso/) in my last post. Today I will write about preprocessing for Neural Networks. 

Today, it took me three hours to understand why my neural network model sometimes diverges. And sometimes it fits well. However, it is impossible to use that model in such case. Because, model requires a new training in each new data arrival and it needs to start from the beginning. Generally, I normalize my data before training a neural network model but I forget it today which is a very important preprocessing step for neural networks.

The neural network models contain too many weights. If inputs contains data with different scales it may diverge your model as in my case. Even if it does not diverges, it can overestimate, underestimate, or ignore some parameters; and thus it decreases efficiency of your estimation. 

Therefore, among the others, normalization is one of the most important preprocessing tool for neural networks. So how to normalize the data?

#### Min-Max Scaling

One of the commonly used techniques is using min-max scaling. It is very straightforward: 
$$
X_{i} ^{S} = \dfrac{X_i - X_{min}}{X_{max} - X_{min}}
$$
Note that Min-Max scaling is very sensitive to the outliers. 

#### Decimal Scaling

Your data may contain a variable with very extreme values like `house prices` . Its weight is likely to diverge during stochastic gradient descent. If such values are not frequent you can simply apply decimal scaling by dividing it, say, $ 1e4 $. 

#### Eliminating Outliers

It might be very efficient if you eliminate the outliers with or without using other normalization techniques. I mean really `outliers`, do not drop $1\%$ quantiles from the beginning. 

#### Z-score normalization or Standardization

It is one of the most common standardization technique. You find the z-scores of your variables on their own distribution. 
$$
X_i ^S = (X_i - mean(X_i)) * std(X_i)
$$
However, it is efficient only if your data is Gaussian-like distributed. It is also sensitive to the outliers. 

#### Mean / Median Absolute Deviation

It is insensitive to the outliers but does not contain the input variances as raw data is. It also means it can be used as a [data augmentation](https://www.google.com.au/search?q=data+augmentation&oq=data+augmentation&aqs=chrome..69i57j0l5.2309j0j7&sourceid=chrome&ie=UTF-8). But it does not increase the number of observations, only increases the number of variables. 
$$
X_i ^S = \dfrac{X_i - mean(X_i)}{N} \qquad \text{or} \qquad  X_i ^S = \dfrac{X_i - median(X_i)}{N}
$$

It also resembles the using [polynomial feature](https://alfurka.github.io/2018-11-06-preprocessing-for-lasso/) parameters. 

#### (Modified) Tanh Estimator

Tanh estimators are considered to be more efficient and robust normalization technique. It is not sensitive to outliers and it also converges faster than Z-score normalization. It yields values between -1 and 1 ($X_i \in [-1, 1]$). 
$$
X_i ^S = 0.5 * tanh \Big [  0.01 * \dfrac{X_i - mean(X_i)}{std(X_i)} \Big]
$$

#### Max-Scaling

It resembles the min-max scaling but it is more efficient. 
$$
X_i ^S = X_i / max(X_i)
$$
*Which one should we use?*

This question really depends on your data and model. On the other hand the last two are more appropriate in general than others. You should also consider the eliminating outliers before training and s[ee further discussion here](https://research.ijcaonline.org/volume32/number10/pxc3875530.pdf).


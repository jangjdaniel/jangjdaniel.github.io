---
layout: post
title: Ridge and LASSO Regression
date: 2025-12-20
description: Extremely Non-rigorous
tags: methods regression
categories: Statistics
---

One thing I felt that was missing from my undergraduate training was a comprehensive look at machine learning / high-dimensional statistics. I often confused the two together, which in hindsight is silly of me (machine learning concerns precise predictions; high-dimensional statistics is statistics with a lot of variables and how to deal with that, especially when sample sizes may prevent regressions from converging). Right now, I am going through some of the more popular methods for supervised learning (where we have access to an outcome variable $Y$) that have connections to high-dimensional statistics. Hopefully this will serve as a foundation for things I explore in the future.

## Machine Learning Notation

Everything boils down to $Y = f(X) + \epsilon$, where Y is our qualitative response, X is the vector of predictors of length p, and f is some fixed, unknown function of our vector of predictors. The error term is independent of X and has a mean of zero. In machine learning, we are trying to find the best estimated function $\hat{f}$ to gain good predictions $\hat{Y}$. We are not really concerned with the form of $\hat{f}$ as long as it gets the job done. This is unlike the "inference paradigm", which I am more used to from doing work in missing data and clinical trials.

There are **two types of error**: irreducible and reducible. This is self-explanatory (at least to me): where the error term represents irreducible errors, potentially from unobserved data or something truly random. 

There is a **tradeoff between model complexity (flexibility) and interpretability**. Since we are not concerned with the form of $\hat{f}$, we have something called a black box, where it is either infeasible or impossible to understand what exactly is going on inside the hood. 

**Bias-Variance Tradeoff**: Expected test MSE can be decomposed into the variance of $\hat{f}$, the squared bias of $\hat{f}$, and the variance of the error term. More flexible methods have a higher variance but usually lower bias. 

## Cross-Validation

I know the most basic form of this is training and testing data, where we first fit a model on some proportion of the data, then we see the accuracy of the predictions on data the model has not encountered. I know that there are more techniques than simply partitioning the data (e.g. 80% training / 20% testing).

```python
#this is the scikit-learn package, which is extensive. we only want one part of it which has the function we want

import pandas as pd
from sklearn import model_selection as ms 

# load in the data
df = pd.read_csv("data.csv")

# training and test split: make sure data only has features X (predictors) and a specified outcome y
# this produces four datasets. of course you should rename to other than X and y
X_train, X_test, y_train, y_test = ms.train_test_split(X, y, test_size=0.2, random_state=42) #like setting seed (80/20 split)
```

What is important for these regressions in machine learning is **k-fold cross validation**, which means we are trying to extract the best **hyperparameter** to use in our model. I will describe what these are in practice, which will hopefully make sense. The idea with k-fold cross validation is that on our training data, we want to see which hyperparameter performs the best overall (you first need a set of hyperparameter values that you think are realistic to use, modern packages do this automatically for you, doing a ton of values and interpolating to fill in the gaps) by partitioning the training data into $k$ equal groups, training the model on $k-1$ groups with all the different hyperparameters, testing the model on the remaining group, and calculating a validation error for each hyperparameter value (in regression it's usually the MSE). This is repeated so that each partition is the testing group once. Then the average of the validation error for each hyperparameter value is found (known as cross-validation error, or CV), and the lowest one is our "best" hyperparameter. Packages will often interpolate between two of the lowest CVs to get a very specific hyperparameter value. After this hyperparameter value is extracted, it is used to specify the regression for the full training dataset, then we check it with the testing dataset as normal to ensure we have good predictions.

As a side thought, I'm curious how multivariate analyses work where we have multiple things to be predicted.

---
## Regularization and Loss Functions

I like the definition of regularization as "convert an 'answer to a question' to a simpler one". The answer to our question comes in the form of our predictors and the answer is the outcome variable. We want to make the predictors simpler to avoid overfitting. Thank goodness I remember what overfitting is from my intermediate statistics course. To do so, we need to do something with a loss function to "penalize" the model. This concept always confused me because there was no concrete example to illustrate with what I knew. So for an example, I looked at in classical linear regression, the loss function is the sum of squared errors, or mean square error. What we want to do is minimize this error to get the most accurate model. What I didn't know is that you can use different loss functions. I'm not entirely sure what the consequence of this is in terms of parameter estimation or the interpretation of parameters. 

### Connection to $L_p$ norms from Measure Theory (flashbacks)

I need to explicitly write this down to avoid my confusion. $L_p$ norms describe a function space (i.e. a function in $L_1$ norm means that the function is integrable), whereas $\ell_p$ norms describe the geometry of distance. The reason why they have almost identical names is because they follow a similar concept of raising each element to the power of p and taking the pth-root of the sum of all those elements. For example, for distances of an element $x = (x_1,\dots,x_n) \in \mathbb{R}^n$, we typically think of the Euclidean norm $\|x\|_2 = (x_1^2 + \cdots + x_n^2)^{1/2}$. To generalize this notion, for $p \ge 1$ we define $\|x\|_p = (x_1^p + \cdots + x_n^p)^{1/p}$. For example, the $\ell_1$ norm describes taxicab geometry (high school flashbacks: it looks like a square). The idea is that each value of $p$ induces a different geometry on $\mathbb{R}^n$, with $\ell_2$ being the most familiar to us and $\ell_1$ somewhat less so.

---
## The goal of these regressions

The idea with both Ridge and LASSO is we add a penalty to the original regression equation to dampen the effect of certain variables, especially when two variables are highly correlated with one another or if there is a very weak / nonexistent relationship between the variable and the outcome. For linear regression, the best linear unbiased estimator (BLUE) or the minimum variance unbiased estimator (MVUE) is ordinary least squares (OLS) because of the Gauss-Markov Theorem. In the aforementioned cases, however, the variance will be unnecessarily huge. Even if in the long run we have an unbiased estimator, it is too unstable to be reliable in a statistical analysis (or for a machine learning model). As a refresher, we are trying to minimize the following:

$$\hat{\vec{\beta}}_{OLS} = \min_{\vec{\beta}} \|y - X\beta\|_2^2 = (\textbf{X}^\top \textbf{X})^{-1} \textbf{X}^\top \vec{y}$$ (note that this notation states that we are in $\ell_2$ norm)

Instead, what we can do is look at some slightly biased estimators that have drastically lower variance! In fact, the amount of reduced variance can make up for the fact that we bias the estimators slightly to the point where our MSE for ridge or lasso is less than our MSE for OLS. The question becomes "how much bias are we willing to take to reduce variance so we have more "consistent" results?"


## Ridge Regression ($\ell_2$ Regularization)

The most common use case of ridge regression is when we know that all variables are important and should remain. The math behind ridge regression is not terribly complicated. It is very similar to the loss function in OLS, but we add a constraint as follows: 

$$\min_\beta \|{y} - {X}{\beta}\|_2^2 \quad \text{s.t.} \quad \|{\beta}\|_2^2 \le c$$

What is this $c$? It relates to the geometrical visualizations of ridge regression, aka the radius of the ball. So on the graphs we see, we have $\beta_0$ on the x-axis and $\beta_1$ on the y-axis as a 2-tuple for $\mathbf{\beta}$ (realistically, there should be at least three dimensions, but this is just to build our intuition). The constraint is a circle, because that is the $\ell_2$ norm. What we are saying is that given the contour plot of $(\beta_0, \beta_1)$, which values minimize the output while respecting the constraint. In other words, we are trying to find a minimum subject to a constraint in a multivariable setting... sound familiar? It's Lagrange Multipliers! We can set up the Lagrangian as the following for some $\lambda \ge 0$:

$$ \|y - X\beta\|_2^2 + \lambda \|\beta\|_2^2 - c $$

What we care most about is the betas, so to get our ridge coefficient vector, we do the following, where $p$ is the number of predictors (notice that the $\arg \min$ gets rid of that constant $c$)

$$\hat{\mathbf{\beta}}_{\text{ridge}} = \arg\min_\beta (\|y - X\beta\|_2^2 + \lambda \|\beta\|_2^2) = (X^\top X + \lambda I_p)^{-1} X^\top y$$

Different $\lambda$ values give different radii $c$, which affect the values that our coefficients can take on. Note that $\lambda$ values have an inverse relationship with the radius $c$. This is why higher $\lambda$ values make ridge regression coefficients approach zero, since the constraint's radius becomes smaller. Conversely, when $\lambda = 0$, we have our original OLS equation, as our circle covers the entire space, so any value is possible. 

How do we choose the best $\lambda$ for our data? It's important to note that $\lambda$ is known as a **hyperparameter**, which means that we are not estimating it as a function of the data like we do for parameters (like in mathematical statistics). We are choosing it "externally" through cross validation. This is where k-fold cross validation comes into play, and as previously stated, this will give us the best value to use in our machine learning model. 

Now that we have all our parameters and hyperparameters specified from the training dataset, we validate with the testing dataset and then we're done. Interpretations vary, but in the prediction paradigm, the details of the function of our data $\hat{f}(X)$ don't matter. In the inference paradigm, I think people identify which variables shrunk a lot. It can't really be interpreted like an OLS coefficient. I guess if I ever explore high-dimensional statistics I will become quite familiar with how people interpret ridge regression, but that's not my main focus today.

Lastly, I want to note that there is a ton of linear algebra going on in the background. I am currently reviewing linear algebra for the comprehensive exam at Amherst, so I am rusty. I also assume that if I need this in the future, I can pick it up easily. Anyway, if we have $p$ predictors and a sample size of $n$, **ridge regression can actually converge when $p > n$**. This is because $X^\top X + \lambda I_p$ is always invertible when $\lambda$ is strictly greater than zero. This will also be true in LASSO. I definitely need to review my linear to be fully convinced why, but it doesn't seem too difficult to do so in the future.

**Example**: Consider the following toy example with OLS:

$$Y = 0.5 + 0.3*x_1 + 0.6*x_2 + \epsilon$$

where $x_1$ and $x_2$ are highly correlated. This means that the nondiagonals of the matrix $X^\top X$ will be pretty high, whereas the diagonals are exactly 1 (a variable $x$ is perfectly correlated with itself). The hyperparameter adds a certain value to the diagonals, so mathematically when we calculate $\hat{\beta}_{\text{ridge}}$, all effects are dampened to some extent. I also realized that I would need to brush up on linear models to fully internalize what's going on under the hood, but I don't have time for that right now.
__________

### Python and R stuff here to run code

Note that in python, the $\lambda$ hyperparameter is called $\alpha$

```python
import numpy as np
from sklearn.linear_model import Ridge
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error, r2_score
from sklearn.datasets import make_regression

# Pretend we have all the data from the previous python chunk

# Tune the hyperparameters (realistically need to check a lot more than this)
alpha_value = 1.0
ridge_model = Ridge(alpha=alpha_value)

# Fit the model
ridge_model.fit(X_train, y_train)
 
# Make predictions on the test data
y_pred = ridge_model.predict(X_test)

# Evaluate the model
mse = mean_squared_error(y_test, y_pred)

```

If you want the k-fold cross validation built in, here's the documentation: https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.RidgeCV.html.

In R, it looks like:

```R
library("glmnet")
```

---
## LASSO (Least Absolute Shrinkage and Selection Operator) Regression ($\ell_1$ Regularization)

LASSO is quite similar to Ridge Regression, but instead of the $\ell_2$ norm, we use the $\ell_1$ norm as the loss function (taxicab geometry), so we have (I NEED TO CHECK THIS)

$$\min_\beta \|y - X\beta\|_2^2 \quad \text{s.t.} \quad \|\beta\|_1 \le c$$

This loss function is unique to all other $\ell_p$ norms because of non-differentiability. I think that’s related to why it can send some parameters directly to zero, and how this version can automatically do variable selection, which is dimension reduction.

However, the model is said to **saturate** at the sample size $n$, which I think means that $f(X)$ will contain at most $n$ features out of the $p$ available predictors.

### Python and R stuff here to run code

```python
from sklearn import model_selection as ms 
```

```R
library("glmnet")
```

## Elastic Net (best of both worlds)

Elastic net combines ridge and lasso using another hyperparameter $\alpha$, called the mixing parameter, which weights the strength of $\ell_1$ or $\ell_2$ regularization in the regression model. For time-saving sakes, I am not going to write it out, but basically we add the $\ell_1$ and $\ell_2$ penalties to the original loss function, then weight each with $\alpha$ and $1-\alpha$. I assume we need to do more cross-validation to get this hyperparameter. 

Also from a cursory reading of material online, it seems that this has the most applications, which makes sense because it combines the best of both worlds without sacrificing much from choosing ridge or LASSO. I think the main appeal is that the ridge part makes the LASSO part more stable. Anyways, if I end up needing to use this in the future, I can do some more digging, but I need to move onto other things now.
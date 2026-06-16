---
layout: post
title: Firth's Penalized Logistic Regression
date: 2026-04-04
description: Methods from the 1990s
tags: methods regression
categories: Statistics


title: ""
date: 2026-04-04
layout: single
mathjax: true

---

The motivation for why I'm learning about Firth's Penalized Logistic Regression is because it's deals with rare events better than classic logistic regression (it also deals with complete or quasi-separation where one variable perfectly or nearly perfectly explains or is related to another). This is in context of censoring weighting over time, where the number of patients who are censored at each time point is proportionally pretty small (between 10 to 100 out of 1,400). I want to make sure our models have unbiased estimations at these sample sizes so we can do some proper prediction

## Penalization in the Context of Logistic Regression

The usual log-likelihood of logistic regression looks like:

$$\ell(\beta) = \sum_{i=1}^n \left[ y_i \log(p_i) + (1 - y_i)\log(1 - p_i) \right]$$

where 

$$p_i = \frac{1}{1 + \exp(-x_i^\top \beta)}$$

But we add a penalty term (apparently some people don't like calling this a penalty term?) based on the fisher information.

$$\ell^*(\beta) = \ell(\beta) + \frac{1}{2} \log \left| I(\beta) \right|$$

Basically when we have lower fisher information, we have less precision in our estimate (think fisher's inequality: lower bound of the variance of an estimator). This means the penalty term increases. 

I don't know all the details on why this works mathematically because it requires asymptotic theory. My colloquial understanding is that the bias of the MLE has a $$\frac{1}{n}$$ term in it. As $$n \rightarrow \infty$$, this term approaches $$0$$, but we can't guarantee that for finite samples, especially smaller samples. Thus the penalization properly removes that $$\frac{1}{n}$$ term in the MLE so that finite samples are unbiased. I don't really know how this means the model deals with complete or quasi-separation, but that's a thought for 2-3 years in the future, if at all.
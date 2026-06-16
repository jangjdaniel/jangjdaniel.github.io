---

title: "Bayesian Regression"
date: 2026-04-24
layout: single
mathjax: true

---

The motivation for this post is because I want to test different regression penalties, and there is a family of methods that is motivated from finding an alternative prior to the jeffrey's prior (equivalent to firth's penalty) in logistic regression to handle rare events and complete / quasi separation.

I know the basics of bayesian statistics from a unit in my undergraduate mathematical statistics class. The idea is we let parameters be random variables and we fix the observed data. I guess this high-level interpretation confuses me because at the point of analysis, don't we fix the data to what we have? I'm probably reading too much into this and should focus on the different marginal distributions each is trying to target for credible or confidence intervals (parameter given X v.s. X given parameter). I also had a great analogy for hypothesis testing between frequentists and bayesians, but I honestly forgot it... I'll circle back in the future.

Regardless, we set a prior distribution for the parameters, and the data adjusts the distribution to give us the posterior distribution. I am adding some notation from my class:

The posterior distribution/density of a parameter $$\theta$$ given a vector of our data $$X$$, denoted as $$g^{*} (\theta \mid X)$$ is proportional to $$Pr(X \mid \theta) \, g(\theta)$$, where $$g$$ is the prior distribution and $$Pr(X \mid \theta) = L(\theta \mid X)$$ is the likelihood function (I think we only need proportion to get the correct shape of the posterior, then we can normalize it to 1 without having to calculate the complicated marginal density). This was derived from Bayes theorem to target a flipped conditional probability from frequentist statistics: Likelihood of X given the (oftentimes unknown) parameter value.  

# Logistic Regression

Using an uninformative prior is analogous to doing a frequentist analysis (since the posterior will be proportional to just the likelihood of the data given the parameter). In the context of logistic regression, having an uninformative prior on the parameters $$\beta_0$$ and $$\beta_1$$ and obtaining the maximum a posteriori (MAP) point estimate (e.g. the mode of the distribution) yields the same value as ML Estimation (since this is also finding the mode of the likelihood distribution). Of course the interpretations will differ since just because the values of the two marginal distributions are the same does not mean they're equivalent structures.

The likelihood function for logistic regression is given as:

$$L(\beta)
= \prod_{i=1}^n \left(\frac{\exp(x_i^\top \beta)}{1 + \exp(x_i^\top \beta)}\right)^{y_i}
\left(\frac{1}{1 + \exp(x_i^\top \beta)}\right)^{1 - y_i}$$

# logF(m,m)

We can set priors: $$\beta_0 \sim logF(m,m)$$ and $$\beta_1 \sim logF(m,m)$$. Along with the likelihood function, we have all tools to calculate the posterior distribution (most likely through approximation methods, as I highly doubt anyone would want to solve this analytically). Apparently simulations showed that $$m = 1$$ performs well and has similar properties to the Jeffrey's prior, although for certain extreme situations, stronger priors should be chosen? I find this all very arbitrary, but I also awknowledge that my knowledge in this subject matter is limited. Who knows, maybe one day this won't feel arbitrary to me anymore.

---

title: "MCMC for Approximating Distributions"
date: 2026-06-07
layout: single
mathjax: true
private: true

---

This is a quick blog post reviewing the metropolis-hastings algorithm and the gibbs sampler, as well as an intuitive understanding on how they work. I just want to get an intuition behind some of the algorithms I take for granted in monte-carlo simulations.

# Metropolis Hastings Algorithm

The posterior can be simulated with the metropolis-hastings algorithm, which is a markov-chain monte-carlo method (something only depends on the most recent thing). Thankfully I did a project where I had to recreate that algorithm in R, so if I needed to do it by hand, I could. I'm just going to copy down what I wrote back then here for ease of finding it in the future, which will be unedited, out of context, and riddled with mistakes. 

----

In our example, we have a beta prior and multinomial data. This is not a conjugate prior result we are familiar with, so how do we get our posterior so we can do our bayesian analysis? Well we can use one of the most popular Markov Chain Monte Carlo (MCMC) Algorithms: The Metropolis Hastings Algorithm. Let’s do a toy example related to the aspirin data. Consider a beta prior with alpha = 3 and beta = 4, and data with a binomial distribution with p = 0.4 and n = 50. Let’s multiply these terms (without coefficients) for the target function. However, we will go through this algorithm without assuming we know the beta/binomial conjugate prior result, and show that the algorithm gives us a histogram that closely approximates the posterior we expect.

Monte Carlo is similar to the simulations we have done before. We take a distribution, specify a parameter, and take many random samples to build a new distribution. Markov Chain is like Monte Carlo, but it is kind of like recursion, where the next sample taken is dependent on the parameter previously chosen. A combination of both Markov Chains and Monte Carlo gives us MCMC algorithms that have wide applications, especially in statistics. 

```r
target <- function(x) { #creating our target distribution
  x^(3-1)*(1-x)^(4-1) * x^20*(1-x)^(50-20)
}
```

The bulk of the algorithm happens in the following function. We propose an uninformative distribution (flat) because we are assuming we know nothing about the posterior distribution. 

We draw one value of x from both the target and proposal (proposed_x) distribution. Then, comparing the ratios, if it is greater than 1, we automatically keep the new value of x. If our proposal distribution has a much higher value than our target, we might reject the value of x being in our distribution, depending on how much bigger the proposal was from the target. If a randomly drawn number from a uniform(0,1) distribution is less than the ratio, then we accept the new value. Otherwise, we reject the value and use our old parameter value for the next iteration. The proposal distribution doesn't move in that case. There is a proof behind why this works, but it is omitted.

```r
metropolis_step <- function(x, sigma) {
  proposed_x <- rbeta(1, shape1 = 1, shape2 = 1) #uninformative! equivalent to a uniform(0,1)
  accept_prob <- min(1, target(proposed_x) / target(x))
  u <- runif(1)
  if(u <= accept_prob) {
    value <- proposed_x
    accepted <- TRUE
  } else {
    value <- x
    accepted <- FALSE
  }
  out <- data.frame(value = value, accepted = accepted) #only accepted values allowed
  out
}
```

There are other important considerations that mitigate common issues with the algorithm. Mainly, I was concerned with the initial sample being too autocorrelated, which is a common issue for Metropolis-Hastings. To solve this issue, the below code removes the first 500 observations. We still sample 2500, as to prevent a sample size issue.

```r
#does certain conditions such as burnin, lag, and the variance

metropolis_sampler <- function(initial_value, 
                               n = 2500, 
                               sigma = 1, 
                               burnin = 500, #burn in deletes the first n samples: helps with issue
                               lag = 5) { 
  
  results <- list()
  current_state <- initial_value
  for(i in 1:burnin) {
    out <- metropolis_step(current_state, sigma)
    current_state <- out$value
  }
  for(i in 1:n) {
    for(j in 1:lag) {
      out <- metropolis_step(current_state, sigma)
      current_state <- out$value
    }
    results[[i]] <- out
  }
  results <- do.call(rbind, results)
  results
}
```

Lastly, we can overlay the conjugate prior result (which is a beta(23,34) distribution) to verify that the metropolis-hastings algorithm really did approximate the correct distribution. As we can see, the metropolis-hastings algorithm works! This can be extended to posteriors that are not conjugate priors, like the aspirin example!

```r
set.seed(1234)
out <- metropolis_sampler(initial_value = 0)

#building pdf beta
x <- seq(0, 1, length.out = 10000)
pdf <- dbeta(x, shape1 = 23, shape2 = 34)

#plot histogram of sampled values
hist(out$value, freq = FALSE, main = "Histogram with Overlay",
     xlab = "Sampled Values", ylab = "Density")

#overlay Beta distribution PDF
lines(x, pdf, col = "blue", lwd = 2)
```


# Gibbs Sampler
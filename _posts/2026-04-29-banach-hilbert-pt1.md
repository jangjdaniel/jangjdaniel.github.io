---
layout: post
title: Banach and Hilbert Spaces (Part 1)
date: 2026-04-29
description: More functional analysis!
tags: theory
categories: Mathematics
---

Unfortunately, I am unsure how to motivate a lot of these results from Banach and Hilbert spaces. I think one day I should try out the proofs myself (especially of the representation theorem), but I think this is sufficient for right now.

# Completeness and Definitions

There were specific types of metric spaces we disucssed: complete metric spaces (all cauchy sequences converge to an element in the set) and compact metric spaces (every open cover has a finite subcover). There's also connected metric space (cannot be partitioned into two nonempty disjoint open subsets, i.e. the only open and closed sets are the empty set and the set itself).

In terms of normed vector spaces, they are always connected (something about a vector space being a convex set) and never compact (infinite-dimension issues). However, they need not be complete. Completeness is a mathematically nice property because it allows us to discuss convergence within the same set. This is why complete normed vector spaces get their own special name... you guessed it!

Banach spaces are normed vector spaces that are complete w.r.t the norm, and Hilbert spaces are inner product spaces that are complete w.r.t the inner product. In statistics, hilbert spaces are more important since we want a notion of an angle along with a notion of distance with elements that follow the axioms of vector spaces. I think we can think about correlation between two random variables via an inner product, and besides, we can induce a norm if needed.

I am not going to prove much about these spaces. Rather, I want to understand at a high level why they are so important and some examples in statistics that I may need in the future. 

# Examples of Banach and Hilbert Spaces

$$\mathbb{R}^n$$ with euclidean norm is a finite-dimensional banach space. With the dot product we know from multivariable calculus (discrete version), it's a hilbert space, so it is also a banach space. However, this is not that interesting.

In general, the lebesgue space $$L_p$$ is the set of all measurable functions (depends on the measure space we are in...) with a finite p-th moment (using probability terms), i.e. $$ \int | f |^p \, d\mu < \infty $$. This is a set of functions (infinite-dimensional)!

$$L_2$$ is a space of square-integrable functions (which means it has a finite second moment in probability terms... variance!). We use the definition of the $$l_p$$ norm on this function space to create a banach space (and also the idea of convergence in $$l_2$$ norm). Specifically for $$L_2$$, this is also a hilbert space. The inner product is defined as $$<f,g> = \int f(x)g(x) \, dx$$ (a continuous analog of the discrete dot product).

## Parallelogram Law: Why L_p spaces are not Hilbert Spaces except when p = 2

The parallelogram law (like in multivariable calc) is: $$\| x+y \| ^2 + \| x-y \| ^2 = 2 \|x\|^2 + 2 \|y\|^2  $$. By the Jordan-von Neumann theorem, an inner product exists if and only if the norm satisfies the parallelogram law. To keep things at a high level, I won't prove this here. 

Any norms in $$L_p$$ where $$p \neq 2$$ will not satisfy this law for all functions. For example, take $$f = \chi _{[0, 1]}$$ and $$g = \chi _{(1, 2]}$$. $$\|f\|_p = (\int_{\mathbb{R}} f^p \, dm)^{\frac{1}{p}} = (1)^{\frac{1}{p}}$$, and similarly, $$\|g\|_p = (1)^{\frac{1}{p}}$$. Notice that $$\|f + g \|_p = \|f - g \|_p = 2$$, so the only case where the parallelogram law holds is when $$p = 2$$.

# Why are norms in finite-dimensional spaces equivalent?

For any two norms in a finite-dimensional space $$\| \cdot \|_a$$ and $$\| \cdot \|_b$$, there exists constants $$C, K$$ such that $$C \| \cdot \|_a \leq \| \cdot \|_b  \leq D \| \cdot \|_a $$. Norms that are equivalent induce the same topology (I don't really care about topology, but you can define a topology using metrics (somehow), just like you can define a metric with norms). I'm just going to take the math god's word for it.

# Linear Operators, Operator Norm, and Dual Spaces

We need to be careful thinking about linear transformations (which we are calling linear operators. Some places make a distincition where linear operators have the same domain and codomain) for vector spaces of infinite dimension. The reason we didn't talk about "boundedness" in linear algebra are two-fold. Firstly, we only really worked with finite-dimension vector spaces, and there is a theorem that states any linear transformation between finite-dimension vector spaces are bounded. Second, we didn't even have a notion of norm (aka length of vector). So we never worried about $$\| T(v) \| = \infty$$ for all $$v \in V$$.

However, this is not necessarily true for infinite-dimensional vector spaces. Consider the following linear transformation between two normed vector spaces $$X, Y$$:

$$T: X \rightarrow Y$$

We use the classic idea of boundedness, i.e. there exists some number $$M > 0$$ such that $$\| T(v) \| \leq M \| v \|$$ for all $$v \in V$$. The smallest such $$M$$ is called the operator norm of $$T$$, and is denoted as $$ \| T \|$$.

A good way to verify if a linear operator is bounded is iff it is continuous (the epsilon delta definition but with metrics... learned this briefly in measure theory for some reason).

Operator theory studies these linear operators between function spaces, which is a bit more general than what we need (since we are in the realm of Banach and Hilbert spaces)

The dual space is the set of linear functionals on a vector space $$V$$. We denote this as $$V'$$, so this is just a set of functionals. Furthermore, it's useful to talk about the continuous dual space (only continuous functionals) for this bounded property.
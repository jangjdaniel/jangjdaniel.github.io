---
layout: post
title: Optimal Transportation
date: 2026-03-29
description: A culmination of my math journey
tags: theory
categories: Mathematics
---

## Motivation

These are my personal notes adapted from my final project in differential geometry. I want to motivate the Monge and Kantorovich problems in notation that I understand so that I have a reference for myself if I ever need these tools (although I doubt I will need it)

## Background

One needs to know the concept of abstract measures, what a measurable space is, abstract integration, and absolute continuity (radon-nikodym derivatives). I don't have a background in calculus of variations, functional analysis, or optimization (I will ignore why being convex makes it easier to solve), so hopefully I will be able to get through this without those tools. 

I will introduce some definitions beyond this, like the pushforward measure, product spaces, and couplings, since these are less familiar definitions. 

## The Monge Formulation

Gaspard Monge asked the following problem in 1781: If I have a pile of dirt and want to move it into a mound, what's the most optimal way to bring each little bit of dirt into the mound, i.e. what's the minimal amount of work I need to do? I will define this problem in the realm of probability spaces.

Let $$(X, \mathscr{B})$$ be a measurable space and denote $$\mathcal{P}(X)$$ the set of all probability measures defined on $$(X, \mathscr{B})$$. For a probability measure $$\mu \in \mathcal{P}(X)$$ and for an element of the $$\sigma$$-algebra $$B \in \mathscr{B}$$, $$\mu(B)$$ represents the amount of mass (or dirt) is in the set $$B$$. The total mass of a probability distribution is $$1$$ by definition, so $$\forall \, \mu \in \mathcal{P}(X)$$, $$\mu(X) = 1$$. 

$$\textbf{Question 1:}$$ What does it mean to transport some mass from the pile of dirt to the mound?. We need a (measurable) (bijective) function $$T: X \rightarrow X$$ that moves the dirt pile $$x \in X$$ to the pile in location $$T(x) \in X$$. However, since we are moving from one distribution to another, we need a new measure to properly count the mass in the mound. How do we do that?

We define the $$\textbf{pushforward measure}$$: $$\forall B \in \mathscr{B}$$, the pushforward measure $$ T_{\#} \mu (B) = \mu(T^{-1}(B))$$. This preserves the mass of $$B$$ relative to the measure $$\mu$$ even after transforming $$B$$. The reason it is defined with $$T^{-1}$$ is to guarantee the existence of an inverse function. We define $$\nu \in \mathcal{P}(X)$$ to be the pushforward measure. Furthermore, this means that $$T$$ is called the $$\textbf{transport map}$$ from $$\mu$$ to $$\nu$$.

$$\textbf{Question 2:}$$ What is the amount of work that is done in transporting the amount of mass from the dirt to the mound? We define the cost function to do exactly this. Let $$c: X \times X \rightarrow [0, + \infty]$$ be the cost function, where $$c(x, T(x))$$ measures the cost from moving one element in $$x$$ to $$T(x)$$. Common cost functions are the $$L_1$$ and $$L_2$$ norm, although $$L_2$$ norm is more often used because it's easier to solve. However, we want to get the cost of moving all elements of $$X$$. This means we want integration with respect to our measure $$\mu$$, since that is the way we are measuring our masses. Thus, we are curious about the total cost, which we define as a functional (a map with a function as an input and a scalar as an output, in our case, the transport map is the input).

$$\mathbb{M}(T) = \int_{X} c(x, T(x))\, d\mu(x)$$

Now we can finally define the Monge problem as finding the infimum of the functional, which gives you the lowest cost out of all possible transport maps.

$$\inf_{T: X \rightarrow X}  \{ \mathbb{M}(T) \mid T_{\#} \mu = \nu \}$$

Now we can generalize this a bit more by going from one space $$X$$ to another space $$Y$$. The idea is still the same: we require the transport map with $$T_{\#}\mu \in \mathcal{P}(Y)$$, but we can define each $$y = T(x)$$ and the functional $$\mathbb{M}(T) = \int_{X \times Y} c(x, y)\, d\mu(x)$$, since we need to integrate over two different spaces (not sure if the details here are correct...)

This formulation of the optimal transportation problem is good for a number of applications, but there are two key limitations:

1. We are not able to split up a mass into two since the pushforward measure requires $$T$$ to be injective
2. Solutions may be not unique

There are plenty of canonical examples here which I won't really explain here. We will move to how these issues were addressed by Kantorovich.

## The Kantorovich Formulation

One motivation for Kantorovich's formulation is to address the splitting limitation in Monge's formulation. This will introduce some flexibility in optimally transporting all the mass from one region to another. Before we can do that, there is one important definition that a lot of notes seem to not touch on.

$$\textbf{Definition, Coupling:}$$ For a measurable space $$(X, \mathscr{B})$$, let $$\mu, \nu \in \mathcal{P}(X)$$ be two measures, where $$ \nu = T_{\#} \mu$$. The coupling of $$\mu$$ and $$\nu$$ is a (probability) measure $$\gamma \in \mathcal{P}(X \times X)$$ on the product space $$(X \times X, \mathscr{B} \times \mathscr{B})$$ such that the marginals of $$\gamma$$ coincide with $$\mu$$ and $$\nu$$. What I mean by that is: $$\gamma(A \times X) = \mu(A)$$ and $$\gamma(X \times A) = \nu(A)$$.

You can surmise that $$\forall B \in \mathscr{B}, \, \gamma(A \times B) \leq \gamma(A \times X) = \mu(A)$$. The proof really is trivial, since $$B \subseteq X$$. Now we can define the analogue to the transport map: 

$$\textbf{Definition, Transport Plan:}$$ A transport plan is a coupling $$\gamma \in \mathcal{P}(X \times X)$$ of $$\mu$$ and $$\nu$$ such that for a map $$\pi_i: X \times X \rightarrow X$$, where $$\pi_i(x_1, x_2) = x_i$$, the pushforward measures:

$$\pi_{1\#} \gamma = \mu$$ and $$\pi_{2\#} \gamma = \nu$$, i.e. $$\gamma(\pi_1^{-1}(B)) = \gamma(B \times X) = \mu (B)$$ and $$ \gamma(\pi_2^{-1}(B)) = \gamma(X \times B) = \nu (B)$$ .

The set of all transport plans from $$\mu$$ to $$\nu$$ is $$\Gamma(\mu, \nu)$$, and $$\mu$$ and $$\nu$$ are marginals of $$\gamma$$. 

We can interpret $$\gamma(A \times B)$$ as the amount of mass sent from $$A$$ to $$B$$, but a single mass from $$A$$ can be sent to many places in the mound. This is analogus to why we are allowed to split up the mass, since not all the mass is brought over (since $$\gamma(A \times B) \leq \mu(A)$$)

Now we finally define the Kantorovich formulation: define the following functional of the total cost of transport given some transport plan $$\gamma$$:

$$\mathbb{K}(\gamma) = \int_{X \times X} c(x_1, x_2)\, d\gamma(x_1, x_2)$$

and "minimize" (take the infimum) it once more to get the minimal cost of transport over all transport plans:

$$\inf_{\gamma \in \Gamma(\mu, \nu)}  \{ \mathbb{K}(\gamma) \mid T_{\#} \mu = \nu \}$$

The idea for two different sets $$X$$ and $$Y$$ is the exact same idea, but the cost function itself is something that needs to be thought of more. Having just one space allows us to use some sort of distance metric like $$l_1$$ or more commonly, $$l_2$$.

## Transport Map and Transport Plan Relationship

Given a transport map $$T: X \times X$$ with $$T_{\#} \mu = \nu$$, then the corresponding transport plan is $$(\text{id} \times T)_{\#} \mu \in \Gamma(\mu, \nu)$$. 

However, this need not be the most optimal transport plan. In fact, there is a theorem that states:

$$\inf_{\gamma \in \Gamma(\mu, \nu)}  \{ \mathbb{K}(\gamma) \mid T_{\#} \mu = \nu \} \leq \inf_{T: X \rightarrow X}  \{ \mathbb{M}(T) \mid T_{\#} \mu = \nu \}$$

I will not be proving this. You cannot make me.

## Wasserstein Metric / Earth Mover Distance

To be filled (suboptimally), maybe.
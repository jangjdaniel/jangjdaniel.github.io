---
layout: post
title: "My Intuition of the Radon-Nikodym Derivative"
date: 2025-12-07 14:24:00
description: Extremely Non-rigorous
tags: theory
categories: Mathematics
---

In calculus, derivatives are taught intuitively as the tangent line of a curve. This extended to the gradient $\nabla f$ in multivariable calculus. Even the real analysis definition was the same in calculus: $ \lim_{h \to 0} \frac{f(x+h) - f(x)}{h} $. Nothing too scary. Then the Radon-Nikodym derivative $f = \frac{d\mu}{dm}$ comes out of nowhere.

<div class="definition">
<strong>Definition:</strong>

Let $(X, \mathcal{M})$ be a measurable space. Let $\mu: \mathcal{M} \to \mathbb{R}$ be a signed finite measure and $m: \mathcal{M} \to [0, +\infty)$ be an unsigned $\sigma$-finite measure s.t. $\mu$ is absolutely continuous w.r.t $m$ (Notation: $\mu << m$). The <strong>Radon–Nikodym derivative</strong> of $\mu$ w.r.t $m$ is a measurable function $f: X \to \mathbb{R}$ s.t.

$$
\mu(A) = \int_A f \, dm \quad \forall A \in \mathcal{M}.
$$
</div>

How on earth does this generalize the classical derivative we have come to love? I feel like I should attempt to develop my intuition here and have the masses correct me if I got some details wrong.

---

<strong>Idea: </strong> Consider the function $F: \mathbb{R} \rightarrow \mathbb{R}$ in a measurable space $(\mathbb{R}, \mathcal{M})$. For examples sake, let $F(x) = x^2$. The derivative $F'(x)$ is of the form $ \lim_{h \to 0} \frac{F(x+h) - F(x)}{h} = 2x$, which is continuous. The radon-nikodym derivative on a set $A = [a,b]$ is given as 

$$
\mu([a, b]) = \int_{[a,b]} f(x) \, dm(x) = \int_{[a,b]} f(x) \, dx
$$

Notice that by the fundamental theorem of calculus, this integral is simply $F(b) - F(a)$. Thus, $\mu([a, b]) = F(b) - F(a)$. I don't think this is a fully kosher measure, but this is in terms of a continuous function $f$ in calculus land, so I think we're allowed to make simplifying assumptions here. This measure is also absolutely continuous w.r.t. lebesgue measure. Also notice that $\mu$ can take on negative values (not in this example because $x^2 \geq 0$). 

We can consider for each point $x \in [a,b]$ what the classical derivative looks like by looking at $ \mu([x, x+h]) = \int_{[x,x+h]} f(x) \, dm(x) $. Taking the limit as h approaches 0 gives us:

$$ 
\lim_{h \to 0} \frac{\mu([x, x+h])}{m([x, x+h])} 
= \lim_{h \to 0} \frac{F(x+h) - F(x)}{h} 
= F'(x)
$$

which is exactly the classical derivative definition. 

---

In general, the "rise-over-run" intuition breaks down with the Radon-Nikodym derivative. Rather it describes the density of $\mu$ w.r.t $m$. For my intuition, this means that how much mass of $\mu$ is there in one unit of $m$? In terms of measure theory, removing a null set does not affect the mass, which is where we can see RN derivatives shine where the classic derivative fails (although the applications in probability are slightly different and more broad). 

<strong>Example: </strong> Consider $f(x) = 2x \, \chi_{\mathbb{R} \setminus \mathbb{Q}}(x)$, where $\chi$ is the characteristic/indicator function. Let $A \subset \mathbb{R}$ be arbitrary in the same measurable space as the first example: $(\mathbb{R}, \mathcal{M})$. Thanks to the tools we developed in measure theory, we can easily decompose this integral by integration properties since the intersection between the rationals and irrationals is empty:

$$
\mu(A) = \int_A f(x) \, dm(x) 
= \int_{A \cap (\mathbb{R} \setminus \mathbb{Q})} f(x) \, dm(x) 
+ \int_{A \cap \mathbb{Q}} f(x) \, dm(x)
$$

Remember that we cannot use a FoTC result, so we must actually evaluate the integral if we want to know the "antiderivative" per-se. Recall that $A \cap \mathbb{Q}$ forms a null set w.r.t Lebesgue measure (the measure we're integrating with respect to). Thus, the second integral is just zero. We are left with:

$$ \mu(A) = \int_{A \cap (\mathbb{R} \setminus \mathbb{Q})} 2x \, \chi_{\mathbb{R} \setminus \mathbb{Q}}(x) \, dm(x) = x^2 \, \text{m-a.e.}$$ 

For example, with $A = [0,2]$, $\mu(A) = 4$ and $m(A) = 2$. With the derivative being simply $\frac{d\mu}{dm}(x) = f(x)$, we have that ratio/density captured. I know that this is improper notation, but please cut me some slack.

So basically, the derivative of $x^2$ is unique up to a null set. That is, the derivative of $x^2$ can be the function $2x$ without the rationals, or just $2x$, or $2x$ minus some point. The classical derivative could not do this! (This is definitely something that needs to be proved, and I think it's quite simple: just do the decomposition but store the entire null set in the second integral so that it goes to zero).

Now if we were to do the same thing with $g(x) = 2x \, \chi_{\mathbb{Q}}(x)$, the RN derivative would be 0, since the part with $2x$ mass is on a null set. Again, this goes back to how we do integration!

These derivatives have applications in probability theory, specifically when defining the PMF (integrate w.r.t the counting measure), doing changes in measure (not sure when this would become necessary), and some things in conditional expectation. Unlike with integration and learning some of the most important theorems (Monotone Convergence Theorem, Fatou's Lemma, Dominated Convergence Theorem), I can't really see how these derivatives are useful (beyond taking out a null set) until I take a probability course. However, hopefully I understand it a bit better now.

---

P.S. I almost surely left out details about integrability of these functions (something about the function being in $L^1$ norm), but my examples don't necessarily break down. I should be more careful next time, however. 

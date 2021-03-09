---
title: "The Real Induction"
author: "Matthew Wildrick Thomas"
layout: "post"
category: "blog"
tag: ["math", "induction"]
date: "2021-03-08"
use_math: true
---

Conventional proof by induction allows you to prove that a statement applies to an infinite sequence. The argument is that if a property holds on the first element, and always holds on the next element, then it must hold on all elements.

> But what do you do when there is no next element?

[A PDF I found online](https://arxiv.org/abs/1208.0973) showed me that there is an analogous version to proof by induction that can apply to uncountable sets like the Real numbers. Conventional induction cannot work on uncountable set because, by definition, you cannot reach all elements by iteratively stepping through them.

In the real induction, we get around this issue by breaking the space into intervals. Formally, let $$a < b$$ be real numbers. We want to show that $$[a, b] = S$$. That is, we want all elements of the interval to "satisfy" property $$S$$. We define the subset $$S \subseteq [a,b]$$ to be *inductive* if:

1. $$a \in S$$.
2. If $$a \leq x < b$$, then $$x \in S \implies [x,y] \subset S$$ for some 
$$y > x$$. 
3. If $$a < x \leq b$$ and $$[a,x) \subset S$$, then $$x \in S$$.  

The result is that a subset $$S \subset [a,b]$$ is inductive if and only if $$S = [a,b]$$.

# Example

As an example, we will apply this method to prove a lemma in my current working paper. The proof in the paper does not use this method, but the method can be applied nonetheless.

In this paper, we show that a particular candidate probability density is the distribution for the Nash equilibrium for a class of auctions. In this lemma, we want to prove that this candidate can be a probability density -- i.e. it's positive and integrates to one.

### Assumptions

Consider the following assumptions on two functions $$v: \mathbb{R}^2 \to \mathbb{R}$$ (the value of the prize) and $$c: \mathbb{R} \to \mathbb{R}$$ (the cost of bidding). In the auction context, these assumptions basically say that bids are costly (A2), the prize attracts bids that are greater than zero and less than infinity (A3), and you would prefer to win the prize than lose it (A4).

**Assumption 1** (A1, Smoothness)

The function $$v(s; y)$$ and $$c(s)$$ are continuously differentiable in $$s$$ and continuous in $$y$$ for all $$s$$ and $$y \leq s$$.

**Assumption 2** (A2, Monotonicity)

$$v'(s; y) < c'(s)$$

holds a.e., where we define $$v'(s; {y}) := \frac{\partial v(s; {y})}{\partial s}$$.

**Assumption 3** (A3, Interiority)

$$v(0, 0) > c(0) = 0$$ and $$\lim_{s \to \infty} \sup_{y \leq s} v(s; {y}) < \lim_{s \to \infty} c(s).$$


**Assumption 3** (A3, Positive value on the margin)

$$v(s; s) > 0$$

### Statement of the lemma

**Lemma (Probability density)** *The solution, $$g(s)$$, to*

$$g(s) = \frac{1}{v(s; s)} \left( c'(s) - \int_0^{s} v'(s , y) \\| g(y) \\| dy \right)$$

*is a probability density on some interval $$[0, \bar{s}]$$.*

### Proof

The finite definite integral cannot diverge because the function is continuous and $$g(0) = \frac{c'(0)}{v(0; 0)} > 0$$.
    
We still need to confirm that $$g(s) > 0$$ on the relevant interval $$ \{ s : \int_0^s \\| g(y) \\| dy \leq 1 \}$$. We will define the probability density to be zero outside this interval so that it integrates to one.

We show this by real induction on the interval $$[0,T]$$ where $$T$$ can be any value such that $$\int_0^T \\| g(y) \\| dy \leq 1$$. We must prove the following statements:

1. $$g(0) > 0$$.
2. For any $$s \in [0,T]$$, if $$g(y) > 0$$ for all $$y \in [0,s]$$, then there exists a $$z > s$$ such that $$g(y) > 0$$ for all $$y \in [s,z)$$.
3. For any $$s \in [0,T]$$, if $$g(y) > 0$$ for all $$y \in [0,s)$$, then $$g(s) > 0$$.

The first statement is true because $$g(0) = \frac{c'(0)}{v(0; 0)} > 0$$. The second statement proceeds by continuity of $$g$$ (A1). The third statement comes from:

$$g(s) = \frac{1}{v(s; s)} \left( c'(s) - \int_0^{s} v'(s , y) \\| g(y) \\| dy \right)$$

$$g(s) \geq \frac{1}{v(s; s)} \bigg[ c'(s) - \left| \max_{y\in[0,s]} v'(s; y) \right| \underbrace{\left( \int_0^{s} \\| g(y) \\| dy \right)}_{\leq 1} \bigg]$$

$$g(s) \geq \frac{1}{v(s; s)} \underbrace{\left[c'(s) - \left| \max_y v'(s; y) \right|  \right]}_{>0 \text{(A2)}} > 0.$$

Therefore, $$g(s) > 0$$ for any $$s$$ such that $$\int_0^s \\| g(y) \\| dy \leq 1$$. 

To complete the lemma, we must show that it is not possible for $$\int_0^\infty \\| g(y) \\| dy \leq 1$$. We can do this in one step with Holder's inequality.

$$c(s) = \int_0^s v(s; y) g (y) dy \leq \left(\int_0^s \\| g(y) \\| dy\right) \left( \max_{y \in [0,s]} v(s; y) \right)$$

so $$\int_0^s \\| g(y) \\| dy \geq \frac{c(s)}{\max v(s; y)}$$ which is assumed to be greater than one as $$s$$ approaches infinity (A3). By continuity, there exists an $$\bar{s}$$ such that $$\int_0^{\bar{s}} \\|g (y) \\| dy = 1$$ (A1).
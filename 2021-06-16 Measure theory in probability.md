# Why probability and statistics needs measure theory

<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

## Introduction to the problem

You may have encountered continuous probability distributions such as the normal distribution. It's often used to model things in the real world, and has nice statistical properties. You know the bell curve. But what you may not have seen is its formula: the *probability density function*,

$$f_\theta : \mathbb{R} \to \mathbb{R}, \quad x \mapsto \frac{1}{\sqrt{2\pi\sigma^2}}\exp\left(-\frac{(x - \mu)^2}{2\sigma^2}\right)$$

given a value of $$\theta = (\mu,\sigma^2) \in \Theta$$. In our case, $$\Theta = \mathbb{R} \times (0,\infty)$$; this is known as the *parameter space*. How does this relate to probability? Well, everything.

We begin informally. Suppose that $$X$$ is a continuous random variable with density $$f_\theta$$. The meaning of this is unimportant initially; think of $$X$$ as a "variable" that takes on values randomly (we will see that this is very much not a random variable *should* be). For a "reasonable" subset $$A \subseteq \mathbb{R}$$, let $$\mathbb{P}(X \in A)$$ denote the probability that $$X \in A$$, i.e. the probability of the event $$\{X \in A\}$$ (whatever that means). It turns out that, and as is often taught in a class in probability (even at high school level),

$$\mathbb P(X \in A) = \int_A f_\theta(x)\,dx = \int_A \frac{1}{\sqrt{2\pi\sigma^2}}\exp\left(-\frac{(x - \mu)^2}{2\sigma^2}\right)\,dx.$$

In fact, we are kind of working backwards. The probability density function is essentially defined to be a function such that integrating it over the desired subset yields the probability of $$X$$ taking on a value in that set. Now, what should $$A$$ be allowed to be? Should we allow *every* subset of the reals? In particular, if $$A = \{x_0\}$$ for some $$x_0 \in \mathbb{R}$$, the previous formula implies that $$\mathbb P(X = x_0) = 0$$, as an integral over a point. Should we allow this? Does that mean the event $$\{X = x_0\}$$ is impossible? It turns out it *is* possible, and that *measure theory* has the answer to all of these questions.

## Measure theory, with a probabilistic flavour

### Topologies and sigma algebras

Measure theory is essentially the theory of assigning sizes to sets, done rigorously. We will motivate probability spaces, which are measure spaces in which the "total size" is 1. But first, we define a *topology*; it turns out that it is intimately related to measures.

**Definition 1.** Let $$\Omega$$ be a set, called the **sample space** in our context. A **topology** on $$\Omega$$ is a collection $$\tau$$ of subsets of $$\Omega$$ satisfying the following properties:

1. **(Whole and empty set)** $$\Omega \in \tau$$ and $$\varnothing \in \tau$$;
2. **(Closure under arbitrary unions)** For an indexed collection of sets $$(A_i)_{i \in I}$$ with each $$A_i \in \tau$$ (finite or infinite), their union $$\bigcup_{i \in I} A_i \in \tau$$ also;
3. **(Closure under finite intersections)** For an finite collection of sets $$A_1,A_2,...,A_n$$ with each $$A_i \in \tau$$, their union $$\bigcap_{i = 1}^n A_i \in \tau$$ also.

If $$\tau$$ is a topology on $$\Omega$$, then the pair $$(\Omega,\tau)$$ is a **topological space**, and the elements in the topology (subsets of $$\Omega$$) are called **open sets**.

Yes, this is the *topology* in which donuts are the same as (homeomorphic to) coffee mugs! Let's look at a simple example of a topological space: the usual ones on the reals.

**Definition 2.** Let $$\Omega$$ be a set, called the **sample space** in our context. A **sigma algebra** on $$\Omega$$

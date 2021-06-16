# Why probability and statistics need measure theory

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

1. **(Whole and empty set)** The whole and empty sets are elements of the topology: $$\Omega \in \tau$$ and $$\varnothing \in \tau$$;
2. **(Closure under arbitrary unions)** For an indexed collection of sets $$(A_i)_{i \in I}$$ with each $$A_i \in \tau$$ (finite or infinite), their union $$\bigcup_{i \in I} A_i \in \tau$$ also;
3. **(Closure under finite intersections)** For an finite collection of sets $$A_1,A_2,...,A_n$$ with each $$A_i \in \tau$$, their intersection $$\bigcap_{i = 1}^n A_i \in \tau$$ also.

If $$\tau$$ is a topology on $$\Omega$$, then the pair $$(\Omega,\tau)$$ is a **topological space**, and the elements in the topology (subsets of $$\Omega$$) are called **open sets**. A set $$B$$ is **closed** if $$\Omega \setminus B$$ is open.

Yes, this is the *topology* in which donuts are the same as (homeomorphic to) coffee mugs! Let's look at a simple example of a topological space: the usual one $$(\mathbb{R},\tau)$$ on the reals. We define it via the open sets: a subset $$A \subseteq \mathbb{R}$$ is *open* if, for *each* point $$a \in A$$, there is an open interval $$I = (a - \epsilon,a + \epsilon)$$ (with $$\epsilon > 0$$) centred at $$a$$ that is wholly contained in $$A$$, i.e. $$I \subseteq A$$. For example, the set $$\mathbb{R}$$ is open: for $$a \in \mathbb{R}$$, we have $$(a - 1,a - 1) \subseteq \mathbb{R}$$. Additionally, the set $$(0,1)$$ is open: for $$a \in (0,1)$$, let $$\epsilon = \min(a,1 - a) \leq a,1 - a$$; note that $$0 = a - a \leq a - \epsilon$$ and $$a + \epsilon \leq a + (1 - a) = 1$$, so $$(a - \epsilon,a + \epsilon) \subseteq (0,1)$$. However, the set $$[0,1)$$ is not open: at $$a = 0$$, *every* open interval centred at $$0$$ goes "outside" of $$(0,1)$$. From these, we see that $$\mathbb R \setminus \mathbb R = \varnothing$$ and $$\mathbb R \setminus (0,1) = (-\infty,0] \cup [1,\infty)$$ are *closed*. A similar argument shows that $$[0,1]$$ is closed (its complement is open); this suggests that the intervals that are open sets are precisely the open intervals $$(a,b)$$, and those that are closed sets are precisely the closed intervals $$[a,b]$$ (where we allow infinity and $$a = b$$).

Now back to our question: which sets can we talk about probabilities of? Suppose that we only allowed open and closed sets. Then sets as simple as $$[0,1)$$ would be disallowed, but it seems quite reasonable to talk about the probability of $$\{0 \leq X < 1\}$$! Thus we introduce sigma algebras:

**Definition 2.** Let $$\Omega$$ be a set, called the **sample space** in our context. A **sigma algebra (of sets)** on $$\Omega$$ is a collection $$\mathcal F$$ of subsets of $$\Omega$$ satisfying the following properties:

1. **(Whole and empty set)** The whole and empty sets are elements of the sigma algebra: $$\Omega \in \mathcal F$$ and $$\varnothing \in \mathcal F$$ (we may omit one of these);
2. **(Closure under countable unions)** For an countable collection of sets $$(A_i)_{i = 1}^\infty$$ with each $$A_i \in \mathcal F$$, their union $$\bigcup_{i = 1}^\infty A_i \in \tau$$ also;
3. **(Closure under complements)** If $$A \in \mathcal F$$, then its **complement** $$A^c := \Omega \setminus A \in \mathcal F$$.

If $$\mathcal F$$ is a sigma algebra on $$\Omega$$, then the pair $$(\Omega,\mathcal F)$$ is a **measurable space**, and the elements in the sigma algebra (subsets of $$\Omega$$) are called **measurable sets**; in the context of probability, we call them **events**.

Note that by de Morgan's laws, we also have closure under countable intersections: if $$A_1,A_2,... \in \mathcal F$$, then $$A_1^c,A_2^c,... \in \mathcal F$$; then $$\bigcup_{i = 1}^\infty A_i^c = \left(\bigcap_{i = 1}^\infty A_i\right)^c \in \mathcal F$$, so its complement $$\bigcap_{i = 1}^\infty A_i \in \mathcal F$$.

Now, what's an example of a sigma algebra on the reals? For this, essentially take our standard topology from above, and just force it to be a sigma algebra: this gives the *Borel sigmal algebra*.

**Definition 3.** Let $$(\Omega,\tau)$$ be a topological space. The **Borel sigma algebra** $$\mathcal B = \mathcal B(\tau)$$ is the smallest sigma algebra containing $$\tau$$, in the sense that if $$\mathcal F$$ is a sigma algebra such that $$\tau \subseteq \mathcal F$$, then $$\mathcal B \subseteq \mathcal F$$. Elements of $$\mathcal B$$ are then called **Borel sets**.

For example, $$[0,1)$$ is a Borel set. Why? Notice that $$[0,1) = [(-\infty,0) \cup [1,\infty)]^c$$. Since $$(-\infty,0)$$ is open, it must be a Borel set. Also, since $$(-\infty,1)$$ is open, it too is a Borel set; its complement $$[1,\infty)$$ is then a Borel set (by property 3 of sigma algebras). Then $$(-\infty,0) \cup [1,\infty)$$ is a Borel set as a countable union of Borel sets. Finally, its complement $$[0,1)$$ is a Borel set, as claimed.

**Challenge exercise 1:** using a similar approach, prove that the set of irrational numbers is a Borel set. (*Hint:* you may want to show that every singleton set $$\{x_0\}$$ for $$x_0 \in \mathbb{R}$$ is closed. Post your solutions in the [Maths @ Monash Discord](https://discord.gg/hx63ZwSXBg)!)

### Measure and probability spaces

To be continued...

# Why probability and statistics need measure theory

<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

## Introduction to the problem

You may have encountered continuous probability distributions such as the normal distribution. It's often used to model things in the real world, and has nice statistical properties. You know the bell curve. But what you may not have seen is its formula: the *probability density function* (often shortened to just *density*),

$$f_\theta : \mathbb{R} \to \mathbb{R}, \quad x \mapsto \frac{1}{\sqrt{2\pi\sigma^2}}\exp\left(-\frac{(x - \mu)^2}{2\sigma^2}\right)$$

given a value of $$\theta = (\mu,\sigma^2) \in \Theta$$. In our case, $$\Theta = \mathbb{R} \times (0,\infty)$$; this is known as the *parameter space*.

![The probability density function of a normally distributed random variable.](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/Standard_deviation_diagram.svg/2880px-Standard_deviation_diagram.svg.png)

How does this relate to probability? Well, everything. We begin informally: suppose that $$X$$ is a continuous random variable with density $$f_\theta$$. The meaning of this is unimportant initially; think of $$X$$ as a "variable" that takes on values randomly (we will see that this is very much not a random variable *should* be). For a "reasonable" subset $$A \subseteq \mathbb{R}$$, let $$\mathbb{P}(X \in A)$$ denote the probability that $$X \in A$$, i.e. the probability of the event $$\{X \in A\}$$ (whatever that means). It turns out that, and as is often taught in a class in probability (even at high school level),

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

Yes, this is the *topology* in which donuts are the same as (homeomorphic to) coffee mugs!

![Classic image of a mug and torus morphing into each other, from Wikipedia.](https://upload.wikimedia.org/wikipedia/commons/2/26/Mug_and_Torus_morph.gif)

Let's look at a simple example of a topological space: the usual one $$(\mathbb{R},\tau)$$ on the reals. We define it via the open sets: a subset $$A \subseteq \mathbb{R}$$ is *open* if, for *each* point $$a \in A$$, there is an open interval $$I = (a - \epsilon,a + \epsilon)$$ (with $$\epsilon > 0$$) centred at $$a$$ that is wholly contained in $$A$$, i.e. $$I \subseteq A$$. For example, the set $$\mathbb{R}$$ is open: for $$a \in \mathbb{R}$$, we have $$(a - 1,a - 1) \subseteq \mathbb{R}$$. Additionally, the set $$(0,1)$$ is open: for $$a \in (0,1)$$, let $$\epsilon = \min(a,1 - a) \leq a,1 - a$$; note that $$0 = a - a \leq a - \epsilon$$ and $$a + \epsilon \leq a + (1 - a) = 1$$, so $$(a - \epsilon,a + \epsilon) \subseteq (0,1)$$. However, the set $$[0,1)$$ is not open: at $$a = 0$$, *every* open interval centred at $$0$$ goes "outside" of $$(0,1)$$. From these, we see that $$\mathbb R \setminus \mathbb R = \varnothing$$ and $$\mathbb R \setminus (0,1) = (-\infty,0] \cup [1,\infty)$$ are *closed*. A similar argument shows that $$[0,1]$$ is closed (its complement is open); this suggests that the intervals that are open sets are precisely the open intervals $$(a,b)$$, and those that are closed sets are precisely the closed intervals $$[a,b]$$ (where we allow infinity and $$a = b$$).

Now back to our question: which sets can we talk about probabilities of? Suppose that we only allowed open and closed sets. Then sets as simple as $$[0,1)$$ would be disallowed, but it seems quite reasonable to talk about the probability of $$\{0 \leq X < 1\}$$! Thus we introduce sigma algebras:

**Definition 2.** Let $$\Omega$$ be a set, called the **sample space** in our context. A **sigma algebra (of sets)** on $$\Omega$$ is a collection $$\mathcal F$$ of subsets of $$\Omega$$ satisfying the following properties:

1. **(Whole and empty set)** The whole and empty sets are elements of the sigma algebra: $$\Omega \in \mathcal F$$ and $$\varnothing \in \mathcal F$$ (we may omit one of these);
2. **(Closure under countable unions)** For an countable collection of sets $$(A_i)_{i = 1}^\infty$$ with each $$A_i \in \mathcal F$$, their union $$\bigcup_{i = 1}^\infty A_i \in \tau$$ also;
3. **(Closure under complements)** If $$A \in \mathcal F$$, then its **complement** $$A^c := \Omega \setminus A \in \mathcal F$$.

If $$\mathcal F$$ is a sigma algebra on $$\Omega$$, then the pair $$(\Omega,\mathcal F)$$ is a **measurable space**, and the elements in the sigma algebra (subsets of $$\Omega$$) are called **measurable sets**; in the context of probability, we call them **events**.

Note that by de Morgan's laws, we also have closure under countable intersections: if $$A_1,A_2,... \in \mathcal F$$, then $$A_1^c,A_2^c,... \in \mathcal F$$; then $$\bigcup_{i = 1}^\infty A_i^c = \left(\bigcap_{i = 1}^\infty A_i\right)^c \in \mathcal F$$, so its complement $$\bigcap_{i = 1}^\infty A_i \in \mathcal F$$.

Now, what's an example of a sigma algebra on the reals? For this, essentially take our standard topology from above, and just force it to be a sigma algebra: this gives the *Borel sigma algebra*.

**Definition 3.** Let $$(\Omega,\tau)$$ be a topological space. The **Borel sigma algebra** $$\mathcal B = \mathcal B(\tau)$$ is the smallest sigma algebra containing $$\tau$$, in the sense that if $$\mathcal F$$ is a sigma algebra such that $$\tau \subseteq \mathcal F$$, then $$\mathcal B \subseteq \mathcal F$$. Elements of $$\mathcal B$$ are then called **Borel sets**.

For example, $$[0,1)$$ is a Borel set. Why? Notice that $$[0,1) = [(-\infty,0) \cup [1,\infty)]^c$$. Since $$(-\infty,0)$$ is open, it must be a Borel set (as the Borel sigma algebra contains the topology). Also, since $$(-\infty,1)$$ is open, it too is a Borel set; its complement $$[1,\infty)$$ is then a Borel set (by property 3 of sigma algebras). Then $$(-\infty,0) \cup [1,\infty)$$ is a Borel set as a countable union of Borel sets. Finally, its complement $$[0,1)$$ is a Borel set, as claimed.

**Challenge exercise 1:** using a similar approach, prove that the set of irrational numbers is a Borel set. (*Hint:* at some point, you may want to consider a union of singleton sets $$\{x_0\}$$ for certain $$x_0 \in \mathbb{R}$$.) Post your solutions in the [Maths @ Monash Discord](https://discord.gg/hx63ZwSXBg)!

Almost any "reasonable" set you can think of will (almost surely, with probability 1!) be a Borel set (come up with your own examples and prove it), and these turn out to be precisely one broad class of sets $$A$$ for which it makes sense to talk about the probability of the event $$\{X \in A\}$$. Now it's time to tie this all back to probability. But to do so, we may as well (finally, for some of you) define probability rigorously...

### Measure and probability spaces

We define a way to assign "sizes" to measurable sets. This is the essence of measure theory.

**Definition 4.** A **measure** on a measurable space $$(\Omega,\mathcal F)$$ is a function $$\mu : \mathcal F \to [0,\infty]$$ (yes, we include infinity), satisfying:

1. **(Null empty set)** $$\mu(\varnothing) = 0$$;
2. **(Countable additivity)** If $$(A_i)_{i = 1}^\infty$$ is a countable sequence of pairwise disjoint sets (i.e. $$A_i \cap A_j = \varnothing$$ for $$i \neq j$$), then
   $$\mu\left(\bigcup_{i = 1}^\infty A_i\right) = \sum_{i = 1}^\infty \mu(A_i)$$.

Then $$(\Omega,\mathcal F,\mu)$$ is a **measure space**. If we add the additional property that $$\mu(\Omega) = 1$$, then $$\mu$$ is a **probability measure**, and $$(\Omega,\mathcal F,\mu)$$ is a **probability space**. (In this case, we usually write $$\mathbb P$$ instead of $$\mu$$, so that $$(\Omega,\mathcal F,\mathbb P)$$ is a probability space; here, $$\mathcal F$$ is the **event space**, and its elements are **events**.)

For example, the *Lebesgue measure* $$\lambda$$ on $$\mathbb{R}$$ is a way to assign lengths to subsets of the reals in a sensible way: $$\lambda((0,1)) = \lambda([0,1]) = 1$$, $$\lambda((0,1) \cup (3,5]) = 3$$, $$\lambda(\{1,2,3,4,5\}) = \lambda(\mathbb{N}) = \lambda(\mathbb{Q}) = 0$$, $$\lambda(\mathbb{R}) = \lambda(\mathbb{R} \setminus \mathbb{Q}) = \infty$$, etc.

But now we turn our attention back to probability spaces. Let's unpack this definition, at least for probability measures. Firstly, a probability is a function from the *event space* to $$[0,\infty]$$ (in fact, we can see that its codomain is $$[0,1]$$, by the other properties). The probability of the empty set must be 0; this makes sense, as we always expect some outcome to occur in an experiment. The probability of the sample space is 1; again this makes sense, as the sample space is the set of all outcomes. Finally, the key property of probability is that for a (countable) sequence of pairwise *disjoint* events, often called **mutually exclusive** events, the probability of their union is the sum of their probabilities. Again, this is intuitive from elementary probability: if two events can't happen simultaneously, we should be able to add their probabilities to get the probability of their union.

Note that the probability measure $$\mathbb P$$ is extremely abstract: once we have decided on a *sample space*, that is, a set of possible outcomes, *any* function $$\mathbb{P} : \mathcal F \to [0,1]$$ satisfying the above two properties of a measure, defines a "probability" on $$\Omega$$. This probability may range from something usual, to something wild. We consider a few simple examples:

**Example 5 (rolling two independent fair dice).** Here, a possible sample space is $$\Omega = \{1,...,6\} \times \{1,...,6\}$$, i.e. ordered pairs of numbers in $$1,...,6$$. This naturally encodes the outcome of a sequence of two dice rolls. Now what are the valid events? Since the sample space has $$36$$ elements (and is finite), we may take $$\mathcal F = \mathcal P(\Omega)$$, the power set of the sample space; that is, every subset of $$\Omega$$ is a valid event. (You can check that this is indeed a sigma algebra on $$\Omega$$. How many events are there in total?)

Now we consider the probability measure $$\mathbb P : \mathcal F \to [0,1]$$. Note that every event $$A$$ is a (countable) union of individual outcomes $$\omega \in \Omega$$. By our assumption of fairness and independence (which gives symmetry), each of the 36 possible outcomes is assigned a measure of $$\frac{1}{36}$$, so that $$\mathbb P(\Omega) = 1$$. Thus, for $$A \in \mathcal F$$, its probability depends only on its cardinality: in fact,

$$\mathbb P(A) = \mathbb P\left(\bigcup_{\omega \in A} \{\omega\}\right) = \sum_{\omega \in A} \mathbb P(\{\omega\}) = \sum_{\omega \in A} \frac{1}{36} = \frac{|A|}{36};$$

this fully specifies the probability space in this experiment. (Note that in this derivation, we assumed that $$\mathbb P$$ was a probability measure, to get countable additivity.)

**Example 6 (independently tossing a sequence of coins).** Here, a possible sample space is $$\{0,1\}^\infty$$, the space of infinite sequences with terms in $$\{0,1\}$$, where we may associate $$0$$ with a tails, and $$1$$ with a heads. Again, we may take the event space $$\mathcal F$$ to be the power set, so any set of sequences is a valid event. Suppose that for each toss, a head appears with probability $$p \in [0,1]$$. For integer $$n \geq 1$$, let $$A_n$$ be the event that the first head is tossed on the $$n$$th toss: then

$$A_n = \{(\underbrace{0,0,...,0,1}_{n\ \text{tosses}},x_{n+1},x_{n+2},...) : x_{n+1},x_{n+2}... \in \{0,1\}\}$$.

**Challenge question 2 (related to example 6).** If a random variable $$X$$ is defined on $$\mathbb{Z}^+$$ such that the event $$\{X = n\} = A_n$$ (i.e. $$\mathbb P(X = n) = \mathbb P(A_n)$$), what is the well-known distribution of $$X$$? Thus, what should $$\mathbb P$$ assign to this event $$A_n$$? What is the probability of any individual sequence $$(x_1,x_2,...)$$ of tosses? Is this a contradiction? (*Hint:* for the last part, consider countable additivity of $$\mathbb P$$. In particular, what is $$\lvert A_n \rvert$$? Can you show that it is *uncountable*? Consider Cantor's diagonalisation argument.) Post your solutions in the [Maths @ Monash Discord](https://discord.gg/hx63ZwSXBg)!

To be continued...

### What is a random variable?

To be continued...

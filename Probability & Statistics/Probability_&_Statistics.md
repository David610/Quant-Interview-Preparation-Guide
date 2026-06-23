# Hard Interview Traps

## Question 1: Recover $p$

Let $X\sim\text{Bernoulli}(p)$ and:

$$
\text{Var}(X)=0.24.
$$

Find $p$.

### Solution

$$
p(1-p)=0.24
$$

Therefore:

$$
\boxed{p=0.4\text{ or }p=0.6}
$$

Variance cannot distinguish $p$ from $1-p$.

---

## Question 2: Zero Covariance

Let $X$ and $Y$ be Bernoulli variables with:

$$
E[X]=0.4,\qquad E[Y]=0.7
$$

and:

$$
\text{Cov}(X,Y)=0.
$$

Are they independent?

### Solution

Zero covariance implies:

$$
E[XY]=E[X]E[Y]=0.28.
$$

Since $E[XY]=P(X=1,Y=1)$:

$$
P(X=1,Y=1)=P(X=1)P(Y=1).
$$

Thus:

$$
\boxed{X\text{ and }Y\text{ are independent}}
$$

This is special to Bernoulli variables.

---

## Question 3: Correlated Trades

Let $X_1,\ldots,X_{12}$ be Bernoulli variables with:

$$
P(X_i=1)=0.3
$$

and pairwise correlation:

$$
\rho=0.05.
$$

For:

$$
S=\sum_{i=1}^{12}X_i,
$$

find $E[S]$ and $\text{Var}(S)$.

### Solution

$$
E[S]=12(0.3)=\boxed{3.6}
$$

Using:

$$
\text{Var}(S) = np(1-p)\left[1+(n-1)\rho\right],
$$

we get:

$$
\text{Var}(S) = 12(0.3)(0.7)\left[1+11(0.05)\right].
$$

Therefore:

$$
\boxed{\text{Var}(S)=3.906}
$$

---

## Question 4: Hidden Market Regime

A hidden variable $P$ equals $0.3$ or $0.7$, each with probability $1/2$.

Conditional on $P$, $X$ and $Y$ are independent Bernoulli variables with parameter $P$.

Are $X$ and $Y$ independent?

### Solution

$$
P(X=1)=P(Y=1)=0.5
$$

but:

$$
P(X=1,Y=1) = \frac12(0.3^2)+\frac12(0.7^2) = 0.29.
$$

Since:

$$
0.29\neq0.5^2,
$$

$$
\boxed{X\text{ and }Y\text{ are not independent}}
$$

They are conditionally independent but marginally dependent.

---

## Question 5: Incomplete Information

Let:

$$
X\sim\text{Bernoulli}(p), \qquad \text{Var}(X)=0.09.
$$

A trade pays $4$ if $X=1$ and $-2$ otherwise. Find $E[Y]$.

### Solution

$$
p(1-p)=0.09
$$

gives:

$$
p=0.1\text{ or }p=0.9.
$$

Since:

$$
Y=6X-2,
$$

$$
E[Y]=6p-2.
$$

Therefore:

$$
\boxed{E[Y]=-1.4\text{ or }3.4}
$$

The expectation is not uniquely determined.

# Binomial Distribution

## Definition

A random variable is binomial if:

$$
X=\sum_{i=1}^n X_i,
\qquad
X_i\overset{\text{i.i.d.}}{\sim}\text{Bernoulli}(p).
$$

Therefore:

$$
X\sim\text{Binomial}(n,p).
$$

The important word is **i.i.d.**:

* identical success probability;
* mutual independence, not only pairwise independence.

Its probability mass function is:

$$
P(X=k)=\binom nkp^k(1-p)^{n-k}.
$$

Also:

$$
E[X]=np,
\qquad
\text{Var}(X)=np(1-p).
$$

---

## Common Theoretical Traps

### Pairwise Independence Is Not Enough

Pairwise-independent Bernoulli variables need not be jointly independent. Their sum may therefore fail to be binomial.

### A Random Probability Does Not Give a Binomial Distribution

If:

$$
X\mid P\sim\text{Binomial}(n,P),
$$

where $P$ is random, then $X$ is generally a **mixture of binomials**, not:

$$
\text{Binomial}(n,E[P]).
$$

### Conditioning Destroys Independence

Independent Bernoulli trials become dependent after conditioning on their total number of successes.

### Unequal Probabilities

A sum of independent Bernoulli variables with probabilities $p_1,\ldots,p_n$ is generally Poisson-binomial, not binomial.

### Moments Must Produce a Valid Integer $n$

From:

$$
E[X]=\mu,
\qquad
\text{Var}(X)=v,
$$

we obtain:

$$
p=1-\frac{v}{\mu},
\qquad
n=\frac{\mu^2}{\mu-v}.
$$

But the model is valid only if:

$$
0\leq p\leq1
$$

and $n$ is a positive integer.

---

# Hard Interview Traps

## Question 1: Recover the Distribution

Suppose:

$$
X\sim\text{Binomial}(n,p),
$$

with:

$$
E[X]=7.2,
\qquad
\text{Var}(X)=5.04.
$$

Find $n$ and $p$.

### Solution

Since:

$$
\text{Var}(X)=E[X](1-p),
$$

we get:

$$
5.04=7.2(1-p).
$$

Therefore:

$$
p=0.3.
$$

Then:

$$
n=\frac{7.2}{0.3}=24.
$$

Thus:

$$
\boxed{X\sim\text{Binomial}(24,0.3)}
$$

---

## Question 2: Pairwise Independent Trials

Let $U$ and $V$ be independent Bernoulli variables with parameter $1/2$, and define:

$$
W=U\oplus V,
$$

where $\oplus$ denotes XOR.

Let:

$$
S=U+V+W.
$$

Is $S$ binomial?

### Solution

The possible outcomes are:

$$
(U,V,W)\in\{(0,0,0),(0,1,1),(1,0,1),(1,1,0)\}.
$$

Therefore:

$$
P(S=0)=\frac14,
\qquad
P(S=2)=\frac34.
$$

Although $U,V,W$ are pairwise independent, they are not jointly independent.

Hence:

$$
\boxed{S\text{ is not binomial}}
$$

---

## Question 3: Hidden Success Probability

A random variable $P$ equals $0.2$ or $0.8$, each with probability $1/2$.

Conditional on $P$:

$$
X\mid P\sim\text{Binomial}(n,P).
$$

Find $E[X]$ and $\text{Var}(X)$.

### Solution

By iterated expectation:

$$
E[X]=E[nP]=\frac n2.
$$

Using total variance:

$$
\text{Var}(X) = E[\text{Var}(X\mid P)] + \text{Var}(E[X\mid P]).
$$

Therefore:

$$
\text{Var}(X) = nE[P(1-P)] + n^2\text{Var}(P).
$$

Here:

$$
E[P(1-P)]=0.16,
\qquad
\text{Var}(P)=0.09.
$$

Thus:

$$
\boxed{
\text{Var}(X)=0.16n+0.09n^2
}
$$

This is not the variance of:

$$
\text{Binomial}\left(n,\frac12\right),
$$

which would be $0.25n$.

---

## Question 4: Conditioning on the Total

Let:

$$
X\sim\text{Binomial}(n,p),
\qquad
Y\sim\text{Binomial}(m,p),
$$

independently.

Find the distribution of $X$ conditional on:

$$
X+Y=r.
$$

### Solution

For valid $k$:

$$
P(X=k\mid X+Y=r) = \frac{P(X=k,Y=r-k)}{P(X+Y=r)}.
$$

After cancelling the powers of $p$ and $1-p$:

$$
P(X=k\mid X+Y=r) = \frac{\binom nk\binom m{r-k}}{\binom{n+m}r}.
$$

Therefore:

$$
\boxed{
X\mid X+Y=r
\sim
\text{Hypergeometric}(n+m,r,n)
}
$$

The conditional distribution does not depend on $p$.

---

## Question 5: When Is a Bernoulli Sum Binomial?

Let $X_1,\ldots,X_n$ be independent Bernoulli variables with:

$$
P(X_i=1)=p_i,
\qquad 0<p_i<1.
$$

Suppose:

$$
S=\sum_{i=1}^nX_i
\sim\text{Binomial}(n,p).
$$

Prove that:

$$
p_1=\cdots=p_n=p.
$$

### Solution

The probability-generating function of $S$ is:

$$
G_S(z) = \prod_{i=1}^n(1-p_i+p_i z).
$$

Because $S$ is binomial:

$$
G_S(z)=(1-p+pz)^n.
$$

The roots on the left are:

$$
z_i=-\frac{1-p_i}{p_i}.
$$

The right-hand side has one repeated root:

$$
z=-\frac{1-p}{p}.
$$

Therefore all roots must be equal:

$$
-\frac{1-p_i}{p_i} = -\frac{1-p}{p}.
$$

Hence:

$$
\boxed{p_i=p\quad\text{for every }i}
$$

So independence alone is insufficient; equal success probabilities are also necessary.
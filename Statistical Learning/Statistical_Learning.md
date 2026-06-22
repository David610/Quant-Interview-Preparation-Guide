# Hard Interview Traps

## Question 1: Recover \(p\)

Let \(X\sim\operatorname{Bernoulli}(p)\) and:

\[
\operatorname{Var}(X)=0.24.
\]

Find \(p\).

### Solution

\[
p(1-p)=0.24
\]

Therefore:

\[
\boxed{p=0.4\text{ or }p=0.6}
\]

Variance cannot distinguish \(p\) from \(1-p\).

---

## Question 2: Zero Covariance

Let \(X\) and \(Y\) be Bernoulli variables with:

\[
E[X]=0.4,\qquad E[Y]=0.7
\]

and:

\[
\operatorname{Cov}(X,Y)=0.
\]

Are they independent?

### Solution

Zero covariance implies:

\[
E[XY]=E[X]E[Y]=0.28.
\]

Since \(E[XY]=P(X=1,Y=1)\):

\[
P(X=1,Y=1)=P(X=1)P(Y=1).
\]

Thus:

\[
\boxed{X\text{ and }Y\text{ are independent}}
\]

This is special to Bernoulli variables.

---

## Question 3: Correlated Trades

Let \(X_1,\ldots,X_{12}\) be Bernoulli variables with:

\[
P(X_i=1)=0.3
\]

and pairwise correlation:

\[
\rho=0.05.
\]

For:

\[
S=\sum_{i=1}^{12}X_i,
\]

find \(E[S]\) and \(\operatorname{Var}(S)\).

### Solution

\[
E[S]=12(0.3)=\boxed{3.6}
\]

Using:

\[
\operatorname{Var}(S)
=
np(1-p)\left[1+(n-1)\rho\right],
\]

we get:

\[
\operatorname{Var}(S)
=
12(0.3)(0.7)\left[1+11(0.05)\right].
\]

Therefore:

\[
\boxed{\operatorname{Var}(S)=3.906}
\]

---

## Question 4: Hidden Market Regime

A hidden variable \(P\) equals \(0.3\) or \(0.7\), each with probability \(1/2\).

Conditional on \(P\), \(X\) and \(Y\) are independent Bernoulli variables with parameter \(P\).

Are \(X\) and \(Y\) independent?

### Solution

\[
P(X=1)=P(Y=1)=0.5
\]

but:

\[
P(X=1,Y=1)
=
\frac12(0.3^2)+\frac12(0.7^2)
=
0.29.
\]

Since:

\[
0.29\neq0.5^2,
\]

\[
\boxed{X\text{ and }Y\text{ are not independent}}
\]

They are conditionally independent but marginally dependent.

---

## Question 5: Incomplete Information

Let:

\[
X\sim\operatorname{Bernoulli}(p),
\qquad
\operatorname{Var}(X)=0.09.
\]

A trade pays \(4\) if \(X=1\) and \(-2\) otherwise. Find \(E[Y]\).

### Solution

\[
p(1-p)=0.09
\]

gives:

\[
p=0.1\text{ or }p=0.9.
\]

Since:

\[
Y=6X-2,
\]

\[
E[Y]=6p-2.
\]

Therefore:

\[
\boxed{E[Y]=-1.4\text{ or }3.4}
\]

The expectation is not uniquely determined.
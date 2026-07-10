# Question 1: Odd Number of Heads

Twelve independent coins:

- three with $P(\text{heads}) = \tfrac{1}{2}$
- three with $P(\text{heads}) = \tfrac{1}{3}$
- three with $P(\text{heads}) = \tfrac{1}{5}$
- three with $P(\text{heads}) = \tfrac{1}{9}$

$S$ = total number of heads. Find $P(S \text{ odd})$.

Let $X_i$ be the indicator for coin $i$ landing heads, so $S = \sum_{i=1}^{12} X_i$. Since the coins aren't identically distributed, $S$ is Poisson-binomial, not binomial - but it turns out we don't need the full distribution at all.

## The shortcut: one fair coin is all that matters

Pull out a single fair coin, $X \sim \text{Bernoulli}(1/2)$ and let $R$ be the sum of the other eleven.

$$S = X + R$$

Look at the two cases for $R$:

- If $R$ is even, $S$ is odd exactly when the fair coin comes up heads.
- If $R$ is odd, $S$ is odd exactly when the fair coin comes up tails.

Either way, that's a coin flip - $P(S \text{ odd} \mid R) = 1/2$, no matter what $R$ turned out to be. So

$$P(S \text{ odd}) = \frac{1}{2}$$

The probabilities of the other eleven coins never enter the calculation. That's the whole trick.

## Second way to see it: split into binomial groups

Group the coins by probability:

$$A \sim \text{Bin}(3, \tfrac12), \quad B \sim \text{Bin}(3, \tfrac13), \quad C \sim \text{Bin}(3, \tfrac15), \quad D \sim \text{Bin}(3, \tfrac19)$$

$S = A + B + C + D$, all independent. Focus on $A$, the fair-coin group:

$$P(A \text{ even}) = P(A=0) + P(A=2) = \frac{1}{8} + \frac{3}{8} = \frac{1}{2}$$
$$P(A \text{ odd}) = P(A=1) + P(A=3) = \frac{3}{8} + \frac{1}{8} = \frac{1}{2}$$

Let $T = B + C + D$. $S$ is odd when $A$ is odd and $T$ even or $A$ even and $T$ odd:

$$P(S \text{ odd}) = \frac{1}{2}P(T \text{ even}) + \frac{1}{2}P(T \text{ odd}) = \frac{1}{2}\big[P(T\text{ even}) + P(T\text{ odd})\big] = \frac{1}{2}$$

Same answer and again $T$'s actual distribution never matters, only that $A$ splits 50/50.

## Third way: the parity generating trick

For any integer-valued $S$, $(-1)^S = 1$ if even, $-1$ if odd, so

$$E[(-1)^S] = P(S \text{ even}) - P(S \text{ odd})$$

Since $S = \sum X_i$ and the coins are independent:

$$E[(-1)^S] = \prod_{i=1}^{12} E[(-1)^{X_i}] = \prod_{i=1}^{12}(1 - 2p_i)$$

because $E[(-1)^{X_i}] = (1-p_i) - p_i = 1 - 2p_i$.

Combined with $P(\text{even}) + P(\text{odd}) = 1$, this gives a general formula worth memorizing:

$$P(S \text{ odd}) = \frac{1 - \prod_{i=1}^n (1-2p_i)}{2}$​

Plug in our coins: the fair coins give $1 - 2(1/2) = 0$, which zeroes out the whole product regardless of what the other nine coins do. So again $P(S \text{ odd}) = 1/2$.

## The pattern worth remembering

$$P(S \text{ odd}) = \frac{1 - \prod_i (1-2p_i)}{2}$$

Two useful special cases:

- **Any fair coin in the mix** → product is 0 → $P(S\text{ odd}) = 1/2$, full stop, regardless of the other coins.
- **All coins identical**, probability $p$ → $P(S \text{ odd}) = \dfrac{1 - (1-2p)^n}{2}$.

A general Poisson-binomial sum isn't binomial, but parity questions almost always reduce to something simpler than the full distribution. When you see "is the sum odd," think: parity, mod 2, $(-1)^S$ and check whether a fair coin is hiding in there - if it is, you're done before you start.

**Answer: $\boxed{1/2}$**

## Checking it in Python
 
Three independent checks in one script: the $(-1)^S$ formula, a full dynamic-programming build of the Poisson-binomial distribution and a Monte Carlo simulation.

```python
import random
from math import isclose
 
probs = [1/2]*3 + [1/3]*3 + [1/5]*3 + [1/9]*3
 
# Exact, via the (-1)^S formula
product = 1.0
for p in probs:
    product *= (1 - 2*p)
p_odd_exact = (1 - product) / 2
 
# Exact, via full distribution (DP convolution over Poisso-binomial)
dist = [1.0]  # dist[k] = P(sum so far = k)
for p in probs:
    new_dist = [0.0] * (len(dist) + 1)
    for k, prob_k in enumerate(dist):
        new_dist[k] += prob_k * (1 - p)
        new_dist[k+1] += prob_k * p
    dist = new_dist
p_odd_dp = sum(dist[k] for k in range(len(dist)) if k % 2 == 1)
 
# Monte Carlo simulation
random.seed(0)
trials = 2_000_000
odd_count = 0
for _ in range(trials):
    s = sum(1 for p in probs if random.random() < p)
    if s % 2 == 1:
        odd_count += 1
p_odd_sim = odd_count / trials
 
print(f"Exact ((-1)^S formula): {p_odd_exact:.6f}")
print(f"Exact (DP over full distribution): {p_odd_dp:.6f}")
print(f"Monte Carlo ({trials:,} trials): {p_odd_sim:.6f}")
assert isclose(p_odd_exact, 0.5, abs_tol=1e-9)
assert isclose(p_odd_dp, 0.5, abs_tol=1e-9)
print("All checks match 1/2.")
```
 
Output when I ran it:
 
```
Exact ((-1)^S formula): 0.500000
Exact (DP over full distribution): 0.500000
Monte Carlo (2,000,000 trials): 0.499953
All checks match 1/2.
```

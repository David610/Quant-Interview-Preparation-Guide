# The Unknown Horizon Problem

## Problem

The total number of candidates $N$ is chosen uniformly from:

$$\{1,2,3,4,5,6\}.$$

Therefore,

$$P(N=m)=\frac{1}{6},\qquad m\in\{1,2,3,4,5,6\}.$$

The candidates have distinct skill levels and arrive in a random order.

When candidate $n$ arrives, you learn only their rank among the first $n$ candidates. You must immediately accept or reject them, and you may accept only one candidate.

**Goal:** Maximize the probability of selecting the best candidate overall.

---

## Solution

A candidate who is not the best seen so far cannot be the best overall. Therefore, only a **record candidate** should ever be accepted.

A record is a candidate ranked first among all candidates seen so far.

The probability that candidate $n$ is a record is:

$$P(R_n)=\frac{1}{n}.$$

### Accepting a Record

Suppose there are $m$ candidates in total.

If candidate $n$ is a record, the probability that they are the best overall is:

$$P(\text{best overall}\mid R_n, N=m)=\frac{n}{m}.$$

After candidate $n$ arrives, we know that:

$$N\in\{n,n+1,\ldots,6\}.$$

These values are equally likely. Therefore, the probability of winning by accepting a record at stage $n$ is:

$$A_n=\frac{1}{7-n}\sum_{m=n}^{6}\frac{n}{m}.$$

### Rejecting and Continuing

Let $V_n$ be the optimal winning probability before candidate $n$ is observed, conditional on candidate $n$ existing.

If candidate $n$ is rejected, another candidate exists with probability:

$$P(N>n\mid N\ge n)=\frac{6-n}{7-n}.$$

Therefore, the continuation value is:

$$C_n=\frac{6-n}{7-n}V_{n+1}.$$

The dynamic-programming recurrence is:

$$V_n=\frac{1}{n}\max(A_n,C_n)+\frac{n-1}{n}C_n.$$

---

## Backward-Induction Results

| Stage $n$ | Accept $A_n$ | Continue $C_n$ | Decision |
|---:|---:|---:|:---|
| 1 | $\frac{49}{120}$ | $\frac{203}{540}$ | Accept |
| 2 | $\frac{29}{50}$ | $\frac{29}{90}$ | Accept |
| 3 | $\frac{57}{80}$ | $\frac{119}{480}$ | Accept |
| 4 | $\frac{37}{45}$ | $\frac{1}{6}$ | Accept |
| 5 | $\frac{11}{12}$ | $\frac{1}{12}$ | Accept |
| 6 | $1$ | $0$ | Accept |

At every stage:

$$A_n>C_n.$$

Therefore, every record should be accepted.

The first candidate is automatically a record, so the optimal strategy is:

> **Accept the first candidate immediately.**

The probability of winning is:

$$P(\text{win})=\frac{1}{6}\left(1+\frac{1}{2}+\frac{1}{3}+\frac{1}{4}+\frac{1}{5}+\frac{1}{6}\right)=\frac{49}{120}.$$

Therefore,

$$P(\text{win})=\frac{49}{120}\approx 40.83\%.$$

The classical $1/e$ secretary strategy does not apply because the total number of candidates is unknown and may be as small as one.

---

## Follow-Ups an Interviewer Will Reach For

1. **General prior.** For arbitrary $p_m = P(N=m)$, replace the uniform posterior with $P(N=m\mid N\ge n)=p_m/\sum_{k\ge n}p_k$. Everything else carries through unchanged. Uniformity is convenience, not structure.

2. **Objective change.** Maximise expected *rank* instead of $P(\text{best})$ and the record-only reduction breaks immediately: a strong non-record is now worth taking.

3. **Absolute values instead of ranks.** Then observations are informative about $N$'s scale in some models, the uniform posterior fails, and the state must include more than $n$.

4. **Asymptotics.** Under a uniform prior on $\{1,\dots,K\}$ the optimal value decreases slowly: $0.2849$ at $K=50$, $0.2714$ at $K=1000$, $0.27068$ at $K=80000$. Numerically it settles near $0.2707$, clearly below $1/e\approx 0.3679$. No closed form for that constant is derived here. The point that matters: the cost of not knowing the horizon does **not** vanish as $K$ grows.

5. **Why $E[1/N]$ and not $1/E[N]$?** Jensen: $E[1/N] = 0.4083 > 1/3.5 = 0.2857$. Confusing the two is a fast way to fail a sanity check.

---

## Python Verification

```python
from fractions import Fraction


def solve(max_candidates: int = 6) -> None:
    accept = {}
    continue_value = {}
    optimal = {}

    for n in range(max_candidates, 0, -1):
        accept[n] = sum(
            Fraction(n, total)
            for total in range(n, max_candidates + 1)
        ) / (max_candidates - n + 1)

        if n == max_candidates:
            continue_value[n] = Fraction(0)
        else:
            probability_of_next_candidate = Fraction(
                max_candidates - n,
                max_candidates - n + 1,
            )

            continue_value[n] = (
                probability_of_next_candidate * optimal[n + 1]
            )

        optimal[n] = (
            Fraction(1, n) * max(accept[n], continue_value[n])
            + Fraction(n - 1, n) * continue_value[n]
        )

        decision = (
            "accept"
            if accept[n] >= continue_value[n]
            else "reject"
        )

        print(
            f"n={n}: "
            f"accept={accept[n]}, "
            f"continue={continue_value[n]}, "
            f"decision={decision}"
        )

    print(
        f"\nOptimal probability: "
        f"{optimal[1]} = {float(optimal[1]):.2%}"
    )


solve()
```

Expected final line:

```text
Optimal probability: 49/120 = 40.83%
```

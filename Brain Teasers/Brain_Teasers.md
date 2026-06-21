# Brain Teasers

Brain teasers test how you think, not what you know. The frame comes from Kahneman's *[Thinking, Fast and Slow]*:

- **System 1**: fast, intuitive, error-prone.
- **System 2**: slow, logical, accurate.

Traders want **System 2** reasoning, done quickly.

## How to Practice

The value is the reasoning, not the memorized answer. Work each problem honestly and try to unblock yourself before checking the solution. That builds real problem-solving skill. Looking up answers does not.

# Problem 1: Egg Dropping Puzzle: Strategic Optimization

A comprehensive guide to solving the Egg Dropping puzzle, a classic quantitative interview question used to test logic, dynamic programming, and decision-tree intuition.

---

## 1. Problem Statement

You are given:
- A building with **$N$ floors**.
- **$n$ identical eggs**.
- A hidden **critical floor $T$**:
    - An egg dropped from floor $T$ or below **survives**.
    - An egg dropped from any floor above $T$ **breaks**.
- If an egg survives, it can be reused. If it breaks, it is discarded.

**The Objective:** Find the minimum number of drops required in the **worst case** to identify $T$ exactly.

---

## 2. Progressive Analysis

### Case A: 1 Egg (Linear Strategy)
With only one egg, you cannot afford to break it. If you drop it from floor 50 and it breaks, you have no way to test floors 1–49.
- **Strategy:** Must test floor-by-floor (1, 2, 3...).
- **Worst Case:** $N$ drops.
- **Math:** $F(1, d) = d$.

### Case B: 2 Eggs (Balanced Strategy)
With two eggs, you can take larger jumps. If the first egg breaks, the second egg covers the remaining gap linearly. To keep the worst-case scenario constant, each subsequent jump must be 1 floor smaller.
- **Strategy:** If the first drop is at floor $d$, and it survives, the next jump is $d-1$.
- **Math:** $d + (d-1) + (d-2) + \dots + 1 \geq N$.
- **For 100 Floors:** $\frac{d(d+1)}{2} \geq 100 \implies d = 14$.

---

## 3. The General Solution ($n$ Eggs, $d$ Drops)

To solve for the general case, we flip the perspective:
**"What is the maximum number of floors $F(e, d)$ we can test with $e$ eggs and $d$ drops?"**

### The Fundamental Recurrence
When you drop an egg from a floor:
1. **The Egg Breaks:** 
   - You have **$e-1$** eggs and **$d-1$** drops left.
   - You can cover $F(e-1, d-1)$ floors **below**.
2. **The Egg Survives:** 
   - You have **$e$** eggs and **$d-1$** drops left.
   - You can cover $F(e, d-1)$ floors **above**.

Including the current floor (+1), the recurrence is:
$$\boxed{F(e, d) = F(e-1, d-1) + 1 + F(e, d-1)}$$

> **Note on $d-1$:** The drop count decreases on both sides because one "attempt" is consumed regardless of the outcome.

---

## 4. The Binomial Interpretation

The recurrence above is identical to the way Pascal's Triangle is constructed. The maximum floors $F(e, d)$ can be expressed as a sum of binomial coefficients:

$$F(e, d) = \sum_{i=1}^{e} \binom{d}{i}$$

### Example: 3 Eggs, 100 Floors
Find the smallest $d$ where $\sum_{i=1}^{3} \binom{d}{i} \geq 100$:
- For $d=8$: $\binom{8}{1} + \binom{8}{2} + \binom{8}{3} = 8 + 28 + 56 = 92$ (Not enough)
- For $d=9$: $\binom{9}{1} + \binom{9}{2} + \binom{9}{3} = 9 + 36 + 84 = 129$ (Enough)

**Answer: 9 Drops.**

---

## 5. Summary Table (100 Floors)

| Eggs ($n$) | Complexity Logic | Min Drops ($d$) |
| :--- | :--- | :--- |
| 1 | $d \geq 100$ | 100 |
| 2 | $\frac{d(d+1)}{2} \geq 100$ | 14 |
| 3 | $\binom{d}{1} + \binom{d}{2} + \binom{d}{3} \geq 100$ | 9 |
| $\infty$ | $\log_2(100)$ | 7 |

---

## 6. Python Implementation

The following algorithm calculates the minimum drops for $n$ eggs and $N$ floors using the coverage recurrence in $O(n \cdot d)$ time and $O(n)$ space.

```python
def solve_egg_dropping(eggs: int, floors: int) -> int:
    """
    Calculates the minimum number of drops needed in the worst case.
    Uses the DP approach: dp[e] = F(e, d)
    """
    if floors == 0: return 0
    if eggs == 1: return floors

    # dp[e] stores the maximum floors reachable with 'e' eggs 
    # for the current number of drops.
    dp = [0] * (eggs + 1)
    drops = 0

    while dp[eggs] < floors:
        drops += 1
        # Traverse backwards to ensure we use results from (drops - 1)
        for e in range(eggs, 0, -1):
            # Formula: F(e, d) = F(e-1, d-1) + F(e, d-1) + 1
            dp[e] = dp[e] + dp[e-1] + 1
            
    return drops

if __name__ == "__main__":
    test_cases = [(1, 100), (2, 100), (3, 100), (10, 100)]
    for e, f in test_cases:
        print(f"Eggs: {e}, Floors: {f} -> Min Drops: {solve_egg_dropping(e, f)}")
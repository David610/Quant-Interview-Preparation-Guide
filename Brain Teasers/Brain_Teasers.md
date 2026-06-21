# Brain Teasers

Brain teasers test how you think, not what you know. The frame comes from Kahneman's *[Thinking, Fast and Slow]*:

- **System 1**: fast, intuitive, error-prone.
- **System 2**: slow, logical, accurate.

Traders want **System 2** reasoning, done quickly.

## How to Practice

The value is the reasoning, not the memorized answer. Work each problem honestly and try to unblock yourself before checking the solution. That builds real problem-solving skill. Looking up answers does not.

# Problem 1: Egg Dropping Puzzle: Strategic Optimization

A comprehensive guide to solving the Egg Dropping puzzle, a classic quantitative interview question used to test logic, dynamic programming and decision-tree intuition.

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
- **Strategy:** If the first drop is at floor $d$ and it survives, the next jump is $d-1$.
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
```


# The Average Speed Trap

A classic "sanity check" for quantitative roles. It tests whether you understand that average speed is weighted by **time**, not distance and whether you can spot a physical impossibility.

---

## 1. Problem Statement

You are given:
- A track of **$10\text{ km}$ per lap**, run for **2 laps** ($20\text{ km}$ total).
- **Lap 1** is run at an average speed of **$10\text{ km/h}$**.
- A **target average** of **$20\text{ km/h}$** over the full $20\text{ km}$.

**The Objective:** Find the speed required on Lap 2 to hit the target average.

---

## 2. The Intuition Trap

Most people instinctively take the arithmetic mean:

$$\frac{10\text{ km/h} + X\text{ km/h}}{2} = 20\text{ km/h} \implies X = 30\text{ km/h}$$

This is **wrong**. Speed is $\text{Distance} / \text{Time}$. You spend more time on slow laps than on fast laps, so the slow laps carry a "heavier" weight in the average.

> **Why the trap works:** The arithmetic mean assumes equal weighting. Here the weights are the *times*, not the distances and the slow lap eats most of the time budget.

---

## 3. The Time-Budget Proof

The cleanest way to solve this is to track time, not speed.

### Time spent on Lap 1
$$t_1 = \frac{\text{Distance}}{\text{Speed}} = \frac{10\text{ km}}{10\text{ km/h}} = 1\text{ hour}$$

### Total time allowed for the target
$$t_{total} = \frac{\text{Total Distance}}{\text{Target Speed}} = \frac{20\text{ km}}{20\text{ km/h}} = 1\text{ hour}$$

### Time remaining for Lap 2
$$t_2 = t_{total} - t_1 = 1\text{ hour} - 1\text{ hour} = 0\text{ hours}$$

**Conclusion:** To average $20\text{ km/h}$, Lap 2 must be completed in **zero time**. This is impossible at any finite speed.

---

## 4. The General Solution (Harmonic Mean)

When the two distances are equal, the average speed $v_{avg}$ is the **harmonic mean** of the two lap speeds:

$$v_{avg} = \frac{2 \cdot v_1 \cdot v_2}{v_1 + v_2}$$

Solving for $v_2$:

$$\boxed{v_2 = \frac{v_1 \cdot v_{avg}}{2v_1 - v_{avg}}}$$

> **The breaking point:** When $v_{avg} = 2v_1$, the denominator becomes zero, so $v_2 \to \infty$. You can never average more than **twice** your first-lap speed, no matter how fast Lap 2 is.

---

## 5. Summary Table ($v_1 = 10\text{ km/h}$)

| Target Avg ($v_{avg}$) | Denominator ($2v_1 - v_{avg}$) | Required Lap 2 ($v_2$) |
| :--- | :--- | :--- |
| 12 km/h | 8 | 15 km/h |
| 15 km/h | 5 | 30 km/h |
| 18 km/h | 2 | 90 km/h |
| 19 km/h | 1 | 190 km/h |
| 20 km/h | 0 | **Impossible** |

---

## 6. Python Implementation

```python
def calculate_required_speed(v1: float, v_target: float) -> float:
    """
    Calculates the Lap 2 speed needed to reach a target average.
    Assumes both laps have equal distance.
    Returns infinity when the target is physically impossible.
    """
    denominator = 2 * v1 - v_target
    if denominator <= 0:
        return float('inf')  # Target >= 2 * v1 is impossible
    return (v1 * v_target) / denominator


if __name__ == "__main__":
    v1 = 10
    for v_target in [12, 15, 18, 19, 20]:
        v2 = calculate_required_speed(v1, v_target)
        if v2 == float('inf'):
            print(f"Target {v_target} km/h is impossible (Lap 1 was {v1} km/h).")
        else:
            print(f"Target {v_target} km/h -> Lap 2 must be {v2} km/h")
```
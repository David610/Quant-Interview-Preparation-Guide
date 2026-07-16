# Market Making

## Definition

A market maker simultaneously posts:

* a bid price $`b_t`$, at which it is willing to buy;
* an ask price $`a_t`$, at which it is willing to sell.

Relative to a reference price $`S_t`$, usually the mid-price:

```math
b_t = S_t - \delta_t^b,
\qquad
a_t = S_t + \delta_t^a.
```

Here:

```math
\delta_t^b = S_t - b_t,
\qquad
\delta_t^a = a_t - S_t
```

are the bid and ask quote distances.

The quoted spread is:

```math
a_t - b_t = \delta_t^a + \delta_t^b.
```

A market maker tries to earn this spread while managing:

* inventory risk;
* adverse selection;
* execution and queue risk;
* fees and hedging costs.

---

## Inventory and Wealth

Let:

```math
q_t = \text{inventory},
\qquad
X_t = \text{cash}.
```

If a bid order fills:

```math
q_t \rightarrow q_t + 1,
\qquad
X_t \rightarrow X_t - b_t.
```

If an ask order fills:

```math
q_t \rightarrow q_t - 1,
\qquad
X_t \rightarrow X_t + a_t.
```

Marked-to-market wealth is:

```math
W_t = X_t + q_t S_t.
```

A common risk-adjusted objective is:

```math
\max\,
E\left[
X_T + q_T S_T
- \frac{\phi}{2}\int_0^T q_t^2\,dt
- \frac{\eta}{2} q_T^2
\right].
```

The term:

```math
\frac{\phi}{2}\int_0^T q_t^2\,dt
```

penalizes holding inventory during the trading period.

The term:

```math
\frac{\eta}{2} q_T^2
```

penalizes ending with inventory.

The goal is therefore not to maximize fills. It is to maximize:

```math
\text{spread revenue}
- \text{inventory risk}
- \text{adverse selection}
- \text{execution costs}.
```

---

## Markouts and Adverse Selection

For a bid fill, define the markout after horizon $`\tau`$ as:

```math
M_{t,\tau}^b = S_{t+\tau} - b_t.
```

For an ask fill:

```math
M_{t,\tau}^a = a_t - S_{t+\tau}.
```

A positive markout is favorable to the market maker.

For a bid fill:

```math
E\left[
M_{t,\tau}^b
\mid
\text{bid fill}
\right]
=
S_t - b_t
+
E\left[
S_{t+\tau} - S_t
\mid
\text{bid fill}
\right].
```

The first term is the earned half-spread.

The second term measures the price movement after execution.

Adverse selection occurs when:

```math
E\left[
S_{t+\tau} - S_t
\mid
\text{bid fill}
\right] < 0
```

or:

```math
E\left[
S_{t+\tau} - S_t
\mid
\text{ask fill}
\right] > 0.
```

The important point is that a fill is not a random event. It often occurs precisely when another trader expects the quote to be favorable.

---

## Fill Intensity

A common model for execution intensity is:

```math
\lambda(\delta) = A e^{-k\delta}.
```

Here:

* $`A`$ controls the overall arrival rate;
* $`k`$ measures how quickly fills disappear as the quote moves away;
* $`\delta`$ is the quote distance.

A smaller $`\delta`$ produces more fills but less profit per fill.

A larger $`\delta`$ produces fewer fills but more profit per fill.

Intensity is not directly a probability. If the intensity is constant over a time interval $`\Delta t`$, then:

```math
P(\text{at least one fill})
=
1 - e^{-\lambda(\delta)\Delta t}.
```

For small $`\Delta t`$:

```math
P(\text{fill})
\approx
\lambda(\delta)\Delta t.
```

---

## Inventory Skew

In the basic Avellaneda–Stoikov model, the reservation price is:

```math
r_t
=
S_t - q_t\gamma\sigma^2(T-t).
```

Here:

* $`q_t`$ is inventory;
* $`\gamma`$ is risk aversion;
* $`\sigma`$ is price volatility;
* $`T-t`$ is the remaining horizon.

The optimal half-spread is:

```math
h_t
=
\frac{\gamma\sigma^2(T-t)}{2}
+
\frac{1}{\gamma}
\ln\left(1 + \frac{\gamma}{k}\right).
```

The optimal quotes are:

```math
b_t^* = r_t - h_t,
\qquad
a_t^* = r_t + h_t.
```

Relative to the mid-price:

```math
\delta_t^b
=
h_t + q_t\gamma\sigma^2(T-t),
```

```math
\delta_t^a
=
h_t - q_t\gamma\sigma^2(T-t).
```

If the market maker is long:

```math
q_t > 0,
```

then:

```math
\delta_t^b > h_t,
\qquad
\delta_t^a < h_t.
```

Therefore:

* the bid becomes less competitive;
* the ask becomes more competitive;
* both quotes shift downward.

This is inventory skewing, not symmetric spread widening.

A technical limitation is that the basic inventory shift approaches zero as:

```math
t \rightarrow T.
```

Therefore, the basic formula alone does not force inventory liquidation near the end. Strong terminal urgency requires an explicit terminal penalty, liquidation cost, hard inventory constraint, or market-order control.

---

## Order-Book Imbalance

Let:

```math
Q_t^b = \text{quantity at the best bid},
```

```math
Q_t^a = \text{quantity at the best ask}.
```

Order-book imbalance is:

```math
I_t
=
\frac{Q_t^b - Q_t^a}{Q_t^b + Q_t^a}.
```

It satisfies:

```math
-1 \leq I_t \leq 1.
```

A positive value means more displayed quantity at the bid than at the ask.

A negative value means more displayed quantity at the ask than at the bid.

Imbalance may predict short-term price pressure, but it is not a guarantee because orders may be cancelled, hidden, or strategically placed.

---

## Microprice

Let:

```math
P_t^b = \text{best bid},
\qquad
P_t^a = \text{best ask}.
```

The mid-price is:

```math
S_t = \frac{P_t^a + P_t^b}{2}.
```

The microprice is:

```math
M_t
=
S_t + \frac{P_t^a - P_t^b}{2} I_t.
```

Equivalently:

```math
M_t
=
\frac{P_t^a Q_t^b + P_t^b Q_t^a}
{Q_t^b + Q_t^a}.
```

If the ask queue is much smaller than the bid queue, the microprice moves closer to the ask because the ask may be depleted first.

---

## Queue Position

Under price-time priority, orders at the same price are executed first-in, first-out.

If 600 shares are ahead of your bid order, your order cannot execute until those 600 shares disappear through:

* sell market orders;
* cancellations specifically ahead of you.

Reaching the front of the queue is not the same as receiving a fill. Another sell market order must still arrive and trade against your own quantity.

---

# Common Theoretical Traps

## A Martingale Does Not Eliminate Adverse Selection

It is possible that:

```math
E[S_{t+\tau} - S_t] = 0
```

but:

```math
E\left[
S_{t+\tau} - S_t
\mid
\text{bid fill}
\right] < 0.
```

The first expectation is unconditional. The second is conditional on an informative event.

---

## Inventory Skew Is Not Spread Widening

Inventory skew changes the center of the quotes.

Spread widening increases the distance between the bid and ask.

The two controls solve different problems.

---

## Intensity Is Not Probability

The value:

```math
\lambda(\delta)
```

is an instantaneous event rate.

The probability over a finite horizon is:

```math
1 - e^{-\lambda(\delta)\Delta t}.
```

---

## More Fills Do Not Necessarily Mean More Profit

A high fill rate may mean that the quote is attractive to informed or faster traders.

A strategy should optimize markout-adjusted PnL, not only executions.

---

## Reaching the Front Is Not a Fill

Queue depletion only determines when an order becomes first in line.

Execution requires additional opposite-side order flow.

---

# Hard Interview Traps

## Question 1: The Martingale Trap

The mid-price satisfies:

```math
E[S_{t+\tau} - S_t] = 0.
```

A market maker posts a bid at:

```math
b_t = S_t - 0.01.
```

Conditional on a bid fill:

```math
E\left[
S_{t+\tau} - S_t
\mid
\text{bid fill}
\right]
=
-0.03.
```

Find the expected markout.

### Solution

For a bid fill:

```math
M_{t,\tau}^b = S_{t+\tau} - b_t.
```

Therefore:

```math
E\left[
M_{t,\tau}^b
\mid
\text{bid fill}
\right]
=
S_t - b_t
+
E\left[
S_{t+\tau} - S_t
\mid
\text{bid fill}
\right].
```

Substituting:

```math
E\left[
M_{t,\tau}^b
\mid
\text{bid fill}
\right]
=
0.01 - 0.03.
```

Thus:

```math
\boxed{
E\left[
M_{t,\tau}^b
\mid
\text{bid fill}
\right]
=
-0.02
}
```

The market maker earns one cent of spread but loses three cents from the conditional price movement.

The martingale property does not prevent adverse selection because the fill is a conditional event.

---

## Question 2: Long Inventory

Suppose:

```math
S_t = 100,
\qquad
q_t = 20,
\qquad
h_t = 0.02,
```

and:

```math
\gamma\sigma^2(T-t) = 0.0005.
```

Find the reservation price and the optimal bid and ask.

### Solution

The inventory adjustment is:

```math
q_t\gamma\sigma^2(T-t)
=
20(0.0005)
=
0.01.
```

Therefore:

```math
r_t
=
100 - 0.01
=
99.99.
```

The optimal bid is:

```math
b_t^*
=
r_t - h_t
=
99.99 - 0.02
=
99.97.
```

The optimal ask is:

```math
a_t^*
=
r_t + h_t
=
99.99 + 0.02
=
100.01.
```

Thus:

```math
\boxed{
b_t^* = 99.97,
\qquad
a_t^* = 100.01
}
```

Relative to the mid-price:

```math
\delta_t^b = 0.03,
\qquad
\delta_t^a = 0.01.
```

The bid moves farther away while the ask moves closer.

The correct response is not to widen both sides symmetrically. The market maker should skew its quotes downward to reduce long inventory.

---

## Question 3: Toxic Order Flow

Suppose fill intensity is:

```math
\lambda(\delta) = A e^{-k\delta}.
```

Each fill earns the quote distance $`\delta`$, but also creates an expected adverse-selection loss $`c`$.

Find the optimal quote distance.

### Solution

Expected profit per unit time is:

```math
R(\delta)
=
A e^{-k\delta}(\delta - c).
```

Differentiate:

```math
R'(\delta)
=
A e^{-k\delta}
\left[
1 - k(\delta - c)
\right].
```

Set:

```math
R'(\delta) = 0.
```

Then:

```math
1 - k(\delta - c) = 0.
```

Therefore:

```math
\delta - c = \frac{1}{k}.
```

Thus:

```math
\boxed{
\delta^*
=
c + \frac{1}{k}
}
```

Without toxicity:

```math
\delta^* = \frac{1}{k}.
```

A constant expected adverse-selection cost widens the optimal quote by exactly $`c`$.

This result depends on the exponential-intensity assumption and constant toxicity.

---

## Question 4: A 60% Fill Probability

A researcher estimates fill probability as:

```math
\frac{\text{number of filled orders}}
{\text{number of submitted orders}}
=
60\%.
```

Why is this estimate generally misleading?

### Solution

Submitted orders are not identical trials.

First, exposure times differ. An order resting for one second and an order resting for one minute do not have the same opportunity to execute.

Second, fill probability depends on state variables such as:

```math
\delta_t,
\qquad
\text{queue position},
\qquad
\text{volatility},
\qquad
\text{imbalance},
\qquad
\text{order flow}.
```

Third, cancellations create censoring. An order may be cancelled before it has enough time to fill.

Fourth, self-cancellation is often informative. The strategy may cancel specifically when market conditions become unfavorable.

The relevant object is a conditional fill intensity:

```math
\lambda_t
=
\lambda\left(
\delta_t,
\text{queue position},
\sigma_t,
I_t,
\text{order flow},
\ldots
\right).
```

Thus:

```math
\boxed{
\frac{\text{fills}}
{\text{submissions}}
\text{ is not a universal fill probability}
}
```

A proper model may require survival analysis, competing risks, or a joint model of fills and cancellations.

---

## Question 5: Queue First-Passage Time

You join the best bid with:

```math
Q_0^{\text{ahead}} = 600
```

shares ahead of you.

Sell market orders arrive according to a Poisson process with:

```math
\lambda_M = 5
\text{ per second}
```

and exponential sizes with mean:

```math
E[V_M] = 70.
```

Cancellations ahead of you arrive with:

```math
\lambda_C = 3
\text{ per second}
```

and exponential sizes with mean:

```math
E[V_C] = 40.
```

Estimate:

1. the probability of reaching the front within two seconds;
2. the expected time conditional on reaching the front.

### Solution

The expected depletion rate from market orders is:

```math
\lambda_M E[V_M]
=
5(70)
=
350.
```

The expected depletion rate from cancellations is:

```math
\lambda_C E[V_C]
=
3(40)
=
120.
```

Total expected depletion is:

```math
350 + 120 = 470
```

shares per second.

A rough time estimate is:

```math
\frac{600}{470}
\approx
1.28\text{ seconds}.
```

This is not an exact first-passage time because depletion occurs through random jumps.

A Monte Carlo simulation is:

```python
import numpy as np


def simulate_queue(
    n_paths: int = 200_000,
    initial_queue: float = 600.0,
    horizon: float = 2.0,
    market_rate: float = 5.0,
    mean_market_size: float = 70.0,
    cancel_rate: float = 3.0,
    mean_cancel_size: float = 40.0,
    seed: int = 7,
) -> tuple[float, float, np.ndarray]:
    """Simulate the time required to reach the front of the queue."""

    rng = np.random.default_rng(seed)

    reached_front = np.zeros(n_paths, dtype=bool)
    front_times = np.full(n_paths, np.nan)

    total_rate = market_rate + cancel_rate

    for path in range(n_paths):
        time = 0.0
        queue_ahead = initial_queue

        while time < horizon and queue_ahead > 0:
            time += rng.exponential(1.0 / total_rate)

            if time > horizon:
                break

            if rng.random() < market_rate / total_rate:
                queue_ahead -= rng.exponential(mean_market_size)
            else:
                queue_ahead -= rng.exponential(mean_cancel_size)

        if queue_ahead <= 0 and time <= horizon:
            reached_front[path] = True
            front_times[path] = time

    probability = reached_front.mean()
    conditional_mean = np.nanmean(front_times)
    quantiles = np.nanquantile(
        front_times,
        [0.10, 0.50, 0.90],
    )

    return probability, conditional_mean, quantiles


probability, mean_time, quantiles = simulate_queue()

print(f"Probability: {probability:.3f}")
print(f"Conditional mean time: {mean_time:.3f}")
print(f"Quantiles: {np.round(quantiles, 3)}")
```

The simulation gives approximately:

```math
\boxed{
P(\text{reach front within 2 seconds})
\approx
0.843
}
```

and:

```math
\boxed{
E\left[
\tau_{\text{front}}
\mid
\tau_{\text{front}} \leq 2
\right]
\approx
1.222\text{ seconds}
}
```

The important final trap is:

```math
\boxed{
P(\text{reach front})
\neq
P(\text{fill})
}
```

After reaching the front, another sell order must still arrive and execute against your own quantity.

Therefore, $`0.843`$ is not the fill probability. It is only the probability of reaching the front under the stated queue model.

---

# The Five Traps Condensed

1. A martingale mid-price does not eliminate adverse selection because executions are conditional events.

2. Long inventory should produce quote skew, not automatically symmetric widening.

3. Under exponential fill intensity and constant toxicity:

```math
\delta^* = c + \frac{1}{k}.
```

4. Fills divided by submissions is generally a biased fill-probability estimate.

5. Reaching the front of the queue is necessary for execution but is not itself a fill.

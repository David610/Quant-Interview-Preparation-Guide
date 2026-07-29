# Advanced Differentiation for Quant Interviews

## 1. Logarithmic Differentiation

Logarithmic differentiation is useful when the variable appears:

* in both the base and exponent;
* in a large product;
* in a likelihood function;
* in a quotient containing many factors.

Suppose:

```math
y=f(x)^{g(x)}.
```

For a real-valued derivation, assume:

```math
f(x)>0
```

on an open interval around the point of interest.

Taking logarithms gives:

```math
\ln(y)=g(x)\ln(f(x)).
```

Differentiate:

```math
\frac{y'}{y}
=
g'(x)\ln(f(x))
+
g(x)\frac{f'(x)}{f(x)}.
```

Therefore:

```math
\boxed{
y'
=
f(x)^{g(x)}
\left[
g'(x)\ln(f(x))
+
g(x)\frac{f'(x)}{f(x)}
\right]
}
```

For:

```math
y=x^x,
\qquad x>0,
```

we obtain:

```math
\boxed{
\frac{d}{dx}x^x
=
x^x[\ln(x)+1]
}
```

### Product form

For:

```math
y=\prod_{i=1}^{n}f_i(x),
```

taking logarithms gives:

```math
\ln(y)
=
\sum_{i=1}^{n}\ln(f_i(x)).
```

Hence:

```math
\boxed{
\frac{y'}{y}
=
\sum_{i=1}^{n}
\frac{f_i'(x)}{f_i(x)}
}
```

This is one reason log-likelihoods are easier to optimize than raw likelihoods.

### Main trap: negative bases

The expression:

```math
f(x)^{g(x)}
```

is not generally a real-valued differentiable function when $`f(x)<0`$ and $`g(x)`$ varies continuously.

For example, $`x^x`$ has real values at some isolated negative rational numbers, but it does not define a real differentiable function on an open negative interval.

The standard logarithmic differentiation formula therefore cannot simply be extended to all negative $`x`$.

---

## 2. Implicit and Inverse Differentiation

### Implicit differentiation

Suppose a curve is defined by:

```math
F(x,y)=0.
```

If the curve can locally be represented as:

```math
y=y(x),
```

then:

```math
F_x(x,y)
+
F_y(x,y)\frac{dy}{dx}
=
0.
```

Therefore:

```math
\boxed{
\frac{dy}{dx}
=
-\frac{F_x}{F_y}
}
```

This requires:

```math
F_y(x,y)\neq0.
```

Together with continuous differentiability of $`F`$, this is the key condition in the implicit function theorem.

### Vertical-tangent trap

For the unit circle:

```math
F(x,y)=x^2+y^2-1,
```

we obtain:

```math
\frac{dy}{dx}
=
-\frac{x}{y}.
```

At:

```math
(1,0),
```

the formula divides by zero.

This does not mean that the curve ceases to exist. It means that the curve cannot locally be represented as a differentiable function $`y(x)`$. The tangent is vertical.

At the same point, we may instead treat $`x`$ as a function of $`y`$:

```math
\frac{dx}{dy}
=
-\frac{F_y}{F_x}
=
-\frac{y}{x}.
```

At $`(1,0)`$:

```math
\frac{dx}{dy}=0.
```

### Inverse differentiation

Let:

```math
y_0=f(x_0).
```

If $`f`$ is continuously differentiable near $`x_0`$ and:

```math
f'(x_0)\neq0,
```

then:

```math
\boxed{
(f^{-1})'(y_0)
=
\frac{1}{f'(x_0)}
}
```

### Main trap: invertibility is not enough

The function:

```math
f(x)=x^3
```

is globally invertible, but:

```math
f'(0)=0.
```

Its inverse is:

```math
f^{-1}(y)=y^{1/3},
```

whose derivative becomes unbounded at $`y=0`$.

Global invertibility does not guarantee that the inverse has a finite derivative.

### Quant connection

Implied volatility is an inverse problem:

```math
C_{\mathrm{BS}}(\sigma)
=
C_{\mathrm{market}}.
```

Locally:

```math
\frac{d\sigma}{dC}
=
\frac{1}{\mathrm{Vega}}.
```

When vega is close to zero, a small option-pricing error can create a very large implied-volatility error.

---

## 3. Higher Derivatives and Curvature

The second derivative measures how the first derivative changes:

```math
f''(x)
=
\frac{d}{dx}f'(x).
```

If:

```math
f''(x)>0,
```

the function is locally convex.

If:

```math
f''(x)<0,
```

the function is locally concave.

### Second-order test

At a stationary point:

```math
f'(x^*)=0,
```

the following implications hold:

```math
f''(x^*)>0
\quad\Longrightarrow\quad
\text{strict local minimum},
```

```math
f''(x^*)<0
\quad\Longrightarrow\quad
\text{strict local maximum}.
```

If:

```math
f''(x^*)=0,
```

the test is inconclusive.

For:

```math
f(x)=x^4,
```

we have:

```math
f'(0)=0,
\qquad
f''(0)=0,
```

but $`0`$ is a strict global minimum.

For:

```math
f(x)=x^3,
```

the same derivative conditions hold, but $`0`$ is not an extremum.

### Inflection-point trap

The condition:

```math
f''(x^*)=0
```

does not prove that $`x^*`$ is an inflection point.

A change in concavity must be shown.

For example:

```math
f(x)=x^4
```

has $`f''(0)=0`$, but the function is convex on both sides of zero.

### Quant interpretation

For an option value $`V(S)`$:

```math
\Delta
=
\frac{\partial V}{\partial S},
\qquad
\Gamma
=
\frac{\partial^2V}{\partial S^2}.
```

Delta measures first-order exposure.

Gamma measures how delta changes when the underlying price changes.

For bonds, the first derivative with respect to yield is related to duration, while the second derivative is related to convexity.

---

## 4. Partial Derivatives, Gradients, and Hessians

Suppose:

```math
f=f(x_1,\ldots,x_n).
```

The gradient is:

```math
\nabla f
=
\begin{bmatrix}
\frac{\partial f}{\partial x_1}\\
\vdots\\
\frac{\partial f}{\partial x_n}
\end{bmatrix}.
```

The Hessian is:

```math
H_f
=
\left[
\frac{\partial^2f}
{\partial x_i\partial x_j}
\right]_{i,j=1}^{n}.
```

The gradient gives local first-order sensitivities.

The Hessian describes:

* curvature in individual variables;
* interactions between variables;
* how the gradient changes.

### Partial versus total derivative

If:

```math
V=V(S,t)
```

and:

```math
S=S(t),
```

then in ordinary deterministic calculus:

```math
\frac{dV}{dt}
=
\frac{\partial V}{\partial t}
+
\frac{\partial V}{\partial S}
\frac{dS}{dt}.
```

The partial derivative holds the other variables fixed.

The total derivative includes indirect dependence through $`S(t)`$.

If $`S`$ follows a stochastic diffusion, this formula is incomplete. Itô's lemma must be used because the quadratic variation contributes an additional term.

### Mixed-partial trap

It is often written that:

```math
\frac{\partial^2f}{\partial x\,\partial y}
=
\frac{\partial^2f}{\partial y\,\partial x}.
```

This is not automatic.

A standard sufficient condition is that the second partial derivatives are continuous in a neighbourhood of the point.

Without suitable regularity, both mixed partial derivatives may exist and still be unequal.

### Scaling trap

Derivative magnitudes depend on units.

A derivative with respect to volatility represented as $`0.20`$ is numerically different from a derivative with respect to volatility represented as $`20`$ percent.

Raw gradient components should not be compared without considering units and parameter scaling.

---

## 5. Taylor Approximation

For a sufficiently smooth scalar function:

```math
f(x+h)
=
f(x)
+
f'(x)h
+
\frac{1}{2}f''(x)h^2
+
R_3.
```

If $`f`$ is three times differentiable, the Lagrange remainder is:

```math
R_3
=
\frac{1}{6}f'''(\xi)h^3
```

for some $`\xi`$ between $`x`$ and $`x+h`$.

A first-order approximation:

```math
f(x+h)
\approx
f(x)+f'(x)h
```

is reliable only when the neglected curvature and the move $`h`$ are sufficiently small.

### Multivariable Taylor approximation

For a vector change $`h`$:

```math
f(x+h)
\approx
f(x)
+
\nabla f(x)^\top h
+
\frac{1}{2}h^\top H_f(x)h.
```

The quadratic term includes mixed effects.

For an option price:

```math
V=V(S,\sigma,t),
```

a more complete approximation may contain:

```math
\Delta V
\approx
\Delta\,\Delta S
+
\mathrm{Vega}\,\Delta\sigma
+
\Theta\,\Delta t
+
\frac{1}{2}\Gamma(\Delta S)^2
+
\mathrm{Vanna}\,\Delta S\,\Delta\sigma
+
\frac{1}{2}\mathrm{Volga}(\Delta\sigma)^2.
```

A delta-gamma approximation is therefore not a full second-order approximation when volatility and time also move.

### Non-smooth-payoff trap

The payoff:

```math
\max(S-K,0)
```

is not differentiable at:

```math
S=K.
```

Taylor expansion cannot be applied directly to the terminal payoff at the strike.

Before maturity, the Black–Scholes price is smooth in $`S`$, but gamma near the strike grows sharply as maturity approaches. The region in which a quadratic approximation is reliable becomes smaller.

### Taylor versus Itô

For:

```math
dS=\mu S\,dt+\sigma S\,dW,
```

Itô calculus uses:

```math
(dW)^2=dt.
```

Therefore:

```math
(dS)^2
```

contributes at order $`dt`$.

This produces the term:

```math
\frac{1}{2}
\sigma^2S^2
\frac{\partial^2V}{\partial S^2}
dt.
```

Discarding every quadratic term before applying Itô's lemma is incorrect.

---

## 6. Optimization

### Unconstrained problems

For:

```math
f:\mathbb{R}^n\rightarrow\mathbb{R},
```

an interior local optimum must satisfy:

```math
\nabla f(x^*)=0.
```

This condition is necessary, not sufficient.

The Hessian gives:

* positive definite: strict local minimum;
* negative definite: strict local maximum;
* indefinite: saddle point;
* singular semidefinite: inconclusive.

### Positive-semidefinite trap

Knowing only that:

```math
H_f(x^*)\succeq0
```

at one stationary point does not prove a strict minimum.

For example, both $`x^4`$ and $`x^3`$ have zero second derivative at zero, but only one has a minimum.

To prove convexity through the Hessian, one generally needs:

```math
H_f(x)\succeq0
```

throughout a convex domain.

### Constraints and boundaries

For:

```math
0\leq w\leq1,
```

solving:

```math
f'(w)=0
```

is not enough.

The candidate set contains:

* feasible interior stationary points;
* $`w=0`$;
* $`w=1`$.

For more general constraints, Karush–Kuhn–Tucker conditions may be required.

A common mistake is to solve the unconstrained problem and then simply remove infeasible variables without re-solving the optimization problem.

---

## 7. Quant Applications

### 7.1 Market-Making Quote Optimization

Suppose fill intensity is:

```math
\lambda(\delta)
=
Ae^{-k\delta},
\qquad
A>0,
\quad
k>0.
```

Each fill earns the quote distance $`\delta`$ but creates an expected adverse-selection cost $`c`$.

Expected profit per unit time is:

```math
R(\delta)
=
Ae^{-k\delta}(\delta-c).
```

Differentiate:

```math
R'(\delta)
=
Ae^{-k\delta}
\left[
1-k(\delta-c)
\right].
```

The stationary point is:

```math
\boxed{
\delta^*
=
c+\frac{1}{k}
}
```

At this point:

```math
R''(\delta^*)
=
-kAe^{-k\delta^*}
<0.
```

The point is a local maximum.

Because $`R'`$ changes sign only once, it is also the global maximum over an unrestricted continuous domain.

The formula is not universal. It depends on:

* exponential fill intensity;
* constant toxicity;
* continuous quote distances;
* no inventory constraint;
* no tick-size restriction.

---

### 7.2 Bond Duration and Convexity

For:

```math
P(y)
=
\sum_{t=1}^{T}
\frac{CF_t}{(1+y)^t},
```

the first derivative is:

```math
P'(y)
=
-\sum_{t=1}^{T}
\frac{tCF_t}{(1+y)^{t+1}}.
```

Modified duration is:

```math
D_{\mathrm{mod}}
=
-\frac{P'(y)}{P(y)}.
```

The second derivative is:

```math
P''(y)
=
\sum_{t=1}^{T}
\frac{t(t+1)CF_t}{(1+y)^{t+2}}.
```

Normalized convexity is:

```math
C
=
\frac{P''(y)}{P(y)}.
```

Therefore:

```math
\boxed{
\frac{\Delta P}{P}
\approx
-D_{\mathrm{mod}}\Delta y
+
\frac{1}{2}C(\Delta y)^2
}
```

This approximation can fail when:

* the yield move is large;
* the yield curve moves non-parallel;
* cash flows depend on interest rates;
* the bond contains embedded options;
* compounding conventions are mixed.

---

### 7.3 Bernoulli Maximum Likelihood

For $`s`$ successes in $`n`$ trials:

```math
\ell(p)
=
s\ln(p)
+
(n-s)\ln(1-p).
```

The first derivative is:

```math
\ell'(p)
=
\frac{s}{p}
-
\frac{n-s}{1-p}.
```

For:

```math
0<s<n,
```

the interior solution is:

```math
\boxed{
\hat p=\frac{s}{n}
}
```

The second derivative is:

```math
\ell''(p)
=
-\frac{s}{p^2}
-
\frac{n-s}{(1-p)^2}
<0.
```

The log-likelihood is strictly concave in the interior.

If $`s=0`$ or $`s=n`$, there is no interior maximizer. The optimum lies at the boundary.

---

### 7.4 Delta and Gamma

For a European call in Black–Scholes on a non-dividend-paying underlying:

```math
\Delta_{\mathrm{call}}
=
\Phi(d_1),
```

```math
\Gamma
=
\frac{\phi(d_1)}
{S\sigma\sqrt{\tau}},
```

where:

```math
d_1
=
\frac{
\ln(S/K)
+
\left(r+\frac{1}{2}\sigma^2\right)\tau
}{
\sigma\sqrt{\tau}
}.
```

For a put:

```math
\Delta_{\mathrm{put}}
=
\Phi(d_1)-1.
```

The delta-gamma approximation is:

```math
\Delta V
\approx
\Delta\,\Delta S
+
\frac{1}{2}\Gamma(\Delta S)^2.
```

The approximation assumes that delta and gamma measured at the initial point remain sufficiently informative over the move.

A common conceptual error is to say that call delta is the risk-neutral probability of finishing in the money.

In Black–Scholes:

```math
\Phi(d_2)
```

is the risk-neutral exercise probability.

Delta is:

```math
\Phi(d_1).
```

---

### 7.5 Implied Volatility and Newton's Method

Define:

```math
F(\sigma)
=
C_{\mathrm{BS}}(\sigma)
-
C_{\mathrm{market}}.
```

Newton's method gives:

```math
\boxed{
\sigma_{n+1}
=
\sigma_n
-
\frac{
C_{\mathrm{BS}}(\sigma_n)
-
C_{\mathrm{market}}
}{
\mathrm{Vega}(\sigma_n)
}
}
```

The method may fail when:

* vega is close to zero;
* the market price violates no-arbitrage bounds;
* the initial guess is poor;
* the next volatility becomes negative;
* maturity is extremely short;
* the option is deep in or out of the money.

A safer implementation combines Newton steps with a bracketed method such as bisection.

---

### 7.6 Portfolio Optimization

Consider:

```math
U(w)
=
\mu^\top w
-
\frac{\gamma}{2}
w^\top\Sigma w.
```

The gradient is:

```math
\nabla U(w)
=
\mu-\gamma\Sigma w.
```

If $`\Sigma`$ is invertible and there are no constraints:

```math
\boxed{
w^*
=
\frac{1}{\gamma}
\Sigma^{-1}\mu
}
```

The Hessian is:

```math
H_U=-\gamma\Sigma.
```

If $`\Sigma`$ is positive definite, the objective is strictly concave and the stationary point is the unique global maximum.

The formula becomes unreliable when:

* $`\Sigma`$ is ill-conditioned;
* expected returns are estimated imprecisely;
* weights must sum to one;
* short selling is restricted;
* turnover costs are present;
* exposure limits are imposed.

---

## 8. Numerical Differentiation

The central-difference approximation is:

```math
f'(x)
\approx
\frac{f(x+h)-f(x-h)}{2h}.
```

Its truncation error is normally:

```math
O(h^2).
```

However, when $`h`$ is extremely small, subtraction of nearly equal floating-point values creates cancellation error.

A minimal Python check is:

```python
def central_difference(f, x, h=1e-5):
    return (f(x + h) - f(x - h)) / (2 * h)


for h in [1e-3, 1e-4, 1e-5, 1e-6]:
    print(h, central_difference(lambda z: z**3, 2.0, h))
```

A derivative should not be validated using only one step size.

### Complex-step trap

For analytic functions:

```math
f'(x)
\approx
\frac{\mathrm{Im}[f(x+ih)]}{h}.
```

This avoids subtractive cancellation.

But it fails when the implementation contains:

* absolute values;
* clipping;
* `max` or `min`;
* discontinuous branching;
* code that silently discards complex parts.

---

## 9. Hard Interview Traps

1. **Logarithmic differentiation with a negative base**
   The standard real-valued derivation requires positivity on an interval.

2. **Dividing by $`F_y`$ without checking it**
   A zero denominator may represent a vertical tangent.

3. **Assuming invertibility implies a differentiable inverse**
   The original derivative must be nonzero.

4. **Using $`f''(x^*)=0`$ to prove an inflection point**
   Concavity must actually change.

5. **Treating a singular positive-semidefinite Hessian as conclusive**
   Higher-order terms may determine the result.

6. **Ignoring mixed Taylor terms**
   Cross-sensitivities such as vanna can matter.

7. **Applying Taylor expansion to a payoff kink**
   The required derivative may not exist.

8. **Dropping every quadratic differential in stochastic calculus**
   In Itô calculus, $`(dW)^2=dt`$.

9. **Solving only the interior optimization problem**
   Boundaries and KKT conditions must be checked.

10. **Assuming mixed partial derivatives always commute**
    Regularity assumptions are required.

11. **Applying Newton's method when vega is almost zero**
    The update can become arbitrarily large.

12. **Assuming smaller finite-difference steps are always better**
    Roundoff error eventually dominates.

13. **Comparing sensitivities expressed in different units**
    Scaling changes their numerical values.

14. **Calling delta the risk-neutral exercise probability**
    In Black–Scholes that role belongs to $`\Phi(d_2)`$.

15. **Inverting a poorly conditioned covariance matrix**
    Tiny estimation errors can create extreme portfolio weights.

---

## 10. Interview Problems

### Problem 1: A parameter-dependent exponent

Differentiate:

```math
f(x)
=
\left(1+\frac{a}{x}\right)^x,
\qquad
x>0,
```

where $`a`$ is constant.

#### Solution

Take logarithms:

```math
\ln f(x)
=
x\ln\left(1+\frac{a}{x}\right).
```

Differentiate:

```math
\frac{f'(x)}{f(x)}
=
\ln\left(1+\frac{a}{x}\right)
+
x
\frac{
-\frac{a}{x^2}
}{
1+\frac{a}{x}
}.
```

Simplify:

```math
\frac{f'(x)}{f(x)}
=
\ln\left(1+\frac{a}{x}\right)
-
\frac{a}{x+a}.
```

Therefore:

```math
\boxed{
f'(x)
=
\left(1+\frac{a}{x}\right)^x
\left[
\ln\left(1+\frac{a}{x}\right)
-
\frac{a}{x+a}
\right]
}
```

#### Interview trap

Do not treat $`x`$ as appearing only in the exponent. It also appears inside the base.

---

### Problem 2: Implicit curve with a singular point

The curve is:

```math
x^3+y^3-3xy=0.
```

Find $`dy/dx`$ and explain what happens at the origin.

#### Solution

Differentiate:

```math
3x^2
+
3y^2\frac{dy}{dx}
-
3y
-
3x\frac{dy}{dx}
=
0.
```

Therefore:

```math
\boxed{
\frac{dy}{dx}
=
\frac{y-x^2}{y^2-x}
}
```

At $`(0,0)`$, both numerator and denominator vanish.

The implicit-function condition fails because:

```math
F_y(0,0)=0.
```

The origin is a self-intersection point with multiple local branches, so there is no single derivative representing the whole curve there.

---

### Problem 3: Inverse-function instability

An option has:

```math
\mathrm{Vega}=0.002
```

per unit of volatility.

The observed option price contains an error of:

```math
\Delta C=0.01.
```

Estimate the local implied-volatility error.

#### Solution

Using inverse sensitivity:

```math
\Delta\sigma
\approx
\frac{\Delta C}{\mathrm{Vega}}.
```

Thus:

```math
\Delta\sigma
\approx
\frac{0.01}{0.002}
=
5.
```

The formal estimate is:

```math
\boxed{
\Delta\sigma\approx5
}
```

This means 500 volatility percentage points.

#### Interview trap

The answer is not economically meaningful as a linear approximation. Its purpose is to reveal that the inverse problem is severely ill-conditioned.

---

### Problem 4: A failed second-order test

Classify $`x=0`$ for:

```math
f(x)=x^6-x^8.
```

#### Solution

The first derivative is:

```math
f'(x)
=
6x^5-8x^7.
```

Therefore:

```math
f'(0)=0.
```

The second derivative is:

```math
f''(x)
=
30x^4-56x^6,
```

so:

```math
f''(0)=0.
```

The second-order test is inconclusive.

Factor the function:

```math
f(x)
=
x^6(1-x^2).
```

For sufficiently small nonzero $`x`$:

```math
x^6>0,
\qquad
1-x^2>0.
```

Therefore:

```math
f(x)>f(0)=0.
```

Hence:

```math
\boxed{
x=0
\text{ is a strict local minimum}
}
```

---

### Problem 5: Hessian with a misleading determinant

Classify the stationary point of:

```math
f(x,y)
=
x^2-4xy+y^2.
```

#### Solution

The gradient is:

```math
\nabla f
=
\begin{bmatrix}
2x-4y\\
-4x+2y
\end{bmatrix}.
```

The only stationary point is:

```math
(x,y)=(0,0).
```

The Hessian is:

```math
H
=
\begin{bmatrix}
2 & -4\\
-4 & 2
\end{bmatrix}.
```

Its determinant is:

```math
\det(H)
=
4-16
=
-12.
```

A negative determinant means the eigenvalues have opposite signs.

Therefore:

```math
\boxed{
(0,0)
\text{ is a saddle point}
}
```

---

### Problem 6: Mixed-partial derivatives

Suppose both mixed partial derivatives exist at a point. Must they be equal?

#### Solution

No.

The equality:

```math
f_{xy}=f_{yx}
```

requires additional regularity.

A standard sufficient condition is continuity of the second partial derivatives in a neighbourhood of the point.

#### Interview trap

Existence at one point is weaker than continuity around the point.

---

### Problem 7: Delta-gamma approximation

An option has:

```math
\Delta=0.40,
\qquad
\Gamma=0.06.
```

The underlying moves by:

```math
\Delta S=-3.
```

Estimate the option-price change.

#### Solution

Use:

```math
\Delta V
\approx
\Delta\,\Delta S
+
\frac{1}{2}\Gamma(\Delta S)^2.
```

Then:

```math
\Delta V
\approx
0.40(-3)
+
\frac{1}{2}(0.06)(9).
```

Therefore:

```math
\Delta V
\approx
-1.20+0.27.
```

Thus:

```math
\boxed{
\Delta V\approx-0.93
}
```

#### Interview trap

For positive gamma, the gamma contribution is positive for both upward and downward moves because $`(\Delta S)^2`$ is positive.

---

### Problem 8: Delta is not the exercise probability

A candidate says:

> A call delta of 0.65 means there is a 65% risk-neutral probability that the option expires in the money.

Is this correct?

#### Solution

Not generally.

In the Black–Scholes model for a non-dividend-paying underlying:

```math
\Delta_{\mathrm{call}}
=
\Phi(d_1).
```

The risk-neutral probability of finishing in the money is:

```math
\Phi(d_2).
```

Therefore, the claim confuses $`d_1`$ with $`d_2`$.

Delta is fundamentally a sensitivity:

```math
\Delta
=
\frac{\partial V}{\partial S}.
```

---

### Problem 9: Market-making quote with a tick constraint

The continuous optimizer is:

```math
\delta^*
=
0.037.
```

Quotes must be multiples of:

```math
0.01.
```

Which quote distance is optimal?

#### Solution

The continuous first-order condition no longer gives an admissible quote.

The nearby feasible candidates are:

```math
0.03
\quad\text{and}\quad
0.04.
```

Evaluate the original objective:

```math
R(\delta)
=
Ae^{-k\delta}(\delta-c)
```

at both values.

The better value is the discrete optimum.

#### Interview trap

Rounding the continuous optimizer automatically is not always correct. Both neighbouring feasible points should be checked.

---

### Problem 10: Boundary Bernoulli MLE

All $`n`$ Bernoulli observations are successes.

Find the maximum-likelihood estimate.

#### Solution

The likelihood is:

```math
L(p)=p^n.
```

It is increasing on:

```math
0\leq p\leq1.
```

Therefore:

```math
\boxed{
\hat p=1
}
```

There is no interior stationary point.

The log-likelihood derivative:

```math
\ell'(p)=\frac{n}{p}
```

is positive throughout the interior.

---

### Problem 11: Constrained portfolio weight

Maximize:

```math
U(w)
=
\mu w
-
\frac{\gamma}{2}
\sigma^2w^2
```

subject to:

```math
0\leq w\leq1.
```

#### Solution

The unconstrained stationary point is:

```math
w^*
=
\frac{\mu}{\gamma\sigma^2}.
```

The constrained solution is:

```math
\boxed{
w_{\mathrm{opt}}
=
\min
\left(
1,
\max
\left(
0,
\frac{\mu}{\gamma\sigma^2}
\right)
\right)
}
```

#### Interview trap

The first-order condition only applies to an interior optimum. The boundaries must be checked.

---

### Problem 12: Newton's method with small vega

Suppose an implied-volatility iteration gives:

```math
C_{\mathrm{BS}}(\sigma_n)
-
C_{\mathrm{market}}
=
0.20
```

and:

```math
\mathrm{Vega}(\sigma_n)
=
0.001.
```

What is the Newton update?

#### Solution

The update is:

```math
\sigma_{n+1}
=
\sigma_n
-
\frac{0.20}{0.001}.
```

Therefore:

```math
\boxed{
\sigma_{n+1}
=
\sigma_n-200
}
```

This is an invalid volatility update.

#### Interview trap

Newton's formula is mathematically defined, but the iteration is numerically useless because vega is too small.

A bracketed fallback should be used.

---

### Problem 13: Bond duration approximation

A bond has:

```math
D_{\mathrm{mod}}=6,
\qquad
C=45.
```

Yield rises by:

```math
\Delta y=0.01.
```

Estimate the percentage price change.

#### Solution

Use:

```math
\frac{\Delta P}{P}
\approx
-D_{\mathrm{mod}}\Delta y
+
\frac{1}{2}C(\Delta y)^2.
```

Then:

```math
\frac{\Delta P}{P}
\approx
-6(0.01)
+
\frac{1}{2}(45)(0.01)^2.
```

Therefore:

```math
\frac{\Delta P}{P}
\approx
-0.06+0.00225.
```

Thus:

```math
\boxed{
\frac{\Delta P}{P}
\approx
-0.05775
}
```

The estimated price decrease is approximately $`5.775\%`$.

---

### Problem 14: Numerical derivative disagreement

A central-difference estimate changes significantly when $`h`$ moves from $`10^{-5}`$ to $`10^{-9}`$.

Give at least four possible explanations.

#### Solution

Possible explanations include:

* floating-point cancellation;
* a non-smooth function;
* evaluation close to a discontinuity;
* poor variable scaling;
* an unstable implementation;
* an incorrect analytical derivative used for comparison;
* branch logic changing between $`x-h`$ and $`x+h`$.

A derivative check should compare several step sizes rather than trusting the smallest one.

---

### Problem 15: Itô correction

Let:

```math
V(S)=S^2
```

and:

```math
dS=\mu S\,dt+\sigma S\,dW.
```

Find $`dV`$ using Itô's lemma.

#### Solution

We have:

```math
V_S=2S,
\qquad
V_{SS}=2.
```

Itô's lemma gives:

```math
dV
=
V_S\,dS
+
\frac{1}{2}V_{SS}(dS)^2.
```

Since:

```math
(dS)^2
=
\sigma^2S^2dt,
```

we obtain:

```math
dV
=
2S(\mu S\,dt+\sigma S\,dW)
+
\sigma^2S^2dt.
```

Therefore:

```math
\boxed{
dV
=
(2\mu+\sigma^2)S^2dt
+
2\sigma S^2dW
}
```

#### Interview trap

Ordinary chain-rule differentiation would miss the $`\sigma^2S^2dt`$ term.

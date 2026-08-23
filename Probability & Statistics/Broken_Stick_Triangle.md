# The Broken Stick Problem

## Task Description

A stick of length $1$ is broken at a uniformly random point, creating two pieces. The longer piece is then chosen and broken again at a uniformly random point along its length. After these two breaks, we have three pieces in total.
What is the probability that these three pieces can form a triangle?

## Solution

Imagine holding a spaghetti noodle of length $1$. Let us walk through the whole solution step by step. Start picturing it in your head.

### 1. When do three pieces make a triangle?

Think about trying to build a triangle with three sticks. If one stick is too long, specifically, half the total length or longer then it is impossible. The other two sticks together won’t even be able to reach across it to meet at a point!
So, our golden rule is simple: A triangle is possible if and only if every single piece is strictly shorter than $\frac{1}{2}$.

### 2. The First Snap

We snap the stick at a random spot. One piece will naturally be the shorter piece and the other will be the longer piece.

Let’s call the length of this longer piece $L$. Because it is the “longer” half, $L$ can be anywhere from $\frac{1}{2}$ up to $1$, with every size in between being equally likely. 
The leftover shorter piece has length $1 - L$, which is already safely smaller than $\frac{1}{2}$. So that piece is good to go!

### 3. The Second Snap

Now, we take only the longer piece (length $L$) and snap it into two smaller pieces. To form a triangle, neither of these new two pieces can be $\frac{1}{2}$ or longer. 
This means we cannot snap too close to the left edge and we cannot snap too close to the right edge.

We must leave this “danger zone” of size $L - \frac{1}{2}$ on each end of the stick. The “safe zone” in the middle where we are allowed to snap has a length of exactly $1 - L$.

Since the total length of the piece we are cutting is $L$, the chance that our random cut hits the safe zone is:

$$\text{Probability (given } L) = \frac{\text{Safe Zone Length}}{\text{Total Length}} = \frac{1 - L}{L}$$

Notice what this means:

If our first cut gave a piece that was barely longer than half ($L \approx 0{,}5$), the chance of making a triangle is almost 
$100\%$. If our first cut gave almost the whole stick ($L \approx 1$), the chance drops down to near $0\%$.


### 4. Averaging Over All Possibilities

Because the first cut $L$ is equally likely to be anywhere between $\frac{1}{2}$ and $1$, we just need to take the average of all these probabilities: $$\text{Average Probability} = \int_{1/2}^1 \frac{1 - L}{L} \cdot 2 \, dL$$

Solving this gives:

$$= 2 \int_{1/2}^1 \left(\frac{1}{L} - 1\right) dL$$

$$= 2 \Big[ \ln(L) - L \Big]_{1/2}^1$$

$$= 2\ln(2) - 1$$

Final Answer

$$2\ln(2) - 1 \approx 0{,}3863 \quad (\text{around } 38{,}63)\%)$$

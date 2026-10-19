# Example note: The Graph Laplacian

This is a complete note produced with the problem-first teaching-notes method. Use it as a model of tone, length and structure. Assumed audience: students who know basic linear algebra (matrices, eigenvectors) and gradients, but have not seen graph Laplacians.

---

# The Graph Laplacian

*How do we measure whether a signal on a network respects the network, and what are the network's own natural patterns?*

## 1. Where does this problem live?

**Many datasets are a network plus a number sitting on every node.**

- **The brain.** Nodes are brain regions; connections are nerve-fibre bundles between them. On each region we also measure something: cortical thickness, activity, gene expression.
- **A road map.** Nodes are cities; connections are highways, stronger when there are more lanes. On each city we record a number: temperature, house prices, flu cases.
- **A social network.** Nodes are people; connections are friendships. On each person we have an opinion score or whether they bought a product.

In every case there are two separate things: the **network** (who is connected to whom, and how strongly) and the **signal** (a number on each node). This note is about how the two relate.

## 2. What do we want to know?

**We want one number that says whether a signal "respects" the network, and then the patterns that respect it most.**

Start with a plain-language question. Do strongly connected regions have similar values? Do cities joined by big highways have similar flu rates? If yes, the signal is **smooth** on the network: it changes little across strong connections. If neighbours look unrelated, the signal is **rough**.

To make this computable we sharpen it into two goals:

1. **A score.** Given any signal, compute a single number that is small when the signal is smooth and large when it is rough.
2. **The network's own patterns.** Ask the reverse question: of all possible signals, which ones are the smoothest? Those patterns are not about any particular measurement. They are a fingerprint of the network itself, and we will see they reveal its clusters.

## 3. What do we actually have?

**Two ingredients: a weight matrix W for the network and a vector x for the signal.**

- **n nodes**, labelled 1, …, n.
- **The weight matrix W.** Entry w_ij ≥ 0 is the strength of the connection between nodes i and j. w_ij = 0 means no connection. Connections here go both ways, so w_ij = w_ji and W is symmetric.
- **The signal x = (x_1, …, x_n).** x_i is the number measured on node i.

**Running example.** Four cities on a line. Cities 1 and 2 are joined by a big highway (weight 3), cities 3 and 4 likewise (weight 3), and a small country road links 2 and 3 (weight 1). So there are two tight pairs joined by one weak link.

$$
W = \begin{pmatrix} 0 & 3 & 0 & 0 \\ 3 & 0 & 1 & 0 \\ 0 & 1 & 0 & 3 \\ 0 & 0 & 3 & 0 \end{pmatrix}
$$

The "obvious" clusters are {1, 2} and {3, 4}. We will check that the mathematics finds them without being told.

## 4. What would we try first?

**The obvious scores either ignore the network or confuse "similar" with "large".**

Take two signals on our four cities:

- a = (1, 1, −1, −1): each tight pair agrees; the jump happens across the weak country road. This *should* count as smooth.
- b = (1, −1, 1, −1): every highway joins opposite values. This *should* count as rough.

**Idea 1: variance.** Variance measures how spread out the values are. But a and b both have mean 0 and variance 1. Variance cannot tell them apart because it never looks at W. Any useful score must use the connections.

**Idea 2: xᵀWx = Σ_ij w_ij x_i x_j.** This uses W: each connection contributes the product of its two end values, so agreeing neighbours add and disagreeing neighbours subtract. It ranks our pair correctly (a gives 10, b gives −14). But try two perfectly smooth signals, all ones and all twos. Neither varies across any road, yet they score 14 and 56. The score grows with the *size* of the values, not with how much they *differ*.

The lesson: smoothness is about **differences between neighbours**, so the score should be built from x_i − x_j directly.

## 5. How do we build a better score?

**Penalise every connection by its weight times the squared difference across it.**

1. **Look at the difference x_i − x_j.** Equal neighbours should cost nothing, and a constant signal should score exactly 0.
2. **Square it.** Going up or down across a road should count the same. Squaring removes the sign, punishes big jumps more than small ones, and turns the score into something matrix algebra can handle. The absolute value would also remove the sign but loses that last benefit.
3. **Multiply by w_ij.** A jump across a big highway is more surprising than a jump across a country road, so strong connections count more.
4. **Add up over all connections.** Summing over all pairs (i, j) counts each road twice, so we put ½ in front.

$$
E(x) = \frac{1}{2}\sum_{i=1}^{n}\sum_{j=1}^{n} w_{ij}\,(x_i - x_j)^2
$$

Check the problem cases. Any constant signal gives E = 0. For a, only the country road has a jump: E = 1 × 2² = 4. For b, every road has a jump of 2: E = 3×4 + 1×4 + 3×4 = 28. Same variance, but b is seven times rougher.

## 6. Where does the matrix come from?

**Expanding the score turns it into xᵀLx, and the matrix L = D − W is the graph Laplacian.**

The score is quadratic in x, so it can be written as xᵀMx for some matrix M. Expanding the square:

$$
E(x) = \frac{1}{2}\sum_{i,j} w_{ij}\left(x_i^2 - 2x_i x_j + x_j^2\right) = \sum_i d_i x_i^2 - \sum_{i,j} w_{ij}\, x_i x_j
$$

Here d_i = Σ_j w_ij is the total connection strength of node i, its **degree**. With D = diag(d_1, …, d_n):

$$
E(x) = x^\top D x - x^\top W x = x^\top (D - W)\, x = x^\top L\, x, \qquad L = D - W
$$

**The matrix L = D − W is called the graph Laplacian.** It is the matrix that computes our roughness score. Idea 2's term xᵀWx is still there, but the degree term now corrects it: for a constant signal the two are equal and E = 0.

For the four cities, the degrees are (3, 4, 4, 3), so

$$
L = \begin{pmatrix} 3 & -3 & 0 & 0 \\ -3 & 4 & -1 & 0 \\ 0 & -1 & 4 & -3 \\ 0 & 0 & -3 & 3 \end{pmatrix}
$$

**What L does to a single node:** (Lx)_i = Σ_j w_ij (x_i − x_j), how much node i sticks out above its neighbours. Every row sums to 0, so L sends constant signals to 0. The same quantity drives diffusion, dx/dt = −Lx.

## 7. Which patterns are the smoothest?

**The smoothest patterns on a network are the eigenvectors of L, and each eigenvalue is that pattern's roughness.**

Two constraints make the question meaningful: fix the size (xᵀx = 1), and rule out the constant pattern (xᵀ**1** = 0).

$$
\min_{x}\; x^\top L\, x \quad \text{subject to} \quad x^\top x = 1,\;\; x^\top \mathbf{1} = 0
$$

**Why the answer is an eigenvector.** At the minimum, no allowed nudge can lower E. The gradient 2Lx must therefore point only in forbidden directions: along x (changes the size) or along **1** (changes the sum). Because L**1** = 0 and L is symmetric, the **1** part is zero, leaving Lx = λx, and then E(x) = λ.

1. **Eigenvalues are never negative,** because E is a sum of squares.
2. **λ_1 = 0 always,** with the constant pattern as eigenvector.
3. **The second eigenvector (the Fiedler vector) is the smoothest non-trivial pattern.** Small λ_2 signals a weak link between two groups. Later eigenvectors run from slow, large-scale patterns to jagged ones.

**Zero eigenvalues count pieces:** the number of zero eigenvalues equals the number of connected components.

## 8. What does this give on the four cities?

**The smoothest non-trivial pattern splits the cities into {1, 2} and {3, 4}.**

| k | λ_k (roughness) | Eigenvector (city 1, 2, 3, 4) | What the pattern does |
| --- | --- | --- | --- |
| 1 | 0 | (0.50, 0.50, 0.50, 0.50) | Constant |
| 2 | 4 − √10 ≈ 0.84 | (0.57, 0.41, −0.41, −0.57) | Pair {1, 2} up, pair {3, 4} down |
| 3 | 6 | (−0.50, 0.50, 0.50, −0.50) | Middle cities against outer cities |
| 4 | 4 + √10 ≈ 7.16 | (−0.41, 0.57, −0.57, 0.41) | Flips sign across every road |

Check by hand: L × (−1, 1, 1, −1) = (−6, 6, 6, −6) = 6 × (−1, 1, 1, −1).

The hand guess a, scaled to length 1, has roughness 1; the Fiedler vector scores 0.84 by pulling cities 2 and 3 toward 0. Cutting the country road gives eigenvalues 0, 0, 6, 6: two zeros, two pieces.

## 9. What does this buy us, and where does it stop?

**One matrix turns questions about a network into linear algebra, but it rests on choices that real data can break.**

- Score any signal with xᵀLx; find clusters from the Fiedler vector (spectral clustering); decompose signals into network "frequencies" (graph Fourier analysis); model spreading with dx/dt = −Lx.
- Limits: weights must be symmetric and non-negative; high-degree nodes dominate (the normalised Laplacian D^(−1/2) L D^(−1/2) is common); an eigenvector's sign is arbitrary; the Fiedler split approximates the best cut; smooth is not the same as important.

## 10. The chain in one breath

**Network + signal → score the differences → the Laplacian falls out → its eigenvectors are the network's own patterns.**

1. Many datasets are a network W plus a signal x.
2. We want to know whether x changes little across strong connections.
3. Variance ignores the network; xᵀWx rewards large values. So we score differences: E(x) = ½ Σ w_ij (x_i − x_j)².
4. Expanding gives E(x) = xᵀ(D − W)x; L = D − W is the graph Laplacian.
5. The smoothest non-trivial patterns solve Lx = λx and reveal the network's clusters.

## Check your understanding

1. Why must every graph Laplacian have 0 as an eigenvalue?
2. If you double every weight, what happens to the eigenvalues and eigenvectors of L?
3. Why did we need the constraint that x sums to 0?
4. What would we lose by using |x_i − x_j| instead of (x_i − x_j)²?

**Answers.** (1) Each row sums to 0, so L**1** = 0. (2) Eigenvalues double; eigenvectors are unchanged. (3) Otherwise the constant pattern wins with E = 0 and says nothing. (4) The score is no longer a quadratic form, so the eigenvector solution disappears (that score, graph total variation, needs other tools).

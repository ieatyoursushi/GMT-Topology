# 11 — Optimal Transport: Wasserstein Distance and Sinkhorn (PSTAT 126)

### Inspired by Karen Grigorian PhD

**Cross-references.** `ml-geometry/NOTATION_ML.md` (typing §1.2, citation
key §3.1–3.2); `notes/06_measure_theory_bridge.md` (Radon measures, DCT —
reused in §2, §4); `notes/07_rectifiability_and_hausdorff_measure.md`
(Lipschitz maps, reused in §5 for the Kantorovich–Rubinstein dual form of
$W_1$); `01_prerequisite_systematic_review_for_GMT_Topology.md` §13.4
(Wasserstein distance already introduced there informally — made precise
here).

---

## 0. Standing Definitions

$$
\mu,\nu \quad \text{Borel probability measures on } \mathbb{R}^n, \qquad c:\mathbb{R}^n\times\mathbb{R}^n\to[0,\infty) \quad \text{a cost function (here } c(x,y)=|x-y|^p\text{)}.
$$

$$
\Pi(\mu,\nu) := \Big\{\pi\in\mathcal{M}(\mathbb{R}^n\times\mathbb{R}^n) : \pi\ge 0,\ \pi(A\times\mathbb{R}^n)=\mu(A),\ \pi(\mathbb{R}^n\times B)=\nu(B)\ \ \forall A,B \Big\} \quad \text{couplings of } \mu,\nu.
$$

$$
W_p(\mu,\nu) := \Big(\inf_{\pi\in\Pi(\mu,\nu)}\int_{\mathbb{R}^n\times\mathbb{R}^n} |x-y|^p\,d\pi(x,y)\Big)^{1/p}, \qquad p\ge 1.
$$

$$
a\in\Delta^{n-1},\ b\in\Delta^{m-1}, \qquad C\in\mathbb{R}^{n\times m}_{\ge0}, \qquad \Pi(a,b):=\Big\{P\in\mathbb{R}^{n\times m}_{\ge0} : P\mathbf{1}_m=a,\ P^\top\mathbf{1}_n=b\Big\} \quad \text{discrete couplings}.
$$

---

## 1. Monge vs. Kantorovich: Why Relax to Measures

**The Monge problem.** Find a map $T:\mathbb{R}^n\to\mathbb{R}^n$ with
$T_\#\mu=\nu$ (pushforward: $\nu(B)=\mu(T^{-1}(B))$) minimizing $\int
c(x,T(x))\,d\mu(x)$.

> **Type-check.** The Monge problem may have **no solution**: if $\mu=\delta_{x_0}$
> is a point mass and $\nu=\tfrac12\delta_{y_1}+\tfrac12\delta_{y_2}$ with
> $y_1\ne y_2$, no map $T$ can send the single point $x_0$ to two different
> points — $T_\#\delta_{x_0}$ is always a point mass. This is not a
> pathology to be dismissed; it is the structural reason the Monge
> formulation is too rigid for general measures.

**The Kantorovich relaxation** replaces a deterministic map $T$ with a
coupling $\pi\in\Pi(\mu,\nu)$ — a joint distribution with the right
marginals, allowing mass to *split*. This is a genuine relaxation: every
Monge map $T$ gives a coupling $\pi_T := (\mathrm{id},T)_\#\mu\in\Pi(\mu,\nu)$
with the same cost, but not every coupling arises this way, so
$\inf_\pi \le \inf_T$. Crucially, $\Pi(\mu,\nu)$ is always nonempty
(it contains $\mu\otimes\nu$) and — reusing `notes/06`'s measure-theoretic
machinery — is weak-$*$ compact when $\mu,\nu$ have compact support, which
is exactly the hypothesis needed to guarantee the infimum defining $W_p$ is
attained (a direct-method-of-calculus-of-variations argument, cited at
recognition depth; Villani, OT, Thm 1.7).

---

## 2. Wasserstein Distance as a Metric

**Proposition 2.1 (recognition depth).** $W_p$ is a genuine metric on the
space of probability measures with finite $p$-th moment (symmetry from
swapping the roles of $x,y$; triangle inequality via a gluing-of-couplings
argument; $W_p(\mu,\nu)=0\iff\mu=\nu$ from strict positivity of $|x-y|^p$
off the diagonal). *(Villani, OT, Ch. 6; not reproved here — the anchor
proof of this document is Kantorovich duality, §3, which is more
load-bearing for the ML applications below.)*

$W_2$ in particular metrizes weak convergence plus convergence of second
moments, and geodesics in $(\mathcal{P}_2(\mathbb{R}^n), W_2)$ between
absolutely continuous measures are given by McCann's displacement
interpolation — at recognition depth here (Villani, OT, Ch. 7), this is the
Wasserstein-space analogue of `notes/04` §3's geodesics, now on an
infinite-dimensional space of measures rather than a finite-dimensional
manifold.

---

## 3. Anchor Proof: Kantorovich Duality in the Finite/Discrete Case

**Theorem 3.1.** For $a\in\Delta^{n-1}$, $b\in\Delta^{m-1}$, $C\in\mathbb{R}^{n\times m}$,

$$
\min_{P\in\Pi(a,b)} \langle C,P\rangle = \max_{\substack{f\in\mathbb{R}^n,\,g\in\mathbb{R}^m \\ f_i+g_j\le C_{ij}\ \forall i,j}} \langle f,a\rangle + \langle g,b\rangle,
$$

where $\langle C,P\rangle=\sum_{i,j}C_{ij}P_{ij}$.

*Proof.* This is a linear program in $P$: minimize the linear functional
$\langle C,P\rangle$ subject to $P\ge 0$ and the linear equality constraints
$P\mathbf{1}_m=a$, $P^\top\mathbf{1}_n=b$. Write the Lagrangian, introducing
multipliers $f\in\mathbb{R}^n$ for the row constraints and $g\in\mathbb{R}^m$
for the column constraints:

$$
\mathcal{L}(P,f,g) = \langle C,P\rangle + \langle f, a-P\mathbf{1}_m\rangle + \langle g, b-P^\top\mathbf{1}_n\rangle, \qquad P\ge 0.
$$

Rearranging the terms involving $P$:

$$
\mathcal{L}(P,f,g) = \sum_{i,j}\big(C_{ij}-f_i-g_j\big)P_{ij} + \langle f,a\rangle+\langle g,b\rangle.
$$

The primal LP's value equals $\min_{P\ge0}\sup_{f,g}\mathcal{L}(P,f,g)$
(the equality constraints are enforced by the sup over unconstrained $f,g$
diverging to $+\infty$ unless they hold exactly). The dual is
$\sup_{f,g}\min_{P\ge0}\mathcal{L}(P,f,g)$. For fixed $f,g$,

$$
\min_{P\ge0}\ \sum_{i,j}(C_{ij}-f_i-g_j)P_{ij} = \begin{cases} 0 & \text{if } C_{ij}\ge f_i+g_j\ \ \forall i,j \\ -\infty & \text{otherwise} \end{cases}
$$

(since each $P_{ij}\ge0$ can be sent to $+\infty$ independently if its
coefficient $C_{ij}-f_i-g_j$ is negative, and $P_{ij}=0$ is optimal whenever
the coefficient is nonnegative). So the dual problem is exactly

$$
\max_{f_i+g_j\le C_{ij}\ \forall i,j} \langle f,a\rangle + \langle g,b\rangle.
$$

**Strong duality** (equality, not just $\ge$, between primal min and dual
max) holds because: the primal feasible region $\Pi(a,b)$ is nonempty
(contains $ab^\top$, the independent coupling) and compact (a closed,
bounded subset of $\mathbb{R}^{n\times m}$, being a polytope), so the primal
optimal value is finite; finite-dimensional linear programs with a
nonempty, bounded feasible region satisfy strong LP duality (a standard
consequence of Farkas' lemma / the LP duality theorem — cited, not
reproved, as the underlying convex-analysis fact this proof rests on).
$\blacksquare$

*(Standard result; this discrete/finite-dimensional derivation follows
Peyré–Cuturi §2.3, chosen deliberately over the continuous Kantorovich
duality theorem — which requires a Hahn–Banach/minimax argument on an
infinite-dimensional space of measures, or a weak-$*$ compactness argument
considerably heavier than finite LP duality — because the discrete case is
completely self-contained at this level and is exactly the object the
computational method of §4 solves.)*

> **Type-check.** The dual variables $f,g$ are exactly the **Kantorovich
> potentials**; the constraint $f_i+g_j\le C_{ij}$ is what makes them a
> *feasible pricing* of the transport problem (no arbitrage: shipping unit
> mass directly from $i$ to $j$ can never be cheaper via the "sell at $i$,
> buy at $j$" prices than the direct cost $C_{ij}$). This is the discrete
> shadow of the continuous Kantorovich duality's $c$-transform structure
> (Villani, OT, Thm 5.10), cited at recognition depth here.

---

## 4. Entropic Regularization and Sinkhorn's Algorithm

**Definition 4.1.** For $\varepsilon>0$, the entropic-OT problem is

$$
\min_{P\in\Pi(a,b)} \langle C,P\rangle - \varepsilon\,H(P), \qquad H(P):=-\sum_{i,j}P_{ij}(\log P_{ij}-1).
$$

**Proposition 4.2 (existence and uniqueness via strict convexity).** The
entropic-OT problem has a unique minimizer $P^\star_\varepsilon\in\Pi(a,b)$.

*Proof.* $\Pi(a,b)$ is convex (an intersection of a nonnegative orthant with
affine constraints) and compact (Theorem 3.1's proof). The map $P\mapsto
\langle C,P\rangle-\varepsilon H(P)$ is continuous on $\Pi(a,b)$ and **strictly
convex**: $\langle C,P\rangle$ is linear (convex, not strict), while $-H(P)
=\sum_{i,j}P_{ij}\log P_{ij}-P_{ij}$ is strictly convex in each coordinate
$P_{ij}$ (second derivative $1/P_{ij}>0$ on $P_{ij}>0$, extended continuously
to $0$), and a sum of strictly convex functions of independent coordinates
is strictly convex jointly. A strictly convex continuous function on a
nonempty compact convex set attains a unique minimum (standard: existence
by compactness/continuity — Weierstrass; uniqueness because if $P_1\ne P_2$
both minimized, the midpoint $\tfrac12(P_1+P_2)\in\Pi(a,b)$ by convexity of
the feasible set, and strict convexity would give it a strictly smaller
objective value, contradicting minimality of $P_1,P_2$). $\blacksquare$

**Proposition 4.3 (the Sinkhorn form, depth B).** The minimizer of
Proposition 4.2 has the form

$$
P^\star_\varepsilon = \operatorname{diag}(u)\,K\,\operatorname{diag}(v), \qquad K_{ij}:=e^{-C_{ij}/\varepsilon},
$$

for some $u\in\mathbb{R}^n_{>0}$, $v\in\mathbb{R}^m_{>0}$.

*Proof sketch.* Form the Lagrangian for the equality constraints as in
Theorem 3.1's proof; the stationarity condition
$\partial_{P_{ij}}\big[C_{ij}P_{ij}-\varepsilon P_{ij}(\log P_{ij}-1) - f_iP_{ij} - g_jP_{ij}\big]=0$
gives $C_{ij}-\varepsilon\log P_{ij}-f_i-g_j=0$, i.e. $P_{ij}=e^{(f_i+g_j-C_{ij})/\varepsilon}
= e^{f_i/\varepsilon}\,e^{-C_{ij}/\varepsilon}\,e^{g_j/\varepsilon}$ — exactly
the claimed form with $u_i:=e^{f_i/\varepsilon}$, $v_j:=e^{g_j/\varepsilon}$. $\blacksquare$

**Sinkhorn's algorithm** alternately rescales $u\leftarrow a/(Kv)$,
$v\leftarrow b/(K^\top u)$ (coordinatewise division) until the marginal
constraints are satisfied; this is exactly alternating Bregman projection
onto the two affine marginal constraints. Convergence (at geometric rate, in
the Hilbert projective metric on the positive orthant) is cited at
recognition depth (Cuturi 2013; Peyré–Cuturi §4.2, whose proof is a
contraction-mapping argument for Birkhoff's/Hilbert's projective metric —
genuinely more technical than the existence/uniqueness argument above, and
not reproduced here).

> **Why this matters computationally.** Theorem 3.1's exact LP has cost
> $O((nm)^3)$ in general via generic LP solvers; Sinkhorn's alternating
> scaling is $O(nm)$ per iteration and embarrassingly parallel/GPU-friendly
> — this is *the* reason entropic OT, not exact Kantorovich duality, is what
> gets used inside ML pipelines (Wasserstein GANs, Sinkhorn-based losses for
> generative models, distributional alignment).

---

## 5. Cross-Reference: Kantorovich–Rubinstein Duality and Lipschitz Maps

For $p=1$, the continuous Kantorovich dual (recognition depth here) takes
the specific form

$$
W_1(\mu,\nu) = \sup\Big\{\int f\,d\mu - \int f\,d\nu \ :\ f:\mathbb{R}^n\to\mathbb{R},\ \operatorname{Lip}(f)\le 1\Big\},
$$

a supremum over exactly the 1-Lipschitz functions of `notes/07` §1
Definition 1.1 — the same object this repo already built machinery for in
the GMT context (Rademacher's theorem, the area formula). This is the
precise sense in which $W_1$ is "dual to Lipschitz functions," and it is the
form used directly in the Wasserstein GAN construction (Arjovsky–Chintala–Bottou,
cited at recognition depth as a famous concrete ML use case, not separately
proved here since it is a direct instantiation of this duality with $f$
parametrized by a neural network constrained to be 1-Lipschitz).

---

## Cross-Reference Callout: The Capstone

The DirectIndexing_ML soft labels ($\tilde y_{BT}$, $\tilde y_{GBM}$, see
`16` §0) are Monte Carlo firing frequencies/probabilities, not optimal
transport constructions — **this document's tools are not what that memo
uses**, and the capstone (`16`) does not claim otherwise. Optimal transport
is flagged here as a *tool the project could apply* (e.g. comparing the
empirical feature-cloud distribution across rebalancing periods via $W_p$,
or regularizing a soft-label distribution toward a target via a
Sinkhorn-based loss) rather than something the existing memo's GMT
scaffolding already invokes — keeping the capstone's claim ledger honest
about what is and is not already present in that project.

---

## References Used

- **Villani, OT** — Ch. 1 (Monge/Kantorovich formulations, §1), Thm 1.7
  (existence of optimal couplings), Ch. 5 (continuous Kantorovich duality,
  cited §3), Ch. 6–7 ($W_p$ as a metric, geodesics/displacement
  interpolation, §2).
- **Peyré–Cuturi** — §2.3 (discrete Kantorovich LP duality, the anchor proof
  of §3 follows this source's derivation), §4 (entropic regularization,
  Sinkhorn, §4 above), §8 (Wasserstein GANs / ML applications, §5).
- **Cuturi 2013** — the original Sinkhorn-distances paper (§4).
- **Kantorovich 1942** — the original relaxation (§1).
- **Santambrogio 2015** — an alternative applied-mathematics treatment,
  cross-checked against Villani for §1–2.
- **`notes/06_measure_theory_bridge.md`** — Radon measures, weak-$*$
  compactness (§1, §2); **`notes/07_rectifiability_and_hausdorff_measure.md`**
  — Lipschitz maps, reused directly in §5.

Full bibliographic data: `ml-geometry/NOTATION_ML.md` §3.

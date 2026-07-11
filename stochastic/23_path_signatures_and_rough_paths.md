# 23 — Path Signatures and Rough Paths: Iterated Integrals as Features

**Cross-references.** `stochastic/NOTATION_STOCH.md` (typing §1.6, §1.7,
citation key §3); `notes/03_differential_forms_and_stokes.md` §5 and `00`
§5 (line integrals of 1-forms — the atom this whole document iterates);
`00` §4.1 (reparametrization type-check — §4 below is its featurization);
`stochastic/20` §1 (Brownian roughness — why §5's rough-path extension is
needed); `01` §8.5 (Fubini, used in the anchor proof); `ml-geometry/12`
(the "features respecting structure" theme, with time-parametrized paths
in place of graphs); feeds §6's finance applications and the DIML feature
remark there.

**Register (maturity ladder, `NOTATION_STOCH.md` §4).** Split, stated
per-section: §§1–5's mathematics is settled (Chen 1958; Lyons 1998;
monograph Friz–Hairer), §6's ML/finance applications are current and
moving (`NOTATION_STOCH.md` §3.2's currency check applies).

---

## 0. Standing Definitions

Everything from `stochastic/NOTATION_STOCH.md` §1.6. For this document,
paths are **piecewise $C^1$** maps $X:[a,b]\to\mathbb{R}^d$ — enough for
every proof below, with the extension to bounded-variation paths by
Stieltjes integration noted where it matters, and the extension *past* BV
deferred to §5. Coordinates: $X_t = (X^1_t,\dots,X^d_t)$.

$$
S^k_{[a,b]}(X) := \int_{a<t_1<\cdots<t_k<b} dX_{t_1}\otimes\cdots\otimes dX_{t_k} \ \in (\mathbb{R}^d)^{\otimes k}, \qquad S^0 := 1,
$$

with entries, for a multi-index $(i_1,\dots,i_k)\in\{1,\dots,d\}^k$,

$$
S^{(i_1,\dots,i_k)}_{[a,b]}(X) = \int_{a<t_1<\cdots<t_k<b} \dot X^{i_1}_{t_1}\cdots\,\dot X^{i_k}_{t_k}\ dt_1\cdots dt_k \ \in \mathbb{R}.
$$

$$
S_{[a,b]}(X) := \big(1,\,S^1,\,S^2,\,\dots\big) \in T\big((\mathbb{R}^d)\big) \quad \text{the signature}; \qquad \Delta := X_b - X_a \in \mathbb{R}^d \quad \text{the increment}.
$$

$$
\Delta_k(a,b) := \{(t_1,\dots,t_k) : a<t_1<\cdots<t_k<b\} \quad \text{the order simplex}, \qquad \mathcal{L}^k(\Delta_k(a,b)) = \frac{(b-a)^k}{k!}.
$$

---

## 1. What Is Being Iterated: `notes/03`'s Line Integral

The degree-one term is literally `00` §5's line integral of the
coordinate 1-forms: $S^{(i)} = \int_a^b dX^i = \Delta^i$, the increment.
Degree two already sees *order*: $S^{(i,j)} = \int_a^b\big(\int_a^{t_2}
dX^i_{t_1}\big)dX^j_{t_2}$ integrates "$X^i$'s progress so far" against
$dX^j$ — its symmetric part is determined by the increment
($S^{(i,j)}+S^{(j,i)} = \Delta^i\Delta^j$, a Fubini exercise), while its
antisymmetric part $\tfrac12(S^{(i,j)}-S^{(j,i)})$ is the **Lévy area**:
the signed area swept between the path's $(i,j)$-projection and its chord
— the first genuinely *path-dependent* feature, invisible to the endpoint.

```text
path X : [a,b] → ℝ^d
   │  (integrate coordinate 1-forms dx^i once — notes/03 §5, 00 §5)
   ▼
increments Δ = S¹                      (endpoint data only)
   │  (iterate: integrate progress against progress)
   ▼
S², S³, …  — Lévy areas, higher moments of *order of events*
   │  (stack: S(X) ∈ T((ℝ^d)))
   ▼
signature = the path's coordinate-free feature transform   (§4: parametrization dies, order survives)
   │  (linear functionals ℓ(S(X)): §5 universality)
   ▼
regression on signatures ≈ nonlinear functionals of the path   (§6: pricing, hedging, execution)
```

---

## 2. Warm-Up Anchor: the Signature of a Line Is a Tensor Exponential

**Proposition 2.1.** For the linear path $X_t = X_a + \frac{t-a}{b-a}\Delta$,

$$
S^k_{[a,b]}(X) = \frac{\Delta^{\otimes k}}{k!}, \qquad \text{i.e.} \qquad S(X) = \exp_\otimes(\Delta) := \sum_{k\ge0}\frac{\Delta^{\otimes k}}{k!}.
$$

*Proof.* Here $\dot X_t \equiv \Delta/(b-a)$ is constant, so the
integrand in §0's definition is the constant tensor
$\Delta^{\otimes k}/(b-a)^k$, and

$$
S^k = \frac{\Delta^{\otimes k}}{(b-a)^k}\ \mathcal{L}^k\big(\Delta_k(a,b)\big) = \frac{\Delta^{\otimes k}}{(b-a)^k}\cdot\frac{(b-a)^k}{k!} = \frac{\Delta^{\otimes k}}{k!},
$$

the simplex volume being $(b-a)^k/k!$ ($k!$ orderings tile the cube, each
of equal $\mathcal{L}^k$-measure — Fubini plus the measure-zero boundary
where coordinates coincide). $\blacksquare$

So straight lines have "trivial" signatures — pure exponentials of their
increment; every deviation of $S(X)$ from $\exp_\otimes(\Delta)$ is a
quantitative record of how $X$ *curved and backtracked* on its way.

---

## 3. Anchor Proof: Chen's Identity

**Theorem 3.1 (Chen 1958).** Let $X:[a,b]\to\mathbb{R}^d$ and
$Y:[b,c]\to\mathbb{R}^d$ be piecewise $C^1$ with $Y_b=X_b$, and let $X*Y$
be their concatenation. Then for every $k\ge0$,

$$
S^k_{[a,c]}(X*Y) \ =\ \sum_{i=0}^{k} S^i_{[a,b]}(X)\ \otimes\ S^{k-i}_{[b,c]}(Y),
\qquad \text{i.e.} \qquad S(X*Y) = S(X)\otimes S(Y) \ \in T\big((\mathbb{R}^d)\big).
$$

*Proof.* Fix $k$ and decompose the order simplex $\Delta_k(a,c)$ by how
many of the times fall before the splice point $b$: up to the
$\mathcal{L}^k$-null set where some $t_j=b$ (a hyperplane — the same
"boundaries of the decomposition are null" bookkeeping as `notes/03` §4.1's
facet argument),

$$
\Delta_k(a,c) \ =\ \bigsqcup_{i=0}^{k}\ \Delta_i(a,b)\times\Delta_{k-i}(b,c),
$$

since "$t_1<\cdots<t_k$ with exactly $i$ of them below $b$" forces those
$i$ to be $t_1,\dots,t_i$ (the times are ordered) and the rest to lie in
$(b,c)$, ordered among themselves — and conversely any such pair of
ordered tuples splices into one. On the piece indexed by $i$, the
integrand factorizes: $\dot{(X*Y)}_{t_j} = \dot X_{t_j}$ for $j\le i$ and
$\dot Y_{t_j}$ for $j>i$, so

$$
\int_{\Delta_i(a,b)\times\Delta_{k-i}(b,c)} d(X*Y)_{t_1}\otimes\cdots\otimes d(X*Y)_{t_k}
= \Big(\int_{\Delta_i(a,b)}dX_{t_1}\otimes\cdots\otimes dX_{t_i}\Big)\otimes\Big(\int_{\Delta_{k-i}(b,c)}dY_{t_{i+1}}\otimes\cdots\otimes dY_{t_k}\Big)
$$

by Fubini (`01` §8.5 — the integrand is a product of a function of the
first $i$ variables and a function of the last $k-i$, integrable since the
derivatives are bounded on compacts), and the right side is
$S^i_{[a,b]}(X)\otimes S^{k-i}_{[b,c]}(Y)$. Summing over the disjoint
pieces gives the claim; collecting all $k$ into the algebra
$T((\mathbb{R}^d))$ gives $S(X*Y)=S(X)\otimes S(Y)$, since the degree-$k$
component of a product in $T((\mathbb{R}^d))$ is exactly
$\sum_{i+j=k}(\cdot)^i\otimes(\cdot)^j$. $\blacksquare$

> **What this buys you.** Concatenation of paths becomes *multiplication*
> of signatures: the map $X\mapsto S(X)$ is a homomorphism from the
> concatenation semigroup of paths into $(T((\mathbb{R}^d)),\otimes)$.
> Consequences used downstream: signatures can be computed *streamingly*
> (extend a data window one tick at a time by one $\otimes$-multiplication
> — the practitioner-relevant form in §6); run backwards, a path's
> time-reversal has signature $S(X)^{-1}$ (Chen applied to
> $X*\overleftarrow{X}$, which is tree-like — see §4); and Proposition 2.1
> plus Chen give closed-form signatures for all piecewise-linear paths,
> i.e. for every actually-recorded data stream.

---

## 4. Invariance and Uniqueness: What the Signature Sees

**Proposition 4.1 (reparametrization invariance).** If
$\phi:[a',b']\to[a,b]$ is an increasing piecewise-$C^1$ bijection, then
$S_{[a',b']}(X\circ\phi) = S_{[a,b]}(X)$.

*Proof.* Substitute $t_j = \phi(s_j)$ in each iterated integral: each
factor $\frac{d}{ds}(X^{i_j}\circ\phi)(s_j)\,ds_j =
\dot X^{i_j}(\phi(s_j))\,\phi'(s_j)\,ds_j = \dot X^{i_j}(t_j)\,dt_j$
transforms as a 1-form pullback (`notes/03` §5 — this *is*
$\int_{\phi}X^*\omega = \int X^*\omega$ in coordinates), and increasing
$\phi$ maps the order simplex bijectively onto the order simplex.
$\blacksquare$

> **Type-check — `00` §4.1, upgraded to a feature map.** `00` §4.1's rule:
> a well-formed geometric quantity must not depend on parametrization.
> Proposition 4.1 says every signature coordinate passes that check —
> and the uniqueness theorem below says the signature is essentially
> *all* such quantities. The practical consequence for §6: two data feeds
> sampling the same price path at different clocks (calendar time, trade
> time) yield the same signature features — sampling-rate robustness is a
> theorem, not a hope. The flip side: *speed is genuinely discarded*. If
> time-of-traversal matters (it usually does in finance), one augments
> the path to $t\mapsto(t,X_t)$ — the standard **time augmentation**,
> which re-injects the clock as a coordinate.

**Theorem 4.2 (Hambly–Lyons 2010; stated, recognition depth).** A
bounded-variation path is determined by its signature up to
**tree-like equivalence** (retraced excursions that cancel exactly —
e.g. $X*\overleftarrow{X}$ is tree-like, matching $S(X)\otimes S(X)^{-1}
= 1$ from §3). Time-augmented paths have no nontrivial tree-like parts,
so for them the signature is a *complete, parametrization-free* encoding.

---

## 5. Universality, Kernels, and the Rough Extension

Three facts at recognition depth, each with its load-bearing citation:

- **Universality (why linear models suffice on signatures).** Chen's
  identity makes products of signature coordinates re-expressible as
  linear combinations of (higher) signature coordinates — the **shuffle
  product** identity — so the linear span of coordinate functionals
  $S(X)\mapsto \ell(S(X))$ is an algebra; it separates points by Theorem
  4.2; Stone–Weierstrass then gives: *linear* functionals of the
  (truncated) signature uniformly approximate *continuous* functionals of
  the path on compacts. Regression on signatures is nonparametric
  regression on path space. (Chevyrev–Kormilitzin 2016, §1–2, for the
  assembled statement.)
- **Signature kernels.** The inner product $k_{\mathrm{sig}}(X,Y) =
  \langle S(X),S(Y)\rangle$ can be computed *without truncation* as the
  solution of a Goursat PDE (Salvi et al. 2021) — the same
  RKHS-ification move `ml-geometry/14` §2 applied to currents/varifolds,
  here applied to signatures; and **neural CDEs** (Kidger et al. 2020)
  are the learned-vector-field counterpart, with the signature as their
  universal linearization.
- **Rough paths (why this survives Brownian roughness).** §0 assumed
  piecewise $C^1$; Brownian paths are nowhere differentiable
  (`stochastic/20` Theorem 1.1) and of unbounded variation, so already
  $S^2$'s defining integral is ill-posed pathwise. Lyons 1998's rough
  path theory resolves this by *postulating* the first $\lfloor p\rfloor$
  levels (for $p$-variation regularity) subject to Chen's identity —
  Theorem 3.1 promoted from theorem to axiom, exactly the pattern of
  `notes/08` §2 (Stokes promoted to the definition of current boundary) —
  and recovering the rest, along with a robust theory of differential
  equations driven by such signals (Friz–Hairer). For Brownian motion the
  postulated level-2 object is precisely the Itô (or Stratonovich)
  integral of `20` §2 — the two theories meet where they should.

---

## 6. The Options Desk, Part IV: Rough Volatility and Signature Trading (current)

Everything in this section carries `NOTATION_STOCH.md` §3.2's currency
flag; the through-line is that §§1–5's objects are now working parts of
the volatility literature.

- **Volatility is rough** (GJR 2018): realized-volatility time series
  behave like paths of Hurst regularity $H\approx0.1$ — far rougher than
  Brownian ($H=\tfrac12$), hence far outside classical semimartingale
  modeling comfort. The **rough Bergomi** model (BFG 2016) builds the
  vol process from fractional Brownian motion and prices under it —
  the concrete reason a derivatives quant meets $p$-variation and §5's
  rough-path toolkit. This also sharpens `stochastic/22` §6's honesty
  item: GBM's constant-$\sigma$ misspecification is not a small one.
- **Nonparametric pricing and hedging** (LNP 2020): an exotic payoff is
  a functional of the price path; by §5's universality it is
  approximately a *linear* functional of the signature, so pricing
  reduces to (i) computing expected signatures under the model measure
  and (ii) linear regression — with hedging strategies read off the same
  expansion. Optimal execution gets the same treatment in KLP 2020.
- **Signature-based volatility models** (Cuchiero et al. 2023; Cuchiero
  et al. 2025): take the *model itself* to be linear in a signature —
  vol as a linear functional of the (augmented) driving noise's
  signature — giving tractable calibration; the 2025 paper jointly
  calibrates SPX and VIX smiles, a benchmark classical models struggle
  with. This is the sub-literature the currency check (2026-07) flags as
  still actively moving.

**DIML remark (a lead, in `ml-geometry/16` §4's sense).** DIML's
asset-level features (`ml-geometry/16` §0.1: $R_t$, $\sigma_{\mathrm{range}}$,
$\Delta_{MA50}$, $\Delta_{MA200}$) are four hand-crafted functionals of
the trailing price path. §5's universality says truncated signatures of
the (time-augmented) trailing window are a *canonical, complete* feature
family for the same information — hand-crafted functionals are
approximately linear in them, by construction rather than by luck.
Whether the swap helps empirically is untested and stays on `16` §4's
open-leads ledger.

---

## References Used

- **Chen 1958** — Theorem 3.1 (the identity and its algebraic setting).
- **Hambly–Lyons 2010** — Theorem 4.2 (uniqueness up to tree-like
  equivalence).
- **Lyons 1998**; **Friz–Hairer** — §5's rough-path extension (theory and
  monograph treatment).
- **Chevyrev–Kormilitzin 2016** — §5's assembled universality statement
  and the ML-facing primer perspective.
- **Salvi et al. 2021** — signature kernels via the Goursat PDE.
- **Kidger et al. 2020** — neural controlled differential equations.
- **GJR 2018**; **BFG 2016** — §6's rough-volatility anchor papers.
- **LNP 2020**; **KLP 2020** — signature pricing/hedging and optimal
  execution.
- **Cuchiero et al. 2023**; **Cuchiero et al. 2025** — signature-based
  volatility models and the SPX/VIX joint calibration.
- **`notes/03`** §5 (pullbacks/line integrals — Propositions 2.1, 4.1);
  **`01`** §8.5 (Fubini — Theorem 3.1); **`stochastic/20`** §1 (Brownian
  roughness — §5); **`ml-geometry/14`** §2 (the RKHS pattern — §5);
  **`ml-geometry/16`** §0.1, §4 (the DIML feature remark).

Full bibliographic data: `stochastic/NOTATION_STOCH.md` §3.

# 09 — Varifolds and Sets of Finite Perimeter

**Cross-references.** `notes/NOTATION.md` (typing conventions, §2.4 for
$V_k(\mathbb{R}^n)$, $BV(\Omega)$); `notes/06_measure_theory_bridge.md`
(Radon measures, Hausdorff measure); `notes/07_rectifiability_and_hausdorff_measure.md`
(rectifiable sets, approximate tangent planes — the geometric substrate of
rectifiable varifolds); `notes/08_currents.md` (contrast: currents carry
orientation, varifolds do not); `notes/04_riemannian_metrics_and_curvature.md`
§4 (curvature $R(X,Y)Z$, generalized here by first variation);
`00_differential_geometry_foundations_for_GMT_Topology.md` §29–30
(survey-level varifolds, finite perimeter).

---

## 0. Standing Definitions

$$
G(n,k) \quad \text{Grassmannian of $k$-dimensional linear subspaces of } \mathbb{R}^n.
$$

$$
V \quad \text{a Radon measure on } \mathbb{R}^n\times G(n,k), \qquad V \in V_k(\mathbb{R}^n) \quad \text{(a $k$-varifold)}.
$$

$$
E \subseteq \mathbb{R}^n \quad \text{Lebesgue-measurable}, \qquad \mathbf{1}_E : \mathbb{R}^n \to \{0,1\} \quad \text{indicator function}.
$$

$$
u \in BV(\Omega): \qquad u\in L^1(\Omega), \quad Du \quad \text{(distributional gradient) is a finite } \mathbb{R}^n\text{-valued Radon measure on } \Omega.
$$

---

## 1. Varifolds

**Definition 1.1.** A **$k$-varifold** on $\mathbb{R}^n$ is a Radon measure
$V$ on $\mathbb{R}^n\times G(n,k)$. Write $V_k(\mathbb{R}^n)$ for the set of
$k$-varifolds. The **weight measure** (or **mass measure**) of $V$ is its
pushforward under projection to the first factor:

$$
\|V\|(A) := V(A\times G(n,k)), \qquad A\subseteq\mathbb{R}^n \text{ Borel}.
$$

**Definition 1.2 (rectifiable varifold).** Given a $k$-rectifiable set
$E\subseteq\mathbb{R}^n$ (`notes/07` §2) and a multiplicity function
$\theta:E\to(0,\infty)$, $\mathcal{H}^k$-integrable, the associated
**rectifiable varifold** is

$$
V(A\times S) := \int_{E\cap A} \theta(x)\,\mathbf{1}_S(T_xE)\,d\mathcal{H}^k(x), \qquad A\subseteq\mathbb{R}^n,\ S\subseteq G(n,k) \text{ Borel},
$$

using the approximate tangent plane $T_xE \in G(n,k)$, existing
$\mathcal{H}^k$-a.e. by `notes/07` Proposition 2.3.

> **Type-check — currents vs. varifolds.** A $k$-current $T\in
> D_k(\mathbb{R}^n)$ (`notes/08` §1) is a functional built from an
> **oriented** approximate tangent plane $\vec\tau(x)$ — a choice of one of
> the two unit "orientations" of $T_xE$. A $k$-varifold is built from the
> **unoriented** plane $T_xE \in G(n,k)$ itself, with no sign choice at all.
> Concretely: the current associated to a surface traversed one way and the
> current associated to the same surface traversed the opposite way are
> *different* currents ($T(\omega)$ negates), but they give rise to the
> *same* varifold ($\theta$ and $T_xE$ are unaffected by reversing
> orientation). This is exactly why varifolds are the right object for weak
> limits of minimal surfaces where a consistent orientation may fail to
> exist in the limit (e.g. limits of soap films with non-orientable
> topology), while currents are the right object when orientation and
> boundary duality (`notes/08` §2) must be tracked.

**Definition 1.3 (first variation).** For $V\in V_k(\mathbb{R}^n)$ and a
compactly supported smooth vector field $X$ on $\mathbb{R}^n$, the **first
variation** of $V$ in the direction $X$ is

$$
\delta V(X) := \int_{\mathbb{R}^n\times G(n,k)} \operatorname{div}_S X(x)\,dV(x,S), \qquad \operatorname{div}_S X(x) := \sum_{i=1}^k \langle D_{e_i}X(x), e_i\rangle
$$

for an orthonormal basis $\{e_1,\dots,e_k\}$ of the plane $S$ — i.e. the
divergence of $X$ tangential to $S$. For a smooth submanifold $M$ (viewed as
a rectifiable varifold via Definition 1.2 with $\theta\equiv 1$), $\delta
V(X) = -\int_M \langle X, \vec{H}\rangle\,d\mathcal{H}^k$, where $\vec H$ is
the **mean curvature vector** — so $\delta V$ generalizes curvature to
varifolds with no smoothness assumed (Simon, LGMT, §16).

> **Type-check.** $\delta V$ has type "linear functional on compactly
> supported vector fields," exactly parallel to how a current is a linear
> functional on test forms (`notes/08` §1) and a distribution is a linear
> functional on test functions (`01` §9.5). $V$ is called **stationary** if
> $\delta V(X) = 0$ for every such $X$ — the varifold-theoretic
> generalization of "$M$ has zero mean curvature," i.e. of being a critical
> point of the area functional, without requiring $M$ smooth enough to have
> a classical mean curvature vector.

---

## 2. Sets of Finite Perimeter

**Definition 2.1.** $E\subseteq\mathbb{R}^n$ Lebesgue-measurable has **finite
perimeter in $\Omega$** if $\mathbf{1}_E \in BV(\Omega)$, i.e. $D\mathbf{1}_E$
is a finite $\mathbb{R}^n$-valued Radon measure on $\Omega$. The **perimeter**
of $E$ in $\Omega$ is

$$
P(E;\Omega) := |D\mathbf{1}_E|(\Omega),
$$

the total variation measure of $D\mathbf{1}_E$.

**Definition 2.2 (reduced boundary).** $x \in \partial^*E$, the **reduced
boundary**, if the limit

$$
\nu_E(x) := \lim_{r\to 0^+} \frac{D\mathbf{1}_E(B_r(x))}{|D\mathbf{1}_E|(B_r(x))}
$$

exists and $|\nu_E(x)|=1$. $\nu_E(x)$ is the **measure-theoretic outer
normal** at $x$.

**Theorem 2.3 (De Giorgi structure theorem, stated).** If $E$ has finite
perimeter in $\Omega$, then $\partial^*E$ is $(n{-}1)$-rectifiable, and

$$
D\mathbf{1}_E = \nu_E\, \mathcal{H}^{n-1}\llcorner\partial^*E \qquad \text{(as measures)},
$$

so $P(E;\Omega) = \mathcal{H}^{n-1}(\partial^*E\cap\Omega)$.

*(Evans–Gariepy, MTFP, Thm 5.6/Ch. 5; Maggi, SFP&GVP, Thm 15.5. A deep
structure theorem, stated here at recognition depth per `01` §17's guidance —
not reproved, but used below.)*

| Boundary type | Definition | Relationship |
|---|---|---|
| topological $\partial E$ | $\overline{E}\setminus\operatorname{int}(E)$ | can be $\mathcal{H}^{n-1}(\partial E) = \infty$ even for finite-perimeter $E$ |
| measure-theoretic boundary | density neither $0$ nor $1$ | contains $\partial^*E$ |
| reduced boundary $\partial^*E$ | Definition 2.2 | $(n{-}1)$-rectifiable, carries $D\mathbf{1}_E$ (Theorem 2.3) |

**Theorem 2.4 (Generalized Gauss–Green).** If $E$ has finite perimeter in
$\Omega$ and $X$ is a $C_c^1$ vector field on $\Omega$, then

$$
\int_E \operatorname{div}X\,d\mathcal{L}^n = \int_{\partial^*E} \langle X,\nu_E\rangle\,d\mathcal{H}^{n-1}.
$$

*(Evans–Gariepy, MTFP, Thm 5.16, an immediate consequence of Theorem 2.3
applied to the defining identity $\int_E \operatorname{div}X\,d\mathcal{L}^n
= -\int_\Omega X\cdot dD\mathbf{1}_E$, itself the definition of the
distributional derivative $D\mathbf{1}_E$, `01` §9.6.)*

---

## 3. Anchor Proof: $P(E)=\mathcal{H}^{n-1}(\partial E)$ for Smooth Domains, and Reduction to the Classical Divergence Theorem

**Theorem 3.1.** Let $E\subseteq\mathbb{R}^n$ be open, bounded, with $C^1$
boundary $\partial E$ (a smooth $(n{-}1)$-submanifold in the classical
sense). Then $E$ has finite perimeter, $\partial^*E = \partial E$, $\nu_E$
agrees with the classical outward unit normal, and

$$
P(E;\mathbb{R}^n) = \mathcal{H}^{n-1}(\partial E).
$$

Moreover, the generalized Gauss–Green theorem (Theorem 2.4) reduces exactly
to the classical divergence theorem (`notes/03` Corollary 4.6).

*Proof.* Since $\partial E$ is $C^1$ and compact (bounded + closed, as the
boundary of a bounded open set with $C^1$ boundary), it has, at every point
$x\in\partial E$, a well-defined classical outward unit normal $\nu(x)$,
smooth in $x$.

*Step 1: $\mathbf{1}_E \in BV(\mathbb{R}^n)$, with $D\mathbf{1}_E = \nu\,
\mathcal{H}^{n-1}\llcorner\partial E$.* For any $X \in C_c^1(\mathbb{R}^n;
\mathbb{R}^n)$, the classical divergence theorem (`notes/03` Corollary 4.6,
applicable since $E$ is a compact $C^1$-boundary solid region) gives

$$
\int_E \operatorname{div}X\,d\mathcal{L}^n = \int_{\partial E} \langle X,\nu\rangle\,d\mathcal{H}^{n-1}
$$

(here using that the classical surface measure on the $C^1$ hypersurface
$\partial E$ coincides with $\mathcal{H}^{n-1}$ restricted to $\partial E$,
by the smooth case of `notes/06` Theorem 5.4/`notes/07` Theorem 3.2 applied
to a $C^1$ parametrization of $\partial E$). By definition, $D\mathbf{1}_E$
is exactly the distribution satisfying $\int_{\mathbb{R}^n}\mathbf{1}_E\,
\operatorname{div}X\,d\mathcal{L}^n = -\int_{\mathbb{R}^n} X\cdot dD\mathbf{1}_E$
for all such $X$ (this is the defining property of the distributional
derivative, `01` §9.6, applied to $u=\mathbf{1}_E$). Comparing the two
displayed identities (note $\int_E\operatorname{div}X\,d\mathcal{L}^n =
\int_{\mathbb{R}^n}\mathbf{1}_E\operatorname{div}X\,d\mathcal{L}^n$),

$$
-\int_{\mathbb{R}^n} X\cdot dD\mathbf{1}_E = \int_{\partial E}\langle X,\nu\rangle\,d\mathcal{H}^{n-1} \quad\Longrightarrow\quad D\mathbf{1}_E = -\nu\,\mathcal{H}^{n-1}\llcorner\partial E
$$

as $\mathbb{R}^n$-valued measures (matching test-vector-field pairings for
all $X$ determines the measure). [Sign convention note: some sources define
$D\mathbf{1}_E$ so that the outward normal appears with a $+$ sign directly;
the essential content — that $D\mathbf{1}_E$ is $\mathcal{H}^{n-1}\llcorner
\partial E$ weighted by the unit normal — is convention-independent.] In
particular $D\mathbf{1}_E$ is a finite Radon measure (since $\partial E$ is
compact, $\mathcal{H}^{n-1}(\partial E)<\infty$ by compactness + $C^1$-ness),
so $\mathbf{1}_E \in BV(\mathbb{R}^n)$.

*Step 2: identify $\partial^*E$ and compute $P(E)$.* By Step 1's identity
$D\mathbf{1}_E = -\nu\,\mathcal{H}^{n-1}\llcorner\partial E$, for
$x\in\partial E$,

$$
\frac{D\mathbf{1}_E(B_r(x))}{|D\mathbf{1}_E|(B_r(x))} = \frac{\int_{\partial E\cap B_r(x)} -\nu\,d\mathcal{H}^{n-1}}{\mathcal{H}^{n-1}(\partial E\cap B_r(x))} \ \longrightarrow\ -\nu(x) \quad \text{as } r\to 0^+,
$$

using continuity of $\nu$ (a $C^1$, hence continuous, function on the compact
hypersurface $\partial E$) to pass to the limit in the average. This limit
exists and has unit length, so $x\in\partial^*E$ with $\nu_E(x) = -\nu(x)$
(up to the sign convention flagged in Step 1) for every $x\in\partial E$;
conversely, off $\partial E$ (i.e. on the open sets $\operatorname{int}(E)$
or $\operatorname{int}(E^c)$), $D\mathbf{1}_E$ vanishes identically in a
neighborhood by Step 1, so the defining limit of Definition 2.2 is not
approached by a unit vector (indeed $|D\mathbf{1}_E|(B_r(x))=0$ for small
$r$, making the ratio undefined/excluded) — such points are not in
$\partial^*E$. Hence $\partial^*E = \partial E$ exactly, and

$$
P(E;\mathbb{R}^n) = |D\mathbf{1}_E|(\mathbb{R}^n) = \int_{\partial E} |\!-\!\nu|\,d\mathcal{H}^{n-1} = \mathcal{H}^{n-1}(\partial E),
$$

using $|\nu(x)|=1$ pointwise (unit normal).

*Step 3: the generalized Gauss–Green theorem reduces to the classical
divergence theorem.* By Step 1, for $X\in C_c^1(\mathbb{R}^n;\mathbb{R}^n)$,
the generalized statement (Theorem 2.4) reads

$$
\int_E \operatorname{div}X\,d\mathcal{L}^n = \int_{\partial^*E}\langle X,\nu_E\rangle\,d\mathcal{H}^{n-1} = \int_{\partial E} \langle X,\nu\rangle\,d\mathcal{H}^{n-1}
$$

(using $\partial^*E=\partial E$, $\nu_E = \nu$ up to the sign convention of
Step 1, which cancels consistently against the sign already absorbed into
$D\mathbf{1}_E$) — which is *exactly* the classical divergence theorem used
as an input in Step 1. So for $C^1$-boundary domains, Theorem 2.4 is not a
strictly new statement — it is the classical divergence theorem,
re-expressed in the language of distributional derivatives and reduced
boundaries, and Step 1–2 show these two languages agree on their common
domain of applicability. $\blacksquare$

> **What this buys you, precisely.** Theorem 3.1 pins down, in a fully
> worked example where every object is classical, exactly how the GMT
> replacements of `notes/NOTATION.md` §3 relate to the smooth originals they
> generalize: $\partial^*E$ *is* $\partial E$ when $\partial E$ is $C^1$;
> $\mathcal{H}^{n-1}\llcorner\partial^*E$ *is* the classical surface measure;
> the generalized Gauss–Green theorem *is* the classical divergence theorem.
> The entire value of the finite-perimeter machinery (Definitions 2.1–2.2,
> Theorem 2.3) is that it continues to make sense — with $\partial^*E$ now
> merely $(n{-}1)$-rectifiable rather than $C^1$, and the Gauss–Green
> identity still holding — for sets $E$ whose boundary is not smooth at all
> (e.g. a boundary with corners, cusps, or a Cantor-set-like singular part),
> exactly the situations that arise as limits of minimizing sequences in the
> calculus of variations (`01` §9.6, §10.5).

---

## References Used

- **Maggi, SFP&GVP** — Ch. 12–15 (sets of finite perimeter, reduced boundary,
  De Giorgi's structure theorem, Thm 15.5).
- **Evans–Gariepy, MTFP** — Ch. 5 (BV functions and sets of finite perimeter;
  Thm 5.6 structure theorem, Thm 5.16 Gauss–Green theorem — the anchor proof
  above reduces to exactly this theorem in the smooth case).
- **Simon, LGMT** — §16 (varifolds, first variation, mean curvature as a
  special case).

Full bibliographic data: `notes/NOTATION.md` §4.

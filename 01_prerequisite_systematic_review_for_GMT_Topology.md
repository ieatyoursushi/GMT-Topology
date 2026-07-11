# 01 — Prerequisite Systematic Review for Differential Geometry / GMT-Topology

**Purpose.** This is not exam prep. It is a *map-first, proof-anchored* review
of the upper-division prerequisites that matter for studying differential
geometry and then moving toward geometric measure theory (GMT): manifolds
analyzed through measure, rectifiability, weak convergence, and generalized
surface objects, rather than assuming global smoothness everywhere.

**Repository role.** This is the second **survey/index** document. It answers,
per prerequisite subject: what you actually need, what the key findings are,
what can stay black-boxed, what to prove once for ownership, and how the
subject feeds `00` and `notes/02`–`09`. It does not itself carry full proofs of
GMT-level theorems — those live in the `notes/` deep dives, cited below at each
proof anchor.

**Cross-references.** `notes/NOTATION.md` (typing conventions, depth scale
§1.1, citation key — read first); `00_differential_geometry_foundations_for_GMT_Topology.md`
(the arc this review feeds); `notes/02`–`09` (where each GMT-facing proof
anchor is actually carried out in full).

---

## 0. Standing Definitions and the Depth Scale

This document uses the global typing context of `notes/NOTATION.md` §2
throughout. The one convention specific to this document is the three-level
**depth scale** (`notes/NOTATION.md` §1.1), repeated here since the whole
document is organized around it:

| Depth | You can... |
|---|---|
| **A — Recognition** | identify the object/technique when it appears |
| **B — Operational** | use the theorem correctly in a computation or proof |
| **C — Ownership** | reproduce the proof from definitions |

For this repository: most prerequisites only need A/B depth; each subject
below should have at least one C-depth **proof anchor**; GMT-facing topics
eventually need B/C depth.

## 1. The Practical Verdict

$$
\textbf{Map first} \ + \ \textbf{prove anchor theorems} \ + \ \textbf{open black boxes only when they become central.}
$$

The dangerous version is reading theorem *names* and mistaking recognition for
understanding. Working rule for this repository:

> For each prerequisite field, know the main objects (with their types), the
> big theorems (with precise hypotheses), why they matter geometrically, and
> prove at least one serious theorem from scratch so the subject stops feeling
> like a tourist zone.

---

## 2. Dependency Map

A typed dependency graph — each arrow is annotated with the *kind of object*
the upstream subject hands to the downstream one:

```text
Linear Algebra
    │  supplies: vector spaces V, duals V*, Λᵏ(V*)
    ↓
Multivariable Analysis / Vector Calculus
    │  supplies: DF_p : ℝⁿ→ℝᵐ, Jacobians, parametrizations
    ↓
Real Analysis + Point-Set Topology
    │  supplies: compactness, convergence, charts, manifolds
    ↓
Smooth Manifolds + Differential Forms
    │  supplies: T_pM, Ω^k(M), d, Stokes
    ↓
Riemannian Geometry / De Rham Theory
    │  supplies: g, ∇, R(X,Y)Z, H^k_dR(M)
    ↓
Measure Theory + Functional Analysis
    │  supplies: (Ω,𝓕,μ), L^p, weak convergence, distributions
    ↓
GMT: 𝓗^k, rectifiability, currents, varifolds, BV, finite perimeter
```

Parallel statistical/probabilistic line:

```text
Linear Algebra
    ↓  supplies: inner products, projections
Probability as Measure Theory
    ↓  supplies: (Ω,𝓕,ℙ), random variables X:Ω→ℝ
L^p / Hilbert Spaces / Projection
    ↓  supplies: L²(Ω,𝓕,ℙ), orthogonal projection
Empirical Measures, Wasserstein Distances, KL, Fisher Information
    ↓  supplies: μ̂_n = (1/n)Σδ_{X_i}, W_1(μ,ν), I(θ)
Information Geometry / Geometric ML / Probabilistic GMT-flavored intuition
```

Optional complex-geometry line:

```text
Real Analysis + Topology
    ↓
Complex Analysis
    ↓  supplies: holomorphic f:U⊆ℂ→ℂ, conformal maps
Riemann Surfaces / Complex Manifolds
    ↓
Kähler Geometry / Complex Analytic Varieties
```

Highest-priority fields for GMT/DG, in order: (1) linear algebra, (2)
multivariable analysis, (3) real analysis, (4) point-set topology, (5) measure
theory, (6) differential geometry foundations, (7) functional analysis
(Hilbert spaces, weak convergence, Sobolev/BV intuition), (8) PDE/variational
methods, eventually, (9) complex analysis, for complex manifolds/conformal
geometry.

---

## 3. Subject Priority Table (typed)

| Subject | Core objects (typed) | Priority | Depth | Main payload |
|---|---|---:|---:|---|
| Linear algebra | $V$, $V^*$, $\Lambda^kV^*$, $Df_p$ | Essential | Operational + anchor | tangent spaces, forms, metrics, curvature, PCA/Fisher geometry |
| Multivariable analysis | $DF_p:\mathbb{R}^n\to\mathbb{R}^m$, $X:\Omega\to\mathbb{R}^n$ | Essential | Operational | parametrization, Jacobians, Stokes/Gauss, local derivatives |
| Real analysis | $(x_n)\subseteq\mathbb{R}$, compact $K$ | Essential | Map + anchor | limits, compactness, convergence, rigorous control |
| Point-set topology | $(X,\tau)$, charts $(U,\varphi)$ | Essential | Map + operational | manifolds, charts, compactness, quotient/gluing |
| Measure theory | $(\Omega,\mathcal{F},\mu)$, $\mathcal{H}^k$ | Essential | Operational + eventual deep | Lebesgue integration, a.e., Radon/Hausdorff measure, convergence |
| Functional analysis | Hilbert space $H$, $H^*$ | Very useful | Map + selected operational | Hilbert/Banach spaces, weak convergence, duality, distributions |
| Differential equations/PDE | $\dot\gamma = X(\gamma)$ | Useful | Map first | flows, geodesics, Laplacian, variational equations |
| Complex analysis | $f:U\subseteq\mathbb{C}\to\mathbb{C}$ holomorphic | Optional/specialized | Map first | conformal geometry, Riemann surfaces, complex manifolds |
| Algebraic topology/de Rham | $H^k_{\mathrm{dR}}(M)$ | Very useful | Map + examples | cycles, boundaries, cohomology, forms detect topology |
| Probability/statistical learning | $(\Omega,\mathcal{F},\mathbb{P})$, $X:\Omega\to\mathbb{R}$ | Personally high-value | Operational | measures, projections, Wasserstein, KL/Fisher, empirical geometry |

---

## 4. Linear Algebra

### Core objects

Vector spaces $V$; subspaces; bases and $\dim V$; linear maps $T:V\to W$;
matrices as coordinate representations of $T$; dual spaces
$V^* = \operatorname{Hom}(V,\mathbb{R})$; inner products $\langle\cdot,\cdot\rangle$;
orthogonal projections; determinants $\det:V^n\to\mathbb{R}$; eigenvalues and
eigenvectors; symmetric/self-adjoint operators; quadratic forms; singular
value decomposition; exterior powers $\Lambda^k(V^*)$.

### Key findings

**4.1 Coordinates are not the object.** A vector is not "a column of numbers"
intrinsically — it becomes a column only after choosing a basis. A tangent
vector $v \in T_pM$ is geometric; its coordinate column changes under a chart
change. This matters because differential geometry is largely the art of
saying things without depending on a coordinate choice.

**4.2 Linear maps are the local models of smooth maps.** The derivative of a
smooth map is a linear map, $df_p : T_pM \to T_{f(p)}N$; in Euclidean
coordinates this becomes the Jacobian matrix.

**4.3 Dual vectors measure vectors.** A covector $\omega \in V^*$ is a linear
map $\omega : V \to \mathbb{R}$. Differential forms are built from
alternating multilinear covectors — this is why $dx,dy,dz$ should be
understood as covectors, not mysterious infinitesimals.

**4.4 The determinant is oriented volume.** For $v_1,\dots,v_n \in
\mathbb{R}^n$, $\det(v_1,\dots,v_n) \in \mathbb{R}$ is signed $n$-dimensional
volume — the prototype of a top-degree differential form.

**4.5 Exterior algebra encodes oriented $k$-dimensional measurement.** A
$k$-form eats $k$ tangent vectors, $\omega_p(v_1,\dots,v_k) \in \mathbb{R}$.
$dx\wedge dy$ measures oriented area in the $xy$-directions; $dx\wedge
dy\wedge dz$ measures oriented volume.

**4.6 Inner products identify vectors with covectors — only after choosing a
metric.** Intrinsically $df_p \in T_p^*M$ is a covector; $\nabla f$ is a
vector field only after choosing a Riemannian metric $g$, via $g(\nabla f,X)
= df(X)$ (`00` §17.1, `notes/04` §1.1). This distinction is central in
differential geometry.

**4.7 Symmetric operators diagonalize geometry.** The spectral theorem: a
real symmetric matrix is diagonalizable in an orthonormal basis. Geometrically,
quadratic forms are ellipsoids with principal axes — this shows up in PCA,
Hessians, second fundamental forms, curvature operators, Fisher information
matrices, and local ellipsoid approximations of loss/likelihood surfaces.

### Why it matters for differential geometry

Linear algebra is the local language of geometry:

| Linear algebra object | Differential geometry object |
|---|---|
| vector space $V$ | tangent space $T_pM$ |
| dual space $V^*$ | cotangent space $T_p^*M$ |
| linear map | derivative/pushforward $dF_p$ |
| dual map | pullback $F^*$ |
| determinant | volume form |
| inner product | Riemannian metric $g$ |
| symmetric bilinear form | metric, Hessian, second fundamental form |
| exterior power $\Lambda^k(V^*)$ | differential $k$-forms |
| eigenvalues | principal curvatures, PCA axes, stability modes |

### Why it matters for GMT

GMT keeps a lot of linear algebra but weakens smoothness. Approximate tangent
planes ($T_pE$, `00` §27) are still linear objects — but exist only
$\mathcal{H}^k$-a.e. rather than everywhere. Rectifiable sets are approximately
covered by countably many Lipschitz images of Euclidean pieces; their
"tangent spaces" are measurable approximate tangent planes rather than a
smooth tangent bundle (`notes/07`).

### Proof anchor

**Theorem (Finite-dimensional Hilbert/projection theorem).** Let $V$ be a
finite-dimensional real inner product space and $W \subseteq V$ a subspace.
For every $v \in V$ there is a unique $w^* \in W$ minimizing $\lVert v -
w\rVert$ over $w \in W$; it satisfies $v - w^* \perp W$, and the map $v
\mapsto w^*$ is a linear, symmetric, idempotent map $P_W : V \to V$ with
$\operatorname{im}(P_W)=W$.

*(Standard; see e.g. Rudin, RCA, or any linear algebra text with an inner
product chapter.)* Best choice for this repo: prove this, then show
$H = X(X^TX)^{-1}X^T$ (§12.1 below) is exactly $P_{\operatorname{Col}(X)}$.

---

## 5. Multivariable Analysis / Vector Calculus

### Core objects

Partial derivatives; total derivative $Df_p$; Jacobian matrix; gradient,
divergence, curl; line/surface/flux integrals; change of variables; Green's,
Stokes', divergence theorems; parametrized curves and surfaces
$X:\Omega\to\mathbb{R}^n$.

### Key findings

**5.1 The derivative is the best linear approximation.** For $f:\mathbb{R}^n
\to\mathbb{R}^m$, $f(a+h) = f(a) + Df_a(h) + o(\lVert h\rVert)$. The real
object is the linear map $Df_a$; the Jacobian is its coordinate
representation.

**5.2 A parametrization is a coordinate choice, not the surface itself.** A
surface patch $r(u,v) = (x(u,v),y(u,v),z(u,v))$ is a map, not the surface
(its image). The same geometric surface can have multiple parametrizations —
this distinction becomes "charts vs. manifold" in differential geometry.

**5.3 Jacobians measure local distortion.** For $F:\mathbb{R}^n\to\mathbb{R}^n$,
$dV_{\mathrm{new}} = |\det DF|\,dV$; for a surface parametrization $r(u,v)$,
$dS = \lVert r_u\times r_v\rVert\,du\,dv$. This becomes the area formula in
GMT (`notes/07` §3) once $r$ is only assumed Lipschitz.

**5.4 Vector calculus theorems are baby generalized Stokes.** FTC $\Rightarrow$
Green $\Rightarrow$ Stokes $\Rightarrow$ Divergence are all shadows of
$\int_M d\omega = \int_{\partial M}\omega$ (`00` §16).

**5.5 Boundary information can encode derivative information.**
Green/Stokes/Gauss do *not* say the boundary contains all pointwise
information about the interior — they say one particular accumulated
derivative quantity over the interior equals one particular accumulated
quantity over the boundary.

### Why it matters for differential geometry

Tangent vectors as velocities of curves; normals from parametrizations;
pullback-like substitution; orientation; boundary-integral duality; flux and
circulation; local-to-global integration.

### Why it matters for GMT

The smooth theorem $\int_{\partial M}\omega = \int_M d\omega$ eventually
becomes a weak/distributional/measure-theoretic statement for currents and
finite-perimeter sets (`notes/08`, `notes/09`).

### Proof anchor

**Theorem (Green's theorem on a rectangle).** Let $R = [a,b]\times[c,d]$ and
$P,Q \in C^1(R)$. Then

$$
\int_{\partial R} P\,dx + Q\,dy = \iint_R (Q_x - P_y)\,dA,
$$

with $\partial R$ traversed counterclockwise.

*(Standard; e.g. Spivak, CoM, Ch. 4–5.)* Best choice for this repo: prove it
directly by reducing both sides to iterated 1-D integrals via FTC, then
explicitly identify $M = R$, $\partial M = \partial R$, $\omega = P\,dx+Q\,dy$
in the language of `00` §16.1.

---

## 6. Real Analysis

### Core objects

Real numbers and completeness; sequences; Cauchy sequences; limits;
continuity; uniform continuity; compactness; connectedness in $\mathbb{R}$;
differentiability; Riemann integration; uniform convergence; function
sequences.

### Key findings

**6.1 Completeness is what makes limits exist.** $\mathbb{Q}$ has Cauchy
sequences that fail to converge in $\mathbb{Q}$; $\mathbb{R}$ fixes this.
Analysis is largely about controlling limiting processes, and GMT is full of
them: limits of surfaces, limits of measures, weak limits of currents,
compactness theorems, lower-semicontinuity arguments.

**6.2 Compactness converts local control into global control.** On compact
sets, continuous functions attain extrema and are uniformly continuous;
sequences often admit convergent subsequences. In geometry this is the
skeleton of many existence proofs: *a minimizing sequence has a convergent
subsequence, and the limit is still admissible.*

**6.3 Uniform convergence controls interchange of limits.** Pointwise
convergence is weak; uniform convergence lets you preserve continuity and
interchange operations — important for convergence of charts/functions,
Fourier series, approximation arguments, and later, weak-vs-strong
convergence.

**6.4 Epsilon-delta arguments train quantitative control.** Even without exam
fluency as a goal, tracking $\forall\varepsilon>0,\exists\delta>0,\dots$
dependence is exactly the skill needed to later read "for almost every," "for
sufficiently small radius," "up to a null set," "uniformly on compact
subsets," "after passing to a subsequence."

### Why it matters for differential geometry

Justifies smoothness, inverse/implicit function theorems, local coordinates,
compactness arguments, convergence of functions, differentiability under
coordinate changes.

### Why it matters for GMT

GMT is analysis-heavy; real analysis is the base layer under measure theory
and functional analysis. Without it, measure-theoretic convergence theorems
feel like magic rather than upgraded limit machinery.

### Proof anchor

**Theorem (Heine–Borel, in $\mathbb{R}$).** $K \subseteq \mathbb{R}$ is
compact (every open cover has a finite subcover) if and only if $K$ is closed
and bounded.

*(Standard; e.g. Rudin, RCA, Thm 2.41, or any real analysis text — the
Bolzano–Weierstrass route is the usual proof.)* Best choice for this repo:
prove it in $\mathbb{R}$ via nested intervals/Bolzano–Weierstrass, then treat
it as the prototype compactness theorem referenced throughout §6.2.

---

## 7. Point-Set Topology

### Core objects

Topological spaces $(X,\tau)$; open/closed sets; bases; subspace, product,
quotient topology; continuous maps; homeomorphisms; compactness;
connectedness; Hausdorff spaces; second countability; local compactness;
manifolds.

### Key findings

**7.1 Topology studies structure before measurement.** A topology specifies
which sets are open; from that alone you get continuity, convergence,
compactness, connectedness, local/global structure — geometry without
lengths or angles.

**7.2 A manifold is locally Euclidean but globally glued.** A topological
$n$-manifold is $M$ such that $\forall p\in M,\ \exists U\ni p,\ \varphi : U
\to \varphi(U)\subseteq\mathbb{R}^n$ a homeomorphism. The chart $\varphi$ is a
coordinate system; $M$ is the object being coordinatized.

**7.3 Hausdorff and second-countable conditions prevent pathology.**
Manifolds are usually required Hausdorff (distinct points separated by
neighborhoods) and second-countable (countable topology base) — these keep
manifolds close enough to Euclidean space to support analysis.

**7.4 Quotients are powerful but dangerous.** $[0,1]/(0\sim 1) \cong S^1$, but
quotient spaces can behave badly if the equivalence relation is pathological
— relevant later for moduli spaces and singular spaces.

**7.5 Topology is what survives deformation.** A sphere and a cube are
topologically the same kind of surface; a sphere and a torus are not.
Differential geometry adds smoothness, metrics, curvature, and integration on
top of topology.

### Why it matters for differential geometry

You cannot define manifolds without topology; smoothness is defined by
compatibility of charts on overlaps, not pointwise from scratch (`00` §7).

### Why it matters for GMT

GMT often studies subsets that are not manifolds. Topology clarifies what is
lost when smoothness fails: singularities may appear, tangent planes may
exist only a.e., boundaries may be measure-theoretic rather than topological,
connectedness may not control geometry.

### Proof anchor

**Theorem.** The continuous image of a compact space is compact: if
$f : X \to Y$ is continuous and $K \subseteq X$ is compact, then $f(K)
\subseteq Y$ is compact.

*(Munkres, Topology, Thm 26.5.)* Best choice for this repo: prove it directly
from the open-cover definition of compactness, then use it as the reusable
global argument it is (e.g. immediately gives the extreme value theorem).

---

## 8. Measure Theory and Integration

### Core objects

$\sigma$-algebras; measurable spaces $(\Omega,\mathcal{F})$; measures $\mu$;
probability measures; outer measures; Lebesgue measure $\mathcal{L}^n$; Borel
sets; measurable functions; Lebesgue integral; null sets; a.e. equivalence;
monotone convergence, Fatou, dominated convergence; product measures;
Fubini/Tonelli; $L^p$ spaces; Radon measures; Hausdorff measure $\mathcal{H}^k$.

### Key findings

**8.1 Measurability is a wider frame than smoothness.** Smooth objects are
highly structured; measurable objects need only enough structure to define
size/integration. Key bridge to GMT:

$$
\{\text{smooth geometry}\} \subset \{\text{measurable geometry}\}.
$$

**8.2 Sets matter as much as functions.** In calculus, functions dominate; in
measure theory, $\mu(A)$, the size of a measurable set $A$, is fundamental.
Functions are integrated by measuring level sets and approximating by simple
functions.

**8.3 Almost everywhere is a controlled weakening of everywhere.** "$f=g$
a.e." fails only on a $\mu$-null set — a systematic equivalence relation, not
sloppiness. In $L^p$, functions are equivalence classes under $f\sim g \iff
f=g$ a.e.

**8.4 Convergence theorems are the engine.** Monotone convergence: $0\le
f_n\uparrow f \implies \int f_n \to \int f$. Dominated convergence: $f_n\to f$
pointwise, $|f_n|\le g \in L^1 \implies \int f_n \to \int f$. These are
theorems about when limits commute with integrals.

**8.5 Product measures justify high-dimensional integration.**
Fubini/Tonelli specify when a product integral can be computed iteratively —
the measure-theoretic version of repeated integrals.

**8.6 Hausdorff measure generalizes dimension-specific area.** $\mathcal{H}^k$
measures $k$-dimensional size inside $\mathbb{R}^n$: $\mathcal{H}^1$ is curve
length, $\mathcal{H}^2$ surface area, $\mathcal{H}^n$ volume up to
normalization. Foundational for GMT; constructed in full in `notes/06` §5.

**8.7 Radon measures are geometry-compatible measures.** Finite on compact
sets, regular, compatible with integration against continuous test
functions — the natural measure objects in weak convergence and GMT.

### Why it matters for differential geometry

Measure theory generalizes integration on manifolds; even on smooth
manifolds, the Riemannian volume form induces a measure — a smooth special
case of a general measure.

### Why it matters for GMT

Core prerequisite: GMT studies geometric objects through $\mathcal{H}^k$,
Radon measures, density, weak convergence, rectifiability, integration over
non-smooth sets, finite perimeter, BV functions.

### Proof anchor

**Theorem (Dominated Convergence Theorem).** Let $(f_n)$ be measurable
functions on $(\Omega,\mathcal{F},\mu)$ with $f_n \to f$ $\mu$-a.e., and
suppose there is $g \in L^1(\mu)$ with $|f_n|\le g$ $\mu$-a.e. for all $n$.
Then $f \in L^1(\mu)$ and $\int_\Omega f_n\,d\mu \to \int_\Omega f\,d\mu$.

*(Evans–Gariepy, MTFP, Thm 1.19; or Rudin, RCA, Thm 1.34.)* Full proof (via
Fatou's lemma applied to $g\pm f_n \ge 0$) worked out, alongside the
construction of $\mathcal{H}^k$, in `notes/06` §4, §5.

---

## 9. Functional Analysis

### Core objects

Normed vector spaces; Banach spaces; Hilbert spaces; bounded linear
operators; dual spaces; weak convergence; compact operators; Hahn–Banach;
uniform boundedness; open mapping; closed graph; Riesz representation;
spectral theorem; Sobolev spaces; distributions/weak derivatives.

### Key findings

**9.1 Functional analysis is infinite-dimensional linear algebra plus
topology.** In finite dimensions all norms are equivalent and closed bounded
sets are compact; both fail in infinite dimensions, which is why functional
analysis is not merely "harder linear algebra."

**9.2 Banach spaces make convergence complete.** A Banach space is a
complete normed vector space, so Cauchy sequences converge inside the space —
crucial for existence theorems.

**9.3 Hilbert spaces preserve the geometry of projections.** A Hilbert space
is a complete inner product space. If $C$ is closed and convex, every point
has a unique closest point in $C$ (the projection theorem, §4 above lifted to
infinite dimensions). OLS is a finite-dimensional shadow of this; conditional
expectation is an $L^2$-projection.

**9.4 Weak convergence is weaker but more compactness-friendly.** Strong:
$\lVert x_n - x\rVert \to 0$. Weak: $\ell(x_n)\to\ell(x)$ for every continuous
linear functional $\ell$. Weak convergence loses pointwise strength but gains
compactness behavior; GMT uses it heavily for measures, currents, and
variational limits.

**9.5 Dual spaces are test-function interfaces.** A distribution acts on test
functions rather than being a function in the classical sense; currents act
on test forms the same way. The conceptual move object $\leadsto$ linear
functional on test objects is one of the most important bridges into GMT
(`notes/08`).

**9.6 Sobolev/BV spaces encode weak differentiability.** A weak derivative
$D^\alpha f$ satisfies $\int f\,D^\alpha\varphi = (-1)^{|\alpha|}\int D^\alpha
f\,\varphi$ for test functions $\varphi$, defined by integration by parts
rather than pointwise limits. BV functions allow distributional derivatives
that are measures — central for finite-perimeter sets (`notes/09`).

### Why it matters for differential geometry

PDE on manifolds; Hodge theory; Sobolev spaces on manifolds; variational
calculus; spectral geometry; weak solutions.

### Why it matters for GMT

Weak convergence of measures; compactness theorems; lower semicontinuity;
distributions; currents as functionals on forms; BV and Sobolev spaces;
variational methods.

### Proof anchor

**Theorem (Hilbert projection theorem).** Let $H$ be a Hilbert space and
$C \subseteq H$ nonempty, closed, and convex. For every $x \in H$, there is a
unique $y^* \in C$ with $\lVert x - y^*\rVert = \inf_{y\in C}\lVert x-y\rVert$,
characterized by $\operatorname{Re}\langle x-y^*, y-y^*\rangle \le 0$ for all
$y \in C$.

*(Standard; e.g. Rudin, RCA, Thm 4.10, restricted to real Hilbert spaces.)*
Best choice for this repo: prove it via the parallelogram law argument (a
minimizing sequence for $\lVert x-y\rVert$ over $y\in C$ is Cauchy by
parallelogram law + convexity, hence converges by completeness), then
connect the $C = $ subspace case to OLS and to conditional expectation as
$L^2$-projection.

---

## 10. Differential Equations / PDE / Fourier Analysis

### Core objects

ODEs; vector fields $X:M\to TM$; flows; existence/uniqueness; stability;
linear systems; eigenvalues; heat, wave, Laplace, Poisson equations; Fourier
series/transform; Green's functions; weak solutions; variational PDE.

### Key findings

**10.1 Vector fields generate flows.** $\dot x = V(x)$ says a vector field
$V$ generates motion; on a manifold, $\dot\gamma(t) = X_{\gamma(t)}$ for a
vector field $X$ (`00` §9).

**10.2 Geodesics are differential equations.** $\nabla_{\dot\gamma}\dot\gamma
= 0$, i.e. $\ddot x^k + \Gamma^k_{ij}\dot x^i\dot x^j = 0$ (`00` §19).

**10.3 Fourier analysis diagonalizes translation-invariant operators.** The
heat equation $u_t = \Delta u$ is solved by decomposing $u$ into eigenfunctions
of $\Delta$ — the same linear-algebra idea as diagonalizing a matrix, now in a
function space.

**10.4 PDEs often encode geometry.** Minimal surface equation, mean curvature
flow, harmonic map equation, Ricci flow, Laplace–Beltrami eigenvalue
problems. GMT often appears exactly where classical smooth solutions fail to
exist globally or develop singularities.

**10.5 Weak solutions are the bridge to non-smooth worlds.** Classical
solution: $u$ differentiable enough pointwise. Weak solution: $u$ satisfies
integrated identities against test functions — directly analogous to
currents/varifolds encoding an object through how it test-pairs, rather than
requiring a smooth surface.

### Why it matters for differential geometry

Geodesics, curvature evolution, harmonic functions, heat flow, spectral
geometry.

### Why it matters for GMT

Area-minimizing surfaces, finite-perimeter sets, minimal currents, and
stationary varifolds all lead naturally to weak Euler–Lagrange equations.

### Proof anchor

**Theorem (Picard–Lindelöf / Banach fixed-point existence).** Let $V :
\mathbb{R}^n \to \mathbb{R}^n$ be Lipschitz with constant $L$. For any
$x_0\in\mathbb{R}^n$, there exists $\varepsilon>0$ and a unique $C^1$ curve
$\gamma : (-\varepsilon,\varepsilon)\to\mathbb{R}^n$ with $\gamma(0)=x_0$ and
$\dot\gamma(t) = V(\gamma(t))$.

*(Standard; via the Banach fixed-point theorem applied to the integral
operator $\Phi(\gamma)(t) = x_0 + \int_0^t V(\gamma(s))\,ds$ on a complete
metric space of curves.)* Best choice for this repo: prove via Banach
fixed-point, then note this is exactly the existence theorem underlying flows
of vector fields and, later, the exponential map on a Riemannian manifold.

---

## 11. Complex Analysis / Complex Geometry

### Core objects

Complex differentiability; holomorphic functions; Cauchy–Riemann equations;
Cauchy integral theorem/formula; residues; meromorphic functions; conformal
maps; Riemann surfaces; complex manifolds; holomorphic charts; complex
differential forms.

### Key findings

**11.1 Complex differentiability is extremely rigid.** A real derivative is
local linear approximation over $\mathbb{R}$; a complex derivative is local
linear approximation over $\mathbb{C}$. This extra structure forces:
holomorphic $\implies$ analytic; values on small sets can determine global
behavior; contour integrals detect singularities; conformal maps preserve
angles where the derivative is nonzero.

**11.2 Conformal maps preserve angles.** A holomorphic map with nonzero
derivative locally looks like $z\mapsto az$, $a\in\mathbb{C}\setminus\{0\}$ —
rotation plus scaling — hence angle-preserving.

**11.3 Riemann surfaces are one-complex-dimensional manifolds.** A real
2-manifold with holomorphic transition maps — the clean prototype for complex
manifolds.

**11.4 Complex manifolds are smoother than smooth manifolds.** A complex
$n$-manifold is a real $2n$-manifold with holomorphic coordinate changes,
much more rigid than $C^\infty$-smoothness.

**11.5 Complex geometry is optional unless complex structures matter.** Not a
first-line prerequisite unless studying complex manifolds, calibrated
geometry, Kähler geometry, complex analytic varieties, minimal surfaces via
complex methods, or 2D conformal structures.

### Why it matters for differential geometry

Simplest examples of complex manifolds, conformal geometry, holomorphic maps,
residues as topological/geometric invariants, local-to-global rigidity.

### Why it matters for GMT

Complex analytic varieties can be singular geometric objects, interacting
with GMT-like questions — but for a general GMT route, measure theory and
functional analysis matter more first.

### Proof anchor

**Theorem (Cauchy Integral Formula).** Let $f$ be holomorphic on an open set
containing a closed disk $\overline{D}(z_0,r)$. For $z$ in the open disk,

$$
f(z) = \frac{1}{2\pi i}\oint_{|\zeta - z_0|=r} \frac{f(\zeta)}{\zeta - z}\,d\zeta.
$$

*(Standard; e.g. any complex analysis text, via Cauchy's theorem for
triangles + a deformation argument.)* Best choice for this repo: prove it,
then derive that holomorphic functions are automatically analytic
(differentiating under the integral sign).

---

## 12. Algebraic Topology / De Rham Theory

### Core objects

Homotopy; fundamental group; simplices; chains; cycles; boundaries; homology;
cohomology; differential forms; closed/exact forms; de Rham cohomology;
Poincaré duality.

### Key findings

**12.1 Boundaries have no boundary.** $\partial^2 = 0$: the boundary of a
boundary is $0$. Parallel identity for forms: $d^2=0$ (`00` §12, proved in
`notes/03` §2).

**12.2 Homology measures holes using cycles modulo boundaries.** A cycle has
no boundary; a boundary bounds a higher-dimensional object; homology asks for
$\{\text{cycles}\}/\{\text{boundaries}\}$ — a nonzero class detects a hole.

**12.3 De Rham cohomology measures holes with differential forms.** $\omega$
closed if $d\omega=0$; exact if $\omega=d\eta$. Since $d^2=0$, exact
$\implies$ closed, but not conversely; $H^k_{\mathrm{dR}}(M) =
\ker(d_k)/\operatorname{im}(d_{k-1})$ detects topology (`00` §22, proved in
`notes/05`).

**12.4 Stokes explains why exact forms vanish on cycles.** If $C$ is a cycle
and $\omega=d\eta$, then $\int_C\omega = \int_C d\eta = \int_{\partial C}\eta
= 0$ by Stokes — integration of closed forms over cycles carries topological
information.

**12.5 Currents are chain-like objects with analysis attached.** A current
can be thought of as a generalized oriented surface acting on differential
forms — where algebraic topology and GMT start talking to each other
(`notes/08`).

### Why it matters for differential geometry

De Rham theory is the topology-detecting layer of differential forms: it
shows calculus on manifolds can see global holes.

### Why it matters for GMT

Currents generalize oriented manifolds/chains while retaining boundary
operator, mass, weak convergence, and integration against forms.

### Proof anchor

**Theorem.** $d^2=0$ on $\Omega^\bullet(M)$, and consequently
$H^1_{\mathrm{dR}}(S^1) \cong \mathbb{R}$.

*(Tu, Introduction to Manifolds, §17 ($d^2=0$), §26–29 ($H^1_{\mathrm{dR}}(S^1)$);
Bott–Tu, Ch. 1.)* Full proofs, including the Poincaré lemma these results
depend on: `notes/05` (anchor proofs).

---

## 13. Probability / Statistics / ML Bridge

This section is included because my probability/statistical-learning
background is not separate from the geometry project — it is one of the
strongest personal bridges into measure-theoretic geometry.

### Core objects

Random variables as measurable functions $X:\Omega\to\mathbb{R}$; probability
distributions as measures; empirical measures; CDFs and quantile functions;
Wasserstein distance; $L^2(\Omega,\mathcal{F},\mathbb{P})$; orthogonal
projection; OLS geometry; hat matrix and residual maker; PCA/SVD; KL
divergence; Fisher information; MLE; Newton–Raphson/IRLS; neural networks as
parametrized function classes.

### Key findings

**13.1 OLS is projection geometry.** $\hat\beta = (X^TX)^{-1}X^Ty$,
$\hat y = Hy$, $H = X(X^TX)^{-1}X^T \in \mathbb{R}^{n\times n}$. $H$ is
symmetric and idempotent ($H^T=H$, $H^2=H$), i.e. it is the orthogonal
projection onto $\operatorname{Col}(X)$ (this is precisely the §4 proof
anchor, instantiated). $M = I-H$ projects onto the orthogonal complement —
finite-dimensional Hilbert geometry.

**13.2 Regression already contains functional analysis.** "OLS is best
linear unbiased" is projection theory plus inner-product geometry; in
infinite dimensions, regression becomes best approximation in a Hilbert
space, and conditional expectation is an $L^2$-projection (§9.3).

**13.3 Empirical distributions are measures.** Given $X_1,\dots,X_n$, the
empirical distribution $\hat\mu_n = \frac1n\sum_{i=1}^n \delta_{X_i}$ is a
probability measure built from point masses — measure theory, not just
statistics.

**13.4 Wasserstein distance is geometry on probability measures.** In one
dimension, $W_1(\mu,\nu) = \int_0^1 |F_\mu^{-1}(u) - F_\nu^{-1}(u)|\,du$ —
treats probability distributions as points in a metric space.

**13.5 PCA is spectral geometry.** PCA solves $\max_{\lVert w\rVert=1}
w^T\Sigma w$; the solution is the top eigenvector of $\Sigma$ — the spectral
theorem applied to covariance geometry, i.e. ellipsoid geometry, not just a
data trick.

**13.6 Ridge and lasso are geometry of constraints.** Ridge:
$\min_\beta \lVert y-X\beta\rVert^2 + \lambda\lVert\beta\rVert_2^2$ (round/
elliptic geometry). Lasso: $\min_\beta \lVert y-X\beta\rVert^2 +
\lambda\lVert\beta\rVert_1$ (polyhedral geometry, encouraging sparsity) —
optimization geometry.

**13.7 Fisher information is local statistical geometry.** $I(\theta) =
-\mathbb{E}_\theta[\nabla_\theta^2\ell(\theta;Y)]$ acts as a local metric on
parameter space; $D_{\mathrm{KL}}(P_{\theta_0}\|P_{\theta_0+h}) =
\frac12 h^TI(\theta_0)h + o(\lVert h\rVert^2)$. Locally, statistical models
carry a Riemannian-like geometry.

**13.8 Newton/IRLS is curvature-aware optimization.** Newton's method uses
the Hessian; for logistic regression it becomes IRLS,
$\beta^{N+1} = (X^TWX)^{-1}X^TWz$ — weighted projection geometry that changes
each iteration.

**13.9 $L^2$ random variables form a Hilbert space.** $(\xi,\eta) =
\mathbb{E}[\xi\eta]$, $\lVert\xi\rVert_2 = (\mathbb{E}[\xi^2])^{1/2}$ makes
$L^2(\Omega,\mathcal{F},\mathbb{P})$ a Hilbert space; $\xi\perp\eta \iff
\mathbb{E}[\xi\eta]=0$, which for centered variables means uncorrelatedness.

### Why it matters for differential geometry

Metric spaces of distributions, statistical manifolds, Fisher geometry,
projections, Hilbert spaces, variational optimization.

### Why it matters for GMT

Probability measures and Radon measures are central in modern analysis.
Wasserstein geometry, empirical measures, weak convergence, and
metric-measure spaces are conceptually adjacent to GMT. GMT is not
"statistics," but a statistics background gives real measure-theoretic
intuition ahead of time.

### Proof anchor

**Theorem.** $H = X(X^TX)^{-1}X^T$ is the orthogonal projection onto
$\operatorname{Col}(X)$ (equivalently, OLS is best approximation onto
$\operatorname{Col}(X)$ in the Euclidean inner product).

*(Direct corollary of the §4 projection theorem, specialized to $V =
\mathbb{R}^n$, $W = \operatorname{Col}(X)$.)* Best choice for this repo:
prove $H^T=H$, $H^2=H$ directly, verify $y - Hy \perp \operatorname{Col}(X)$,
then prove PCA's first component solves the Rayleigh-quotient problem
$\max_{\lVert w\rVert=1}w^T\Sigma w$ via the spectral theorem (§4.7) as a
second worked example.

---

## 14. Differential Geometry Prerequisite Payload

Compressed payload these prerequisites feed into `00` and `notes/02`–`04`;
each line below is a typed one-liner with a pointer to where it is treated
in full.

| Object | Type | Full treatment |
|---|---|---|
| Smooth $n$-manifold | locally modeled on $\mathbb{R}^n$, smoothly compatible charts | `00` §7, `notes/02` §1–2 |
| Tangent space | $T_pM$, an $n$-dim real vector space | `00` §8, `notes/02` §3 |
| Cotangent space | $T_p^*M = \operatorname{Hom}(T_pM,\mathbb{R})$ | `00` §10, `notes/02` §4 |
| Differential $k$-form | $p\mapsto\omega_p\in\Lambda^k(T_p^*M)$ | `00` §11, `notes/03` §1 |
| Exterior derivative | $d:\Omega^k(M)\to\Omega^{k+1}(M)$, $d^2=0$ | `00` §12, `notes/03` §2 |
| Generalized Stokes | $\int_Md\omega=\int_{\partial M}\omega$ | `00` §16, `notes/03` §3–4 |
| Riemannian metric | $g_p:T_pM\times T_pM\to\mathbb{R}$ | `00` §17, `notes/04` §1 |
| Covariant derivative | $\nabla_XY\in\Gamma(TM)$ | `00` §18, `notes/04` §2 |
| Curvature | $R(X,Y)Z\in\Gamma(TM)$ | `00` §20, `notes/04` §3 |

---

## 15. GMT Bridge: Smooth Geometry After Smoothness Breaks

### Smooth model

A smooth $k$-submanifold $M\subseteq\mathbb{R}^n$ has: $T_pM$ at every point,
smooth charts, a smooth volume/area element, a classical boundary,
integration of forms, and Stokes' theorem.

### GMT replacement

A GMT object may have: tangent planes only $\mathcal{H}^k$-a.e.; density
instead of pointwise regularity; weak convergence instead of smooth
convergence; measure-theoretic boundary instead of topological boundary;
rectifiability instead of smoothness; first variation instead of classical
curvature; current/varifold structures instead of embedded manifolds.

### Core GMT objects (typed; full construction in `notes/06`–`09`)

$$
\mathcal{H}^k : 2^{\mathbb{R}^n} \to [0,\infty] \qquad \text{(`notes/06` §5)}
$$

$$
E \subseteq \mathbb{R}^n \text{ countably } k\text{-rectifiable} \iff \mathcal{H}^k\Big(E\setminus\bigcup_j f_j(A_j)\Big)=0, \quad f_j:A_j\subseteq\mathbb{R}^k\to\mathbb{R}^n \text{ Lipschitz} \qquad \text{(`notes/07`)}
$$

$$
T_pE \text{ exists for } \mathcal{H}^k\text{-a.e. } p\in E \qquad \text{(`notes/07` §2)}
$$

$$
T: \Omega_c^k(\mathbb{R}^n)\to\mathbb{R}, \qquad \partial T(\omega) := T(d\omega) \qquad \text{(`notes/08`)}
$$

$$
V \in V_k(\mathbb{R}^n), \qquad V \text{ a Radon measure on } \mathbb{R}^n\times G(n,k) \qquad \text{(`notes/09`)}
$$

$$
u \in BV(\Omega) \iff Du \text{ is a finite Radon measure}, \qquad P(E) := |D\mathbf{1}_E|(\mathbb{R}^n) \qquad \text{(`notes/09` §2)}
$$

### Smooth-to-GMT dictionary

Full typed version: `notes/NOTATION.md` §3. Compressed:

| Smooth differential geometry | GMT replacement |
|---|---|
| smooth submanifold | rectifiable set |
| tangent plane everywhere | approximate tangent plane a.e. |
| smooth area form | Hausdorff measure/mass measure |
| smooth boundary | measure-theoretic boundary |
| oriented manifold | current |
| unoriented surface | varifold |
| smooth convergence | weak convergence |
| classical derivative | distributional derivative |
| smooth Stokes | current boundary identity |
| smooth perimeter | BV/finite perimeter |
| mean curvature vector | first variation |

### The main philosophical upgrade

Smooth differential geometry asks: *what can be said when the object is
differentiable enough?* GMT asks: *what can still be said when
differentiability fails but measure-theoretic structure remains?* Measure
theory is not "extra" here — it is the new foundation.

---

## 16. Proof Anchor Menu

One theorem per row; pick it up when the subject becomes relevant. Precise
statements for each are given inline in the sections above.

| Field | Proof anchor | Why this one |
|---|---|---|
| Linear algebra | Finite-dim Hilbert projection theorem | Explains OLS, PCA, metric geometry |
| Multivariable analysis | Green's theorem on a rectangle | Baby Stokes |
| Real analysis | Heine–Borel | Compactness prototype |
| Topology | Continuous image of compact is compact | Reusable global argument |
| Measure theory | Dominated convergence theorem | Core Lebesgue advantage |
| Functional analysis | Hilbert projection theorem (infinite-dim) | Infinite-dimensional OLS/geometry |
| PDE/Fourier | Picard–Lindelöf existence | Underlies flows and the exponential map |
| Complex analysis | Cauchy integral formula | Holomorphic rigidity |
| Algebraic topology | $d^2=0$, then $H^1_{\mathrm{dR}}(S^1)$ | Forms detect holes |
| Probability/statistics | OLS projection + PCA Rayleigh quotient | Personal bridge into geometry |

---

## 17. What Can Be Safely Black-Boxed at First

### Safe to black-box initially

Full proofs of: inverse function theorem; Sard's theorem; Whitney embedding
theorem; construction of smooth partitions of unity; the de Rham theorem;
Radon–Nikodym theorem; Riesz representation theorem; compactness theorem for
currents; regularity theory for area-minimizing currents; Sobolev embedding
machinery.

### Do not permanently black-box

What tangent/cotangent spaces are; what a differential form is; what
$d\omega$ means; why $d^2=0$; what Stokes' theorem says; what compactness
does; what "almost everywhere" means; what weak convergence means; what
Hausdorff measure is trying to measure; what rectifiability means
conceptually; what currents/varifolds are trying to generalize.

---

## 18. Minimal Study Sequence for Intellectual Practicality

(Learning stages, not the repo's document phases — see the note in `00` §33.)

### Stage 1 — Smooth geometry literacy

1. Linear algebra: dual spaces, determinants, symmetric operators, exterior
   algebra basics.
2. Multivariable analysis: derivative as linear map, parametrized
   curves/surfaces, Jacobians, Green/Stokes/Gauss.
3. Topology: open sets, compactness, connectedness, quotient spaces,
   manifolds.
4. Differential geometry foundations: smooth manifolds, tangent/cotangent
   spaces, forms, Stokes, metrics, covariant derivatives, curvature.

### Stage 2 — Measurable geometry upgrade

1. Measure theory: $\sigma$-algebras, Lebesgue integral, convergence
   theorems, $L^p$, Radon measures, Hausdorff measure.
2. Functional analysis: Hilbert/Banach, weak convergence, duality,
   distributions, Sobolev/BV intuition.
3. GMT intro: rectifiable sets, area/coarea formula, currents, varifolds,
   finite perimeter, compactness/lower semicontinuity.

### Stage 3 — Variational geometry

Minimal surfaces; first variation; mean curvature; weak solutions;
regularity/singularity split; Plateau problem; geometric flows if desired.

---

## 19. The Repository Interpretation

> Smooth manifolds are the ideal differentiable case. GMT studies which
> parts of manifold theory survive when the object is only measurable,
> rectifiable, variational, or weakly convergent.

A clean README-level claim:

```text
This repo studies geometry after smoothness stops being guaranteed.
The organizing contrast is not "calculus vs. no calculus," but rather
classical pointwise differentiability vs. measure-theoretic differentiability,
rectifiability, weak convergence, and generalized integration.
```

Another version:

```text
Differential geometry gives the smooth model of tangent spaces, forms,
metrics, curvature, and Stokes-type theorems. Geometric measure theory asks
which of these structures survive for rough sets and weak limits: Hausdorff
measure replaces smooth area, approximate tangent planes replace tangent
bundles, currents/varifolds replace smooth submanifolds, and BV/finite
perimeter theory replaces classical boundaries.
```

---

## 20. Personal "Do Not Over-Study" Rules

1. Do not redo a whole textbook because a theorem appears once.
2. When a theorem appears repeatedly, upgrade it from black-box to
   proof-sketch.
3. When a theorem becomes central to the repo's goals, prove it carefully.
4. Learn definitions with domains/codomains explicit — always write the type.
5. Track whether an object is pointwise, local, global, smooth, measurable,
   a.e., or weak/distributional.
6. Always ask: What is the object (its type)? What structure does it
   require? What theorem makes it useful? What breaks if smoothness is
   removed? What measure-theoretic replacement survives?

---

## 21. One-Page Compressed Map

```text
Linear algebra:
    Local geometry. Tangent spaces, cotangent spaces, metrics, forms.

Multivariable analysis:
    Euclidean prototype. Jacobians, parametrizations, vector calculus Stokes.

Real analysis:
    Limit discipline. Completeness, compactness, convergence.

Topology:
    Global shape. Manifolds, charts, gluing, compactness, connectedness.

Measure theory:
    Irregular size/integration. Lebesgue, Radon, Hausdorff, a.e., convergence.

Functional analysis:
    Infinite-dimensional geometry. Hilbert spaces, weak convergence, duality.

Differential equations/PDE:
    Dynamics and variational geometry. Flows, geodesics, Laplacian, weak solutions.

Complex analysis:
    Rigid 2D geometry. Holomorphic maps, conformality, Riemann surfaces.

Algebraic topology/de Rham:
    Holes via calculus. Cycles, boundaries, closed/exact forms, cohomology.

Probability/statistics:
    Measures + geometry of inference. Empirical measures, projection, Fisher/KL, Wasserstein.

GMT:
    Geometry beyond smoothness. Rectifiability, Hausdorff measure, currents, varifolds, BV.
```

---

## 22. References Used

Full bibliographic data for every short key: `notes/NOTATION.md` §4. These are
reliable anchors, not a complete bibliography, organized by where they were
cited above.

### Differential geometry / forms
- **Spivak, CoM** — §5, §11 (Green's theorem, Cauchy formula analogy in style).
- **Lee, ISM**; **Lee, IRM**; **do Carmo, RG**; **do Carmo, DGCS** — §14.
- MIT OpenCourseWare, **18.950 Differential Geometry**; **18.952 Differential
  Forms**; **18.965 Geometry of Manifolds** — supplementary lecture-note
  anchors for the same material.

### Analysis / topology / measure / functional analysis
- **Rudin, RCA** — §6 (Heine–Borel), §8 (DCT), §9 (Hilbert projection).
- **Munkres, Topology** — §7 (continuous image of compact is compact).
- MIT OpenCourseWare, **18.100B Real Analysis**; **18.125 Measure and
  Integration**; **18.102 Introduction to Functional Analysis**; **18.905
  Algebraic Topology I** — supplementary.

### Vector calculus / PDE / complex analysis
- MIT OpenCourseWare, **18.02SC Multivariable Calculus**; **18.03
  Differential Equations**; **18.303 Linear PDE**; **18.04 Complex
  Variables**; **18.112 Functions of a Complex Variable** — supplementary,
  §5, §10, §11.

### Algebraic topology / de Rham
- **Tu, Introduction to Manifolds**; **Bott–Tu** — §12.

### GMT
- **Federer, GMT**; **Simon, LGMT**; **Evans–Gariepy, MTFP**; **Maggi,
  SFP&GVP**; **Falconer** — §15; full use in `notes/06`–`09`.

### Personal course bridge
- PSTAT 126 notes: OLS geometry, projection matrices, empirical
  distributions, $W_1$, variance-stabilizing transformations,
  multicollinearity via eigenvalues/VIF, ridge/lasso, PCA/SVD, GLS/WLS, MLE,
  Fisher information, KL divergence, Newton/IRLS, Hilbert-space $L^2$
  regression intuition — §13.

---

## 23. Final Guiding Sentence

$$
\text{Euclidean calculus} \to \text{smooth coordinate-free geometry} \to \text{measure-theoretic geometry} \to \text{weak/variational geometry.}
$$

Differential geometry tells you what the world looks like when tangent
planes vary smoothly. GMT asks what remains true when tangent planes exist
only almost everywhere, boundaries are measure-theoretic, and surfaces are
weak limits rather than smooth objects.

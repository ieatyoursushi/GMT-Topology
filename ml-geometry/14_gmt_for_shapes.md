# 14 — Geometric Measure Theory for Shapes: Point Clouds, Meshes, and Registration

**Cross-references.** `ml-geometry/NOTATION_ML.md` (typing §1.5, citation
key §3.1–3.2); `notes/07_rectifiability_and_hausdorff_measure.md` (Lipschitz
maps, the linear-map area formula — reused directly in §3); `notes/06_measure_theory_bridge.md`
§4 (Dominated Convergence Theorem — reused directly in §3); `notes/08_currents.md`
(currents, boundary by duality); `notes/09_varifolds_and_finite_perimeter.md`
(varifolds, currents-vs-varifolds type-check, finite perimeter — all reused
below). This document is the topic doc most tightly bound to the capstone,
`16_capstone_directindexing_gmt.md`.

---

## 0. Standing Definitions

$$
X=\{x_1,\dots,x_N\}\subseteq\mathbb{R}^n \quad \text{a point cloud}, \qquad \hat\mu_X := \tfrac1N\sum_{i=1}^N\delta_{x_i} \quad \text{its empirical measure}.
$$

$$
\Omega\subseteq\mathbb{R}^{n-1} \text{ bounded open}, \qquad X:\overline\Omega\to\mathbb{R}^n \quad C^1 \text{ embedding}, \qquad M:=X(\overline\Omega) \quad \text{a compact $C^1$ hypersurface patch}.
$$

$$
\mathcal{T}_h \quad \text{a shape-regular triangulation of } \overline\Omega \text{ with mesh size } h>0, \qquad X_h:\overline\Omega\to\mathbb{R}^n \quad \text{the piecewise-affine (nodal) interpolant of } X \text{ on } \mathcal{T}_h.
$$

$$
T,T_h \in D_{n-1}(\mathbb{R}^n): \qquad T(\omega):=\int_{\overline\Omega} X^*\omega, \qquad T_h(\omega):=\int_{\overline\Omega} X_h^*\omega, \qquad \omega\in\Omega_c^{n-1}(\mathbb{R}^n).
$$

---

## 1. Shapes as Currents (Oriented) vs. Varifolds (Unoriented)

A discretized surface — a point cloud, a triangle mesh, a polygon soup —
can be represented as a GMT object in (at least) two ways, exactly the
distinction already built in `notes/08`–`09`:

- As a **current** (`notes/08` Definition 1.1, 1.3): requires a consistent
  choice of orientation (e.g. consistently outward-pointing mesh normals).
  Well-suited when orientation is known and consistent across a dataset.
- As a **varifold** (`notes/09` Definition 1.1–1.2): requires only a
  tangent-plane direction at each point, no sign. Well-suited when
  orientation is inconsistent, unknown, or not meaningful — e.g. raw scanner
  point clouds, where normal direction may flip arbitrarily across a
  surface with no ground truth for "which way is outward."

> **Type-check (direct specialization of `notes/09`'s currents-vs-varifolds
> box).** For shape *comparison and registration* — the setting Charon–Trouvé
> 2013 addresses — datasets routinely contain shapes whose orientation
> conventions do not agree (different scanning protocols, meshes built by
> different pipelines). Representing shapes as varifolds sidesteps this by
> construction: two meshes of "the same surface, traversed oppositely" give
> the *same* current only up to a sign but the identical varifold (`notes/09`
> §1's type-check, restated here for the shape-ML setting). This is why the
> varifold representation, not the current representation, is standard in
> shape-analysis ML pipelines built on this machinery.

---

## 2. Kernel Metrics for Shape Comparison

Both currents and varifolds are, by construction, continuous linear
functionals on a space of test objects (test forms for currents;
implicitly, test vector fields for the first-variation functional of a
varifold, `notes/09` Definition 1.3). This makes them elements of a dual
space, and a standard trick converts this into a computable **metric**
between shapes: fix a reproducing kernel Hilbert space (RKHS) $\mathcal{W}$
of test objects, giving each current/varifold $T$ a well-defined RKHS-dual
norm $\lVert T\rVert_{\mathcal{W}^*}$, and hence a distance
$\lVert T_1-T_2\rVert_{\mathcal{W}^*}$ between two shapes' representations
— computable in closed form when $\mathcal{W}$'s kernel is (e.g.) Gaussian,
since $\langle T_1,T_2\rangle_{\mathcal{W}^*}$ reduces to a finite sum over
pairs of mesh faces/points. This is the mechanism (cited at recognition
depth; Vaillant–Glaunès 2005 for currents, Charon–Trouvé 2013 for
varifolds) by which the purely GMT objects of `notes/08`–`09` become a
usable *loss function* for shape registration in an ML pipeline.

---

## 3. Anchor Proof: Mesh Currents Converge to the Smooth Surface's Current

This is the precise justification for "a point cloud/mesh is a legitimate
discrete stand-in for a smooth shape's current" — i.e. the theoretical
bridge underlying §1–2's practice.

**Theorem 3.1.** With notation as in §0 ($X\in C^1(\overline\Omega')$ for
some open $\overline\Omega\subset\Omega'$, $\{\mathcal{T}_h\}$ shape-regular
with mesh size $h\to0$), $T_h(\omega)\to T(\omega)$ for every
$\omega\in\Omega_c^{n-1}(\mathbb{R}^n)$.

*Proof.* **Step 1 (pullback as a fixed continuous function of the Jacobian).**
Write $\omega = \sum_{i=1}^n (-1)^{i-1} f_i\,dx^1\wedge\cdots\wedge\widehat{dx^i}\wedge\cdots\wedge dx^n$
as in `notes/03` §4.1. For any Lipschitz $Y:\overline\Omega\to\mathbb{R}^n$
with Jacobian $DY(y)\in\mathbb{R}^{n\times(n-1)}$ (defined a.e. by
Rademacher's theorem, `notes/07` Theorem 1.2 — for the piecewise-affine
$X_h$ below, this holds trivially off the finite mesh skeleton, a
$\mathcal{L}^{n-1}$-null set), the standard multilinear pullback formula
(direct generalization of the wedge-product bookkeeping of `notes/03` §1,
via the Cauchy–Binet identity relating a matrix's maximal minors to
$\det(A^\top A)$ — the same identity underlying the linear-map area formula,
`notes/07` Theorem 3.1) gives

$$
Y^*\omega\big|_y = \varphi\big(DY(y),\,Y(y)\big)\,dy^1\wedge\cdots\wedge dy^{n-1}, \qquad \varphi(A,z) := \sum_{i=1}^n(-1)^{i-1}f_i(z)\cdot\operatorname{minor}_i(A),
$$

where $\operatorname{minor}_i(A)$ is the determinant of the $(n-1)\times(n-1)$
matrix obtained by deleting row $i$ from $A$. Crucially, $\varphi$ is a
**fixed function**, continuous jointly in $(A,z)\in\mathbb{R}^{n\times(n-1)}
\times\mathbb{R}^n$ (a polynomial in the entries of $A$ for each fixed $z$,
and continuous in $z$ since each $f_i\in C_c^\infty$), independent of which
map $Y$ is plugged in. So

$$
T(\omega) = \int_{\overline\Omega} \varphi(DX(y),X(y))\,dy, \qquad T_h(\omega) = \int_{\overline\Omega} \varphi(DX_h(y),X_h(y))\,dy.
$$

**Step 2 (uniform interpolation estimates).** Since $X\in C^1$ on a
neighborhood of the compact set $\overline\Omega$, $DX$ is uniformly
continuous there (Heine–Cantor, `01` §6.2) and bounded: $\lVert
DX(y)\rVert\le L<\infty$ for all $y\in\overline\Omega$. Standard
piecewise-affine (nodal/$P^1$) interpolation error estimates for a
shape-regular triangulation family — a standard numerical-analysis fact,
cited here rather than reproved — give, as $h\to0$:

$$
\sup_{y\in\overline\Omega} |X_h(y)-X(y)| \to 0, \qquad DX_h(y)\to DX(y) \text{ for } \mathcal{L}^{n-1}\text{-a.e. } y\in\overline\Omega,
$$

with $\lVert DX_h(y)\rVert \le L'$ for all sufficiently small $h$ and a.e.
$y$, for a fixed $L'<\infty$ depending only on $L$ and the shape-regularity
constant (not on $h$).

**Step 3 (dominated convergence).** By Step 2 and continuity of $\varphi$,
$\varphi(DX_h(y),X_h(y)) \to \varphi(DX(y),X(y))$ for a.e. $y\in\overline\Omega$
as $h\to0$. Domination: $\varphi$ is continuous, hence bounded, on the
compact set $\{A:\lVert A\rVert\le L'\}\times K$ where $K\supseteq
X_h(\overline\Omega)$ for all small $h$ is a fixed compact set (exists since
$X_h\to X$ uniformly, Step 2, so all the $X_h(\overline\Omega)$ eventually
lie in a fixed bounded neighborhood of $X(\overline\Omega)$); write $C:=
\sup$ of $|\varphi|$ on this compact set. Then $|\varphi(DX_h(y),X_h(y))|\le
C$ for all small $h$ and a.e. $y\in\overline\Omega$, and the constant
function $C$ is integrable on the finite-measure set $\overline\Omega$.
By the Dominated Convergence Theorem (`notes/06` Theorem 4.3),

$$
T_h(\omega) = \int_{\overline\Omega}\varphi(DX_h,X_h)\,dy \ \longrightarrow\ \int_{\overline\Omega}\varphi(DX,X)\,dy = T(\omega). \qquad\blacksquare
$$

> **Direct reuse of `notes/07`'s area formula.** Taking $\omega$ to be (a
> compactly-supported cutoff of) the specific form whose pullback density is
> $J_{n-1}(DY(y))$ itself (rather than a general linear combination of
> minors) makes $\int_{\overline\Omega}\varphi(DX_h,X_h)\,dy$ literally the
> **total surface area of the triangulated mesh**, computed simplex-by-simplex
> via `notes/07` Theorem 3.1 (the linear-map area formula, applied to each
> simplex's constant Jacobian). So Theorem 3.1 above simultaneously proves
> mesh surface area converges to the smooth surface's $\mathcal{H}^{n-1}$-measure
> as $h\to0$ — the mass-convergence statement underlying "finer meshes are
> better area/current approximations," not just a test-form-by-test-form
> convergence statement.

---

## 4. Finite Perimeter for Learned Decision Regions

Setting the stage for the capstone: an ML classifier's positive region
$\{x:\hat\eta(x)\ge\tau\}\subseteq\mathcal{X}$ is a set whose *boundary* one
frequently wants to measure or regularize (e.g. penalizing decision-boundary
"roughness"). `notes/09`'s perimeter $P(E)$ and reduced boundary
$\partial^*E$ are exactly the right tools for this — already built,
already proved (`notes/09` Theorem 2.3, 2.4, 3.1) — and the capstone
(`16`) applies them directly to a *specific* such region, adapting the
smooth-boundary anchor of `notes/09` Theorem 3.1 to a region with corners.
**The running example of this section is completed in `16`.**

---

## References Used

- **Charon–Trouvé 2013** — the varifold shape representation (§1, §2), the
  primary anchor for why varifolds rather than currents are standard in
  shape-comparison ML.
- **Vaillant–Glaunès 2005** — currents for shape/surface matching (§2), the
  earlier oriented-representation approach.
- **Kaltenmark–Charon–Trouvé 2017** — a general oriented-varifold kernel
  framework unifying currents and varifolds (§2, cited at recognition
  depth, flagged current).
- **`notes/07_rectifiability_and_hausdorff_measure.md`** — Rademacher's
  theorem and the linear-map area formula (Theorem 3.1 there), both reused
  directly in the anchor proof of §3 above.
- **`notes/06_measure_theory_bridge.md`** Theorem 4.3 (Dominated
  Convergence) — the final step of §3's proof.
- **`notes/08_currents.md`**, **`notes/09_varifolds_and_finite_perimeter.md`**
  — currents, varifolds, finite perimeter, all reused directly in §1 and §4.

Full bibliographic data: `ml-geometry/NOTATION_ML.md` §3.

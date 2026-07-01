# Differential Geometry Foundations for a GMT-Topology Path

**Audience.** Curious accelerated undergraduate with strong multivariable-calculus
intuition, informal exposure to manifolds/forms/topology, and a long-term goal of
understanding geometric measure theory (GMT) — especially spaces that are not
assumed smooth.

**Repository role.** This is the **survey/hub** document: it walks the entire
arc from Euclidean calculus to the smooth-to-GMT bridge at a pace fast enough to
see the whole shape, but with every object fully typed and at least one theorem
proved from scratch per major stage. Full-depth treatments of each stage live in
`notes/02`–`09`; this file links to them at the point where they take over.

**Core idea.**

> Differential geometry studies spaces where calculus works locally.
> GMT asks how much of that geometric calculus survives when the space is only
> measurable, rectifiable, of finite perimeter, varifold-like, current-like, or
> otherwise not classically smooth.

**Cross-references.** `notes/NOTATION.md` (typing conventions, global context,
citation key — read first); `01_prerequisite_systematic_review_for_GMT_Topology.md`
(what background each stage below draws on); `notes/02`–`09` (full-depth
treatment of each stage, with proofs).

---

## 0. Standing Definitions

Everything below additionally uses the global typing context of
`notes/NOTATION.md` §2. Restated here because they are used immediately:

$$
M \quad \text{a smooth manifold}, \qquad \dim M = n \in \mathbb{Z}_{\ge 0},
\qquad p \in M, \qquad U \subseteq \mathbb{R}^n \text{ open}.
$$

$$
T_pM \quad \text{tangent space at } p, \qquad T_p^*M = \operatorname{Hom}(T_pM,\mathbb{R}) \quad \text{cotangent space at } p.
$$

$$
\Omega^k(M) \quad \text{smooth $k$-forms on } M, \qquad d : \Omega^k(M) \to \Omega^{k+1}(M) \quad \text{exterior derivative}.
$$

$$
g \quad \text{a Riemannian metric on } M, \qquad g_p : T_pM \times T_pM \to \mathbb{R} \text{ symmetric, bilinear, positive-definite}.
$$

$$
\mathcal{H}^k \quad k\text{-dimensional Hausdorff measure}, \qquad E \subseteq \mathbb{R}^n \quad \text{a Lebesgue/Hausdorff-measurable set}.
$$

Throughout, Einstein summation is in force (`notes/NOTATION.md` §2.5): a Latin
index repeated once up and once down in a single term is summed over
$1,\dots,n$.

---

## 1. Mental Model

### 1.1 The local-to-global principle

A smooth manifold is a space that may be globally curved, twisted, or
topologically nontrivial, but locally behaves like Euclidean space. Formally:

$$
\varphi : U \subseteq M \to \varphi(U) \subseteq \mathbb{R}^n,
\qquad
U \text{ open}, \qquad \varphi \text{ a homeomorphism onto its image (a chart)}.
$$

The local coordinate variables produced by $\varphi$ are written

$$
(x^1,\dots,x^n) = \varphi(p) \in \mathbb{R}^n, \qquad p \in U.
$$

> **Type-check.** The point is not that $M$ *literally is* $\mathbb{R}^n$. The
> point is that $\varphi$ is a map with a precise type, $U \to \mathbb{R}^n$,
> and calculus on $M$ is performed by moving through $\varphi$ into
> $\mathbb{R}^n$, doing ordinary calculus there, then checking the result does
> not depend on which chart $\varphi$ was chosen.

### 1.2 The three layers

| Layer | Object | Type | Intuition |
|---|---|---|---|
| Topological | $M$ as a space | a set with a topology | What points are near each other? What is continuous? |
| Smooth | an atlas $\{(U_\alpha,\varphi_\alpha)\}$ | charts with $C^\infty$ transition maps | What does differentiability mean? |
| Metric/geometric | $g$, $\nabla$, $R$ | $g_p: T_pM\times T_pM\to\mathbb{R}$, etc. | How do we measure length, angle, volume, bending? |

GMT weakens the smooth layer and asks how much of the metric/measure/
integration layer survives regardless.

Full definitions of charts, atlases, and the smooth-manifold axioms: see
`notes/02_manifolds_and_tangent_spaces.md` §1–2.

---

## 2. Euclidean Calculus as the Seed

Before manifolds, everything lives in an open subset of Euclidean space.

A **scalar field** is

$$
f: U \to \mathbb{R}, \qquad U \subseteq \mathbb{R}^n \text{ open}.
$$

A **vector field** is

$$
F: U \to \mathbb{R}^n,
\qquad
F(x) = \big(F^1(x),\dots,F^n(x)\big),
\qquad
F^i : U \to \mathbb{R} \text{ for each } i.
$$

A **curve** is

$$
\gamma : I \to \mathbb{R}^n, \qquad I \subseteq \mathbb{R} \text{ an interval}.
$$

A **parametrized surface** is

$$
X : \Omega \to \mathbb{R}^3, \qquad \Omega \subseteq \mathbb{R}^2 \text{ open}.
$$

More generally, a **parametrized $k$-dimensional object** in $\mathbb{R}^n$ is a map

$$
X : \Omega \to \mathbb{R}^n, \qquad \Omega \subseteq \mathbb{R}^k \text{ open}, \qquad k \le n.
$$

This is the primitive form of a $k$-dimensional manifold sitting inside an
$n$-dimensional ambient space — and, later, the primitive form of a
Lipschitz parametrization of a rectifiable set (`notes/07`).

---

## 3. Derivatives as Linear Maps

The derivative of a map is not fundamentally a slope. It is the best linear
approximation to the map near a point.

$$
F : U \to \mathbb{R}^m, \qquad U \subseteq \mathbb{R}^n \text{ open}.
$$

At $p \in U$, the derivative is a linear map

$$
DF_p : \mathbb{R}^n \to \mathbb{R}^m,
\qquad
F(p+h) = F(p) + DF_p(h) + o(\lVert h\rVert) \text{ as } h \to 0.
$$

In the standard bases, $DF_p$ is represented by the **Jacobian matrix**

$$
J_F(p) =
\begin{bmatrix}
\dfrac{\partial F^1}{\partial x^1}(p) & \cdots & \dfrac{\partial F^1}{\partial x^n}(p)\\[4pt]
\vdots & \ddots & \vdots\\[4pt]
\dfrac{\partial F^m}{\partial x^1}(p) & \cdots & \dfrac{\partial F^m}{\partial x^n}(p)
\end{bmatrix}
\in \mathbb{R}^{m\times n}.
$$

For $v \in \mathbb{R}^n$, $DF_p(v) \in \mathbb{R}^m$ is the directional output
velocity of $F$ at $p$ in the input direction $v$.

### 3.1 Manifold translation

On manifolds this becomes

$$
dF_p : T_pM \to T_{F(p)}N, \qquad F : M \to N \text{ smooth}.
$$

> **Primitive-vs-derived.** The Jacobian matrix is a *coordinate
> representation* of $DF_p$; $DF_p$ itself is the coordinate-free linear map.
> On a manifold, $dF_p$ is the primitive object and any matrix representing it
> depends on the charts chosen on $M$ and $N$. Full construction and the
> chart-change (Jacobian conjugation) law: `notes/02` §3.

---

## 4. Curves, Tangents, and Arc Length

Let $\gamma : I \to \mathbb{R}^n$ be $C^1$. Its **velocity** is

$$
\gamma'(t) = \frac{d\gamma}{dt}(t) \in \mathbb{R}^n, \qquad t \in I.
$$

The **speed** is $\lVert \gamma'(t)\rVert \in [0,\infty)$. The **arc length**
of $\gamma$ from $a$ to $b$ is

$$
L(\gamma) = \int_a^b \lVert\gamma'(t)\rVert\,dt \in [0,\infty], \qquad a,b \in I, \ a \le b.
$$

This is already a measure-like idea: a 1-dimensional object is measured by a
1-dimensional integral, even though it may live inside $\mathbb{R}^n$ for
$n > 1$. It is the $n=1$, smooth special case of $\mathcal{H}^1$ (`notes/06`
§5).

### 4.1 Reparametrization

If $\tilde\gamma(s) = \gamma(\phi(s))$ for a $C^1$ monotone bijection
$\phi : J \to I$, then $\tilde\gamma$ and $\gamma$ trace the same geometric
curve, even though $\tilde\gamma'(s) = \gamma'(\phi(s))\,\phi'(s) \ne \gamma'(t)$
in general.

> **Type-check.** A well-formed differential-geometric quantity should not
> depend on the arbitrary parametrization chosen to compute it. $L(\gamma)$
> passes this check (verify by substitution); the raw velocity vector
> $\gamma'(t)$, as a number attached to a specific $t$, does not directly —
> only the *reparametrization-covariant* structure of $\gamma'$ survives.

---

## 5. Line Integrals

### 5.1 Scalar line integral

$$
f : \mathbb{R}^n \to \mathbb{R}, \qquad \gamma : [a,b] \to \mathbb{R}^n \text{ ($C^1$)}.
$$

$$
\int_\gamma f\,ds := \int_a^b f(\gamma(t))\,\lVert\gamma'(t)\rVert\,dt \ \in \mathbb{R}.
$$

This integrates a scalar density over a curve; it does not depend on
orientation of $\gamma$.

### 5.2 Vector line integral

$$
F : \mathbb{R}^n \to \mathbb{R}^n.
$$

$$
\int_\gamma F \cdot d\mathbf{r} := \int_a^b F(\gamma(t)) \cdot \gamma'(t)\,dt \ \in \mathbb{R}.
$$

This *is* orientation-sensitive: reversing the orientation of $\gamma$ negates
the integral.

### 5.3 Differential-form translation

A vector field $F = (P,Q,R) : \mathbb{R}^3 \to \mathbb{R}^3$ corresponds to the
1-form

$$
\omega = P\,dx + Q\,dy + R\,dz \ \in \Omega^1(\mathbb{R}^3),
$$

and

$$
\int_\gamma F\cdot d\mathbf{r} = \int_\gamma \omega.
$$

This is the beginning of forms replacing vector-calculus notation; full
treatment in `notes/03`.

---

## 6. Conservative Fields and Potential Functions

$$
F : U \to \mathbb{R}^n, \qquad U \subseteq \mathbb{R}^n \text{ open},
$$

is **conservative** if there exists $f : U \to \mathbb{R}$ with

$$
F = \nabla f.
$$

**Theorem (Fundamental theorem for line integrals).** If $F = \nabla f$ on
$U$ and $\gamma : [a,b] \to U$ is $C^1$, then

$$
\int_\gamma F \cdot d\mathbf{r} = f(\gamma(b)) - f(\gamma(a)).
$$

*(Spivak, CoM, Ch. 4; a direct consequence of the single-variable FTC applied
to $t \mapsto f(\gamma(t))$ via the chain rule.)*

So a conservative field has path-independent line integrals. In form language,

$$
F = \nabla f \iff \omega = df, \qquad df \in \Omega^1(U) \text{ the exterior derivative of the 0-form } f.
$$

### 6.1 Exact vs. closed

$\omega \in \Omega^1(U)$ is **exact** if $\omega = df$ for some
$f \in \Omega^0(U)$; it is **closed** if $d\omega = 0$.

**Proposition.** Every exact 1-form is closed: $d(df) = d^2f = 0$.

The converse depends on the topology of $U$: on a simply connected domain,
closed 1-forms are exact (Poincaré lemma, `notes/05` §3); on domains with
holes, closed does not imply exact (`notes/05` §4). This is where topology
enters the calculus.

---

## 7. From Euclidean Domains to Manifolds

### 7.1 Topological manifold

An $n$-dimensional **topological manifold** is a topological space $M$ (usually
also required Hausdorff and second-countable, `notes/02` §1) such that every
$p \in M$ has an open neighborhood $U \subseteq M$ and a homeomorphism

$$
\varphi : U \to \varphi(U) \subseteq \mathbb{R}^n.
$$

A **chart** is the pair $(U,\varphi)$; an **atlas** is a collection of charts
whose domains cover $M$.

### 7.2 Smooth manifold

A **smooth manifold** is a topological manifold equipped with an atlas whose
charts are smoothly compatible: for overlapping charts $(U,\varphi)$,
$(V,\psi)$ with $U \cap V \ne \varnothing$, the **transition map**

$$
\psi \circ \varphi^{-1} : \varphi(U\cap V) \to \psi(U\cap V)
$$

is a $C^\infty$ diffeomorphism between open subsets of $\mathbb{R}^n$.

> **Type-check.** $\varphi$ and $\psi$ each have type $M \supseteq (\cdot) \to
> \mathbb{R}^n$; they are not directly composable. $\psi\circ\varphi^{-1}$ is
> the well-typed composite $\mathbb{R}^n \supseteq (\cdot) \to \mathbb{R}^n
> \supseteq (\cdot)$, and *that* is the object required to be smooth. The
> smoothness condition is a statement about a map between open sets of
> $\mathbb{R}^n$ — ordinary multivariable calculus — even though it encodes a
> global compatibility condition on $M$.

Full atlas/manifold construction, including the point-set hypotheses that
prevent pathological gluings: `notes/02` §1–2.

---

## 8. Tangent Spaces

For $p \in M$, the tangent space $T_pM$ is an $n$-dimensional real vector space
attached to $p$. Three equivalent constructions (proved equivalent in
`notes/02` §3):

**(i) Velocities of curves.** $\gamma : (-\varepsilon,\varepsilon) \to M$,
$\gamma(0) = p$; $\gamma'(0)$ represents a tangent vector, with two curves
identified when their images under any chart have the same derivative at $0$.

**(ii) Derivations.** $v : C^\infty(M) \to \mathbb{R}$ linear, satisfying the
Leibniz rule at $p$:

$$
v(af+bg) = a\,v(f) + b\,v(g), \qquad v(fg) = f(p)\,v(g) + g(p)\,v(f), \qquad a,b\in\mathbb{R},\ f,g\in C^\infty(M).
$$

**(iii) Coordinate basis.** Given a chart with coordinates
$(x^1,\dots,x^n)$, $T_pM$ has basis

$$
\left\{ \frac{\partial}{\partial x^1}\Big|_p, \dots, \frac{\partial}{\partial x^n}\Big|_p \right\},
\qquad
v = v^i \frac{\partial}{\partial x^i}\Big|_p \in T_pM, \qquad v^i \in \mathbb{R}.
$$

> **Type-check.** Definition (ii) is the one that makes precise the slogan "a
> tangent vector is a direction, without reference to any ambient embedding
> or chart" — it typechecks purely in terms of $C^\infty(M) \to \mathbb{R}$,
> using no coordinates at all. The full proof that (i), (ii), and (iii)
> define isomorphic $n$-dimensional vector spaces is the anchor proof of
> `notes/02`.

---

## 9. Vector Fields and Flows

A **vector field** assigns a tangent vector to each point:

$$
X : M \to TM, \qquad X(p) \in T_pM \text{ for each } p, \qquad TM = \bigsqcup_{p\in M} T_pM.
$$

In coordinates, $X = X^i \,\partial/\partial x^i$, $X^i \in C^\infty(U)$.

A **flow line** (integral curve) of $X$ is $\gamma : I \to M$ with

$$
\gamma'(t) = X(\gamma(t)), \qquad t \in I.
$$

So an ODE is geometrically the rule: *the tangent vector of the curve must
equal the vector field at each point.* This is why flows and ODE existence
theory are intrinsically differential-geometric (see `01` §9).

---

## 10. Cotangent Spaces and 1-Forms

$$
T_p^*M = \operatorname{Hom}(T_pM,\mathbb{R}).
$$

A **1-form** is a smooth assignment $p \mapsto \omega(p) \in T_p^*M$. In the
coordinate cotangent basis $\{dx^1|_p,\dots,dx^n|_p\}$ (dual to
$\{\partial/\partial x^i|_p\}$), $\omega = \omega_i\,dx^i$, $\omega_i \in
C^\infty(U)$, and

$$
\omega(X) = \omega_i X^i \in C^\infty(M), \qquad X = X^i\,\partial/\partial x^i.
$$

> **Type-check — primitive vs. derived.** A vector field is a direction at
> each point: $X(p) \in T_pM$. A 1-form is a *measuring device* for
> directions at each point: $\omega(p) \in T_p^*M = \operatorname{Hom}(T_pM,
> \mathbb{R})$. These are dual types. A Riemannian metric $g$ can convert one
> into the other (§16.1 below), but **without** a choice of $g$, they are
> different kinds of objects and there is no canonical map between them.

---

## 11. Differential Forms

A $k$-form at $p$ is an alternating multilinear map

$$
\omega_p : \underbrace{T_pM \times \cdots \times T_pM}_{k} \to \mathbb{R}, \qquad \omega_p \in \Lambda^k(T_p^*M).
$$

$\Omega^k(M)$ denotes the smooth $k$-forms on $M$; $\Omega^0(M) = C^\infty(M)$.
A 1-form eats one tangent vector; a 2-form eats two and measures oriented
infinitesimal area; a 3-form eats three and measures oriented infinitesimal
volume. On an $n$-manifold, $\Omega^k(M) = 0$ for $k > n$.

The **wedge product**

$$
\wedge : \Omega^k(M) \times \Omega^\ell(M) \to \Omega^{k+\ell}(M)
$$

is graded-antisymmetric: $dx^i \wedge dx^j = -dx^j\wedge dx^i$, so in
particular $dx^i \wedge dx^i = 0$. Full construction from $\Lambda^k(V^*)$,
including the proof that $\wedge$ is well-defined and associative:
`notes/03` §1.

---

## 12. Exterior Derivative

$$
d : \Omega^k(M) \to \Omega^{k+1}(M), \qquad d^2 = 0 \text{ (i.e. } d(d\omega)=0 \text{ for every } \omega).
$$

`notes/03` §2 proves $d^2=0$ in full as its anchor proof; here we record the
low-degree formulas that recover ordinary vector calculus.

### 12.1 Degree 0

$f \in \Omega^0(M) \implies df \in \Omega^1(M)$, $df = \frac{\partial f}{\partial x^i}dx^i$
— the gradient information, before any metric is chosen.

### 12.2 Degree 1 in $\mathbb{R}^3$

$\omega = P\,dx + Q\,dy + R\,dz \implies$

$$
d\omega = (Q_x - P_y)\,dx\wedge dy + (R_x - P_z)\,dx\wedge dz + (R_y - Q_z)\,dy\wedge dz.
$$

This corresponds to curl, after using the Euclidean metric and orientation to
identify 2-forms with vector fields.

### 12.3 Degree 2 in $\mathbb{R}^3$

$\eta = P\,dy\wedge dz + Q\,dz\wedge dx + R\,dx\wedge dy \implies$

$$
d\eta = (P_x+Q_y+R_z)\,dx\wedge dy\wedge dz,
$$

corresponding to divergence.

### 12.4 Translation table

| Form statement | Type | Vector-calculus shadow in $\mathbb{R}^3$ |
|---|---|---|
| $d : \Omega^0 \to \Omega^1$ | $C^\infty(M) \to \Omega^1(M)$ | gradient |
| $d : \Omega^1 \to \Omega^2$ | $\Omega^1(M)\to\Omega^2(M)$ | curl |
| $d : \Omega^2 \to \Omega^3$ | $\Omega^2(M)\to\Omega^3(M)$ | divergence |
| $d^2=0$ | $\Omega^k(M)\to\Omega^{k+2}(M)$ is the zero map | $\nabla\times(\nabla f)=0$, $\nabla\cdot(\nabla\times F)=0$ |

> Gradient, curl, and divergence are not three unrelated identities to
> memorize. They are the coordinate/metric-dependent shadows, in successive
> degrees, of one operator: $d$, together with $d^2 = 0$.

---

## 13. Pullbacks

$F : M \to N$ smooth. The derivative pushes tangent vectors forward:

$$
dF_p : T_pM \to T_{F(p)}N.
$$

Forms pull back in the opposite direction:

$$
F^* : \Omega^k(N) \to \Omega^k(M), \qquad \omega \in \Omega^k(N) \implies F^*\omega \in \Omega^k(M).
$$

For a parametrization $X : \Omega \to M$, $\Omega \subseteq \mathbb{R}^k$ open,

$$
\int_{X(\Omega)} \omega = \int_\Omega X^*\omega.
$$

This is the coordinate-free version of substitution/Jacobians, and it is
exactly the computational tool that later produces the area formula
(`notes/07` §3) once $X$ is only assumed Lipschitz rather than smooth.

---

## 14. Orientation and Boundary

An **orientation** on an $n$-manifold is a consistent choice of positively
ordered basis in each tangent space. For an oriented manifold-with-boundary
$M$, the **boundary** $\partial M$ is an $(n-1)$-manifold, inheriting an
orientation from $M$.

| $M$ | $\partial M$ |
|---|---|
| interval $[a,b]$ | endpoints, oriented as $b - a$ |
| disk $D^2$ | circle $S^1$ |
| solid ball $B^3$ | sphere $S^2$ |
| closed sphere $S^2$ | $\varnothing$ |

Slogan: boundaries are one dimension lower. If $\dim M = k$ then
$\dim \partial M = k-1$.

---

## 15. Integration on Manifolds

If $\omega \in \Omega^k(M)$ and $S \subseteq M$ is oriented and
$k$-dimensional, then $\int_S \omega$ is defined (via pullback to
coordinates, §13).

> **Type-check — the degree-matching rule.** $k$-forms integrate over
> oriented $k$-dimensional objects, and *only* over such objects. This is
> more structural than memorizing $ds$, $dA$, $dV$ as separate notations:
> those are measure elements, while forms additionally encode orientation.

| Object | Dimension | Integrand type |
|---|---:|---|
| curve | 1 | $\omega \in \Omega^1$ |
| surface | 2 | $\omega \in \Omega^2$ |
| volume | 3 | $\omega \in \Omega^3$ |
| $k$-submanifold | $k$ | $\omega \in \Omega^k$ |

---

## 16. Generalized Stokes Theorem

**Theorem (Stokes).** Let $M$ be an oriented smooth $k$-manifold with boundary
$\partial M$ (with induced orientation), and let $\omega \in \Omega^{k-1}(M)$
have compact support. Then

$$
\boxed{\int_M d\omega = \int_{\partial M} \omega.}
$$

*(Lee, ISM, Thm 16.11; proved in full, including the boundary-chart case, in
`notes/03` §3.)*

This says: the integral of a derivative over an interior equals the integral
of the original quantity over the boundary. It does **not** say the boundary
determines all interior information — only that this specific
derivative-accumulation has a boundary representation.

### 16.1 Special cases

| Theorem | $M$ | $\partial M$ | Form degree |
|---|---|---|---|
| Fundamental theorem of calculus | interval | endpoints | 0-form |
| FT for line integrals | curve | endpoints | 0-form |
| Green's theorem | planar region | boundary curve | 1-form |
| Classical Stokes | surface | boundary curve | 1-form |
| Divergence theorem | solid region | boundary surface | 2-form |

Each is the general theorem specialized to a particular $(M,\partial M,
\omega)$ triple; `notes/03` §4 derives each as a typed corollary.

### 16.2 Why this matters for GMT

GMT generalizes both $M$ and $\partial M$. Instead of requiring $M$ smooth,
GMT works with currents, rectifiable sets, sets of finite perimeter, or
varifolds, and asks: can boundary, tangent planes, orientation, area, and
integration still be defined? `notes/08` shows that for currents, Stokes
becomes essentially *definitional*: $\partial T(\omega) := T(d\omega)$.

---

## 17. Riemannian Metrics

A smooth manifold has calculus, but not automatically lengths or angles. A
**Riemannian metric** is

$$
g : p \mapsto g_p, \qquad g_p : T_pM \times T_pM \to \mathbb{R} \text{ symmetric, bilinear, positive-definite, for every } p \in M.
$$

In coordinates, $g = g_{ij}\,dx^i \otimes dx^j$, $g_{ij} \in C^\infty(U)$,
$g_{ij} = g_{ji}$. For $v = v^i\partial_i$, $w = w^j\partial_j \in T_pM$,

$$
g(v,w) = g_{ij}v^iw^j \in \mathbb{R}, \qquad \lVert v\rVert_g = \sqrt{g(v,v)}, \qquad \cos\theta = \frac{g(v,w)}{\lVert v\rVert_g \lVert w\rVert_g}.
$$

$(M,g)$ is a **Riemannian manifold**. Once $g$ exists, one can define: length
of curves, angle, area/volume, gradient, divergence, Laplacian, geodesics,
curvature — all derived objects, built on top of the primitive smooth
structure. Full construction: `notes/04` §1.

### 17.1 Gradients on Riemannian manifolds

On $\mathbb{R}^n$ with the Euclidean metric, $df$ is casually identified with
$\nabla f$. Intrinsically the more primitive object is $df \in T_p^*M$. The
**gradient** is the unique vector field $\nabla f \in \Gamma(TM)$ satisfying

$$
g(\nabla f, X) = df(X) \quad \text{for every } X \in \Gamma(TM).
$$

> **Type-check.** $df$ exists as soon as $f \in C^\infty(M)$ — no metric
> required. $\nabla f$ requires $g$: it is defined by *inverting* the map
> $T_pM \to T_p^*M$, $v \mapsto g(v,\cdot)$, which requires $g$ to be
> nondegenerate (it is, by positive-definiteness). Without $g$, $\nabla f$
> does not canonically exist. This musical-isomorphism construction is
> spelled out fully in `notes/04` §1.1.

---

## 18. Covariant Derivatives

Tangent vectors at different points of $M$ live in different vector spaces:
$T_pM \ne T_qM$ for $p\ne q$ (they are not even canonically identifiable in
general). A **connection** $\nabla$ gives a rule for differentiating a vector
field $Y$ along a direction $X$:

$$
\nabla_X Y \in \Gamma(TM), \qquad X, Y \in \Gamma(TM).
$$

In coordinates, $\nabla_{\partial_i}\partial_j = \Gamma^k_{ij}\,\partial_k$,
where the **Christoffel symbols** $\Gamma^k_{ij} \in C^\infty(U)$ are *not*
tensors — they depend on the coordinate chart, and their job is to correct
ordinary partial differentiation so that differentiating a vector field makes
intrinsic sense on a curved space.

**Theorem (Levi-Civita, existence and uniqueness).** Given a Riemannian
metric $g$ on $M$, there is a unique connection $\nabla$ satisfying:

1. metric compatibility: $X\big(g(Y,Z)\big) = g(\nabla_XY,Z) + g(Y,\nabla_XZ)$ for all $X,Y,Z\in\Gamma(TM)$;
2. torsion-freeness: $\nabla_XY - \nabla_YX = [X,Y]$ for all $X,Y \in \Gamma(TM)$.

*(Lee, IRM, Thm 5.10; do Carmo, RG, Ch. 2, Thm 3.6. Proved in full, via the
Koszul formula, as the anchor proof of `notes/04` §2.)*

---

## 19. Geodesics

A **geodesic** is a curve $\gamma : I \to M$ satisfying

$$
\nabla_{\dot\gamma}\dot\gamma = 0, \qquad \dot\gamma(t) \in T_{\gamma(t)}M.
$$

In coordinates this is the second-order nonlinear ODE

$$
\frac{d^2x^k}{dt^2} + \Gamma^k_{ij}\frac{dx^i}{dt}\frac{dx^j}{dt} = 0.
$$

Intuition: a geodesic has zero *intrinsic* acceleration — it may look curved
from an ambient embedding, but intrinsically it is not turning.

| Space | Geodesics |
|---|---|
| $\mathbb{R}^n$ | straight lines |
| sphere $S^2$ | great circles |
| cylinder | helices and straight vertical/horizontal unwraps |

---

## 20. Curvature

Curvature measures failure of Euclidean behavior; there are several distinct
but related notions.

### 20.1 Curvature of a curve

For a unit-speed curve $\gamma(s)$ with unit tangent $T(s) = \gamma'(s)$,

$$
\kappa(s) = \lVert T'(s)\rVert \in [0,\infty).
$$

### 20.2 Gaussian curvature

| Surface | Gaussian curvature $K$ |
|---|---:|
| plane | $0$ |
| cylinder | $0$ |
| sphere of radius $R$ | $1/R^2$ |
| saddle | negative |

The cylinder having $K=0$ is important: it bends *extrinsically* in
$\mathbb{R}^3$, but *intrinsically* it is locally isometric to a plane.

### 20.3 Riemann curvature tensor

$$
R(X,Y)Z = \nabla_X\nabla_YZ - \nabla_Y\nabla_XZ - \nabla_{[X,Y]}Z, \qquad X,Y,Z\in\Gamma(TM), \quad R(X,Y)Z \in \Gamma(TM).
$$

Conceptually: curvature measures path-dependence of parallel transport and
noncommutativity of directional differentiation. Full treatment, including
sectional curvature and its relation to $K$: `notes/04` §3.

---

## 21. Laplacian and PDE Geometry

In Euclidean space, $\Delta f = \sum_i \partial^2 f/\partial(x^i)^2$. On a
Riemannian manifold, the **Laplace–Beltrami operator** is

$$
\Delta_g f = \operatorname{div}(\nabla f) = \frac{1}{\sqrt{|g|}}\partial_i\!\left(\sqrt{|g|}\,g^{ij}\partial_j f\right), \qquad |g| = \det(g_{ij}), \quad (g^{ij}) = (g_{ij})^{-1}.
$$

The **heat equation** on $M$: $u : M\times[0,\infty)\to\mathbb{R}$,
$\partial_t u = \Delta_g u$. The **wave equation**: $\partial_t^2 u =
\Delta_g u$. Same operator, different time behavior; the geometry of $M$
enters diffusion/propagation through $\Delta_g$.

---

## 22. De Rham Cohomology

Because $d^2=0$, every exact form is closed. Define

$$
Z^k(M) = \ker\!\big(d:\Omega^k(M)\to\Omega^{k+1}(M)\big), \qquad B^k(M) = \operatorname{im}\!\big(d:\Omega^{k-1}(M)\to\Omega^k(M)\big),
$$

so $B^k(M) \subseteq Z^k(M)$, and the **$k$-th de Rham cohomology group** is

$$
H^k_{\mathrm{dR}}(M) = Z^k(M)/B^k(M).
$$

De Rham cohomology measures the obstruction to solving $\omega = d\eta$ when
$d\omega = 0$ — it measures "holes" using differential forms.

$$
H^1_{\mathrm{dR}}(S^1) \ne 0 \qquad \text{(the circle has a 1-dimensional hole)}, \qquad H^1_{\mathrm{dR}}(\mathbb{R}^n) = 0.
$$

Both facts are proved, along with the Poincaré lemma, in `notes/05` (anchor
proofs). This is the precise version of why topology affects the existence of
potentials for conservative fields (§6).

---

## 23. Gauss–Bonnet as Local-to-Global Geometry

**Theorem (Gauss–Bonnet).** For a compact, oriented, boundaryless surface $M$,

$$
\int_M K\,dA = 2\pi\,\chi(M), \qquad K = \text{Gaussian curvature}, \quad dA = \text{area element}, \quad \chi(M) \in \mathbb{Z} \text{ Euler characteristic}.
$$

*(do Carmo, DGCS, Ch. 4, §5.)* A prototype local-to-global theorem:

| Local quantity | Global/topological quantity |
|---|---|
| curvature density $K\,dA$ | Euler characteristic $\chi(M)$ |

This is why differential geometry is not just calculus: it uses calculus to
detect topology.

---

## 24. Smoothness vs. Measurability

Smooth differential geometry assumes: $C^\infty$ charts, smooth transition
maps, smooth tangent spaces, smooth vector fields, smooth forms — all
holding at *every* point. GMT asks what happens when these assumptions are
too strong. A set might be measurable but not smooth; rectifiable but not a
manifold everywhere; smooth except at isolated singularities; a weak limit of
smooth manifolds; or an area-minimizer with unavoidable singular points.

> Smooth differential geometry starts with differentiability.
> GMT starts with measure, approximation, and weak tangent structure.

This is analogous to how Lebesgue integration extends the reach of Riemann
integration: a measure-theoretic framework can handle objects too irregular
for the classical smooth picture (`notes/06` §1).

---

## 25. Hausdorff Measure

Lebesgue measure $\mathcal{L}^n$ measures $n$-dimensional volume in
$\mathbb{R}^n$. **Hausdorff measure** $\mathcal{H}^k$, $k \ge 0$ not
necessarily an integer, generalizes "$k$-dimensional size" to arbitrary
subsets of $\mathbb{R}^n$.

| Object in $\mathbb{R}^3$ | Natural measure |
|---|---|
| curve | $\mathcal{H}^1$ |
| surface | $\mathcal{H}^2$ |
| solid region | $\mathcal{H}^3$ |

Full construction of $\mathcal{H}^k$ from the diameter-covering outer measure,
plus the anchor proof $\mathcal{H}^1([0,L]) = L$: `notes/06` §5.

---

## 26. Rectifiable Sets

$E \subseteq \mathbb{R}^n$ is **countably $k$-rectifiable** if there exist
countably many Lipschitz maps $f_j : A_j \to \mathbb{R}^n$, $A_j \subseteq
\mathbb{R}^k$, such that

$$
\mathcal{H}^k\Big(E \setminus \bigcup_{j=1}^\infty f_j(A_j)\Big) = 0.
$$

So $E$ need not be a smooth manifold, but it is still made of countably many
$k$-dimensional Lipschitz pieces, up to an $\mathcal{H}^k$-null remainder.

| Smooth manifold | Rectifiable set |
|---|---|
| smooth charts | Lipschitz parametrizations |
| tangent space everywhere | approximate tangent plane $\mathcal{H}^k$-a.e. |
| smooth area form | $\mathcal{H}^k$-measure |
| classical boundary | measure-theoretic/current boundary |
| pointwise differential geometry | almost-everywhere geometry |

This is one of the first serious bridges from differential geometry to GMT;
full treatment in `notes/07`.

---

## 27. Approximate Tangent Planes

A smooth $k$-manifold has $T_pM$ at *every* point. A rectifiable set may have
an **approximate tangent plane** only for $\mathcal{H}^k$-almost every point:

$$
T_pE \text{ exists for } \mathcal{H}^k\text{-a.e. } p \in E.
$$

"Almost everywhere" is essential, not a hedge — GMT systematically replaces
everywhere-smooth structure with almost-everywhere structure, and $T_pE$'s
existence at a.e. $p$ is itself a theorem (a consequence of Rademacher's
theorem applied to the Lipschitz parametrizations; `notes/07` §2).

---

## 28. Currents

A smooth oriented $k$-submanifold $M \subseteq \mathbb{R}^n$ defines a
functional on compactly supported smooth $k$-forms:

$$
T_M(\omega) = \int_M \omega, \qquad T_M : \Omega_c^k(\mathbb{R}^n) \to \mathbb{R}.
$$

A **$k$-current** generalizes this: it is *primarily* a continuous linear
functional, not a point set,

$$
T : \Omega_c^k(\mathbb{R}^n) \to \mathbb{R}, \qquad T \in D_k(\mathbb{R}^n).
$$

The **boundary of a current** is defined by duality with the exterior
derivative:

$$
\partial T(\omega) := T(d\omega), \qquad \partial T \in D_{k-1}(\mathbb{R}^n).
$$

This makes generalized Stokes nearly definitional, and for a smooth manifold
current, $\partial T_M = T_{\partial M}$ — proved as the anchor result of
`notes/08`. Currents retain orientation, multiplicity, boundary, and
integration, without requiring the underlying object to be a smooth manifold.

---

## 29. Varifolds

Where currents carry orientation, **varifolds** are more measure-like and
require none. A $k$-varifold in $\mathbb{R}^n$ is a Radon measure $V$ on

$$
\mathbb{R}^n \times G(n,k), \qquad V \in V_k(\mathbb{R}^n),
$$

where $G(n,k)$ is the Grassmannian of $k$-planes through the origin — i.e. a
measure on pairs $(x,P)$, $x \in \mathbb{R}^n$ a position and $P \in G(n,k)$ a
candidate tangent-plane direction at $x$. Informally: varifold = distribution
of mass + tangent-plane data, without a sign/orientation attached. Useful for
weak limits of surfaces, especially in minimal surface theory. Full
construction and first variation: `notes/09`.

---

## 30. Sets of Finite Perimeter

$E \subseteq \mathbb{R}^n$ has **finite perimeter** if the distributional
derivative of $\mathbf{1}_E$ is a finite $\mathbb{R}^n$-valued Radon measure.
This moves boundary theory into weak derivatives. GMT distinguishes several
notions of boundary for the same set:

| Boundary type | Meaning |
|---|---|
| topological boundary $\partial E$ | $\overline{E}\setminus\operatorname{int}(E)$ |
| measure-theoretic boundary | points of density neither $0$ nor $1$ |
| reduced boundary $\partial^*E$ | points with a measure-theoretic normal |
| current boundary | boundary defined by action on forms, §28 |

The topological boundary can be pathologically large even when $E$ is
well-behaved measure-theoretically; the reduced boundary $\partial^*E$ is the
one relevant to perimeter and variational problems. Full treatment, including
the generalized Gauss–Green theorem: `notes/09` §2.

---

## 31. Area and Coarea Formulae

The classical change-of-variables theorem uses $\det DF$. GMT generalizes this
via the **area formula** and **coarea formula** for Lipschitz maps

$$
f : \mathbb{R}^m \to \mathbb{R}^n,
$$

integrating over images and level sets using a generalized Jacobian that
exists $\mathcal{L}^m$-a.e. by Rademacher's theorem. The philosophical point:
Jacobians survive beyond smooth maps, but become almost-everywhere,
measure-theoretic objects. This is crucial well beyond geometry — many useful
maps in probability, statistics, and machine learning are Lipschitz or a.e.
differentiable but not smooth everywhere. Precise statements and the
linear-map case worked in full: `notes/07` §3.

---

## 32. The Smooth-to-GMT Dictionary

See `notes/NOTATION.md` §3 for the fully typed version of this table (each row
annotated with the type on both the smooth and GMT side, and what is lost in
the transition). Compressed version:

| Smooth differential geometry | GMT / weak geometry |
|---|---|
| smooth manifold | rectifiable set / current / varifold |
| tangent space $T_pM$ | approximate tangent plane a.e. |
| smooth chart | Lipschitz parametrization |
| smooth boundary $\partial M$ | current boundary / reduced boundary |
| area form $dA$ | Hausdorff measure $\mathcal{H}^k$ |
| vector field | test field / weak vector field |
| differential form | test form |
| Stokes theorem | boundary of current, $\partial T(\omega) = T(d\omega)$ |
| curvature tensor | weak curvature / first variation |
| minimal surface | area-minimizing current / stationary varifold |

---

## 33. What to Learn Next

### Phase 1 — Smooth foundations
Smooth manifolds; tangent/cotangent spaces; vector fields; differential
forms; exterior derivative; integration on manifolds; generalized Stokes.
Goal: $\int_{\partial M}\omega = \int_M d\omega$ should feel like the master
theorem behind vector calculus. (`notes/02`, `notes/03`)

### Phase 2 — Riemannian geometry
Metrics; gradient/divergence/Laplacian; covariant derivatives; geodesics;
curvature; volume forms; Gauss–Bonnet. Goal: understand how geometry is
encoded by $g$ and its derivative structure. (`notes/04`)

### Phase 3 — Topology interface
Fundamental group; homology; cohomology; de Rham cohomology; degree theory;
compactness and orientability. Goal: understand why calculus detects holes
and global structure. (`notes/05`)

### Phase 4 — Measure-theoretic geometry
Measure theory; Hausdorff measure; Lipschitz maps; rectifiability; area/coarea
formulae; BV functions; finite-perimeter sets. Goal: replace everywhere-smooth
structure with almost-everywhere structure. (`notes/06`, `notes/07`)

### Phase 5 — GMT objects
Currents; integral currents; varifolds; first variation; monotonicity
formulae; minimal surfaces; regularity and singularities. Goal: understand
geometric objects as weak limits, measures, and integration functionals.
(`notes/08`, `notes/09`)

---

## 34. Core Intuitions to Keep

**34.1** A manifold is a space where calculus works locally: near each point,
the space can be coordinatized by $\mathbb{R}^n$.

**34.2** Tangent spaces are local linearizations: $T_pM$ is the best linear
approximation to $M$ at $p$.

**34.3** Forms are the natural objects of integration: a $k$-form integrates
over a $k$-dimensional oriented object, replacing ad hoc vector-calculus
formulas with one degree-matching rule.

**34.4** Stokes is the master theorem: $\int_M d\omega = \int_{\partial M}
\omega$ unifies FTC, Green, classical Stokes, and the divergence theorem.

**34.5** The metric is extra structure: a smooth manifold does not
automatically know length or angle; $g$ adds those.

**34.6** Curvature is noncommutativity of local linear comparison: it appears
when moving vectors around depends on the path taken.

**34.7** GMT keeps geometry after smoothness breaks: it asks for the weakest
structure needed to preserve length, area, boundary, tangent planes, and
variational calculus.

---

## 35. Repository Structure

```text
GMT-Topology/
├── README.md
├── 00_differential_geometry_foundations_for_GMT_Topology.md
├── 01_prerequisite_systematic_review_for_GMT_Topology.md
└── notes/
    ├── NOTATION.md
    ├── 02_manifolds_and_tangent_spaces.md
    ├── 03_differential_forms_and_stokes.md
    ├── 04_riemannian_metrics_and_curvature.md
    ├── 05_topology_and_de_rham_cohomology.md
    ├── 06_measure_theory_bridge.md
    ├── 07_rectifiability_and_hausdorff_measure.md
    ├── 08_currents.md
    └── 09_varifolds_and_finite_perimeter.md
```

---

## 36. Minimal Problem Set for Self-Calibration

These are concept checks, not computational busywork.

**Problem 1.** $\gamma(t) = (\cos t, \sin t)$, $t \in [0,2\pi]$. Compute
$\gamma'(t)$, $\lVert\gamma'(t)\rVert$, $L(\gamma)$. Interpret as integrating a
1-dimensional measure over a curved object.

**Problem 2.** $F(x,y) = (-y,x)$. Compute $\int_{S^1}F\cdot d\mathbf{r}$. Write
the corresponding 1-form $\omega = -y\,dx + x\,dy$, compute $d\omega$, and
interpret via Green's theorem.

**Problem 3.** $f(x,y,z) = x^2+y^2+z^2$. Compute $df$, then use the Euclidean
metric to identify $df$ with $\nabla f$. Explain why $df$ is more primitive
than $\nabla f$ (§17.1).

**Problem 4.** Let $S^2 \subseteq \mathbb{R}^3$. Explain why $S^2$ is a
2-dimensional manifold despite living in 3-dimensional space. What is
$T_pS^2$ for $p \in S^2$?

**Problem 5.** State generalized Stokes and identify $M$, $\partial M$,
$\omega$ for: FTC, Green's theorem, classical Stokes, the divergence theorem.

**Problem 6.** Give an example of a space that is not smooth everywhere but
should still have a reasonable notion of length or area. Explain why this
motivates Hausdorff measure or rectifiability.

---

## 37. References Used

- **Spivak, CoM** — Ch. 4 (fundamental theorem for line integrals), Ch. 5
  (Stokes' theorem).
- **Lee, ISM** — Ch. 3 (tangent vectors), Ch. 14–16 (forms, orientation,
  Stokes; Thm 16.11 stated in §16 above).
- **Lee, IRM** — Ch. 5 (Levi-Civita connection, Thm 5.10, stated in §18 above).
- **do Carmo, RG** — Ch. 2 (connections), Ch. 4 (curvature).
- **do Carmo, DGCS** — Ch. 3–4 (Gaussian curvature, Gauss–Bonnet).
- **Federer, GMT**; **Simon, LGMT**; **Evans–Gariepy, MTFP**; **Maggi,
  SFP&GVP** — for the GMT-bridge sections (§25–31); precise citations for each
  result are given in `notes/06`–`09`, where the results are proved.

Full bibliographic data for all short keys: `notes/NOTATION.md` §4.

---

## 38. One-Sentence Summary

Differential geometry builds calculus on smooth spaces using tangent spaces,
forms, metrics, and curvature; GMT keeps the geometric-integration machinery
while weakening smoothness into measure, rectifiability, currents, varifolds,
and almost-everywhere tangent structure.

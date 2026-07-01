# 04 — Riemannian Metrics and Curvature

**Cross-references.** `notes/NOTATION.md` (typing conventions);
`notes/02_manifolds_and_tangent_spaces.md` (defines $T_pM$, $\Gamma(TM)$
implicitly via vector fields, $df_p$); `notes/03_differential_forms_and_stokes.md`
(exterior derivative, used to contrast with $\nabla$); `00_differential_geometry_foundations_for_GMT_Topology.md`
§17–20 (survey-level metrics, connections, geodesics, curvature); feeds
`notes/09_varifolds_and_finite_perimeter.md` (first variation generalizes
curvature) and is used implicitly wherever `01`'s Fisher-information analogy
(`01` §13.7) is invoked.

---

## 0. Standing Definitions

$$
M \quad \text{smooth manifold}, \qquad \dim M = n, \qquad \Gamma(TM) \quad \text{smooth vector fields on } M.
$$

$$
g \quad \text{a Riemannian metric}: \qquad g_p : T_pM\times T_pM \to \mathbb{R} \text{ symmetric, bilinear, positive-definite}, \ \forall p\in M.
$$

$$
\nabla \quad \text{an affine connection}: \qquad \nabla : \Gamma(TM)\times\Gamma(TM) \to \Gamma(TM), \quad (X,Y)\mapsto \nabla_XY.
$$

$$
[X,Y] \quad \text{Lie bracket of vector fields}: \qquad [X,Y](f) := X(Y(f)) - Y(X(f)), \quad f\in C^\infty(M).
$$

---

## 1. Riemannian Metrics

**Definition 1.1.** A Riemannian metric on $M$ is a smooth assignment
$p\mapsto g_p$ as in §0, smoothly varying in the sense that in any chart
$g = g_{ij}\,dx^i\otimes dx^j$ with $g_{ij} = g_{ji} \in C^\infty(U)$ and
$(g_{ij}(p))$ positive-definite for every $p\in U$.

For $v = v^i\partial_i, w=w^j\partial_j \in T_pM$: $g(v,w) = g_{ij}v^iw^j$;
$\lVert v\rVert_g = \sqrt{g(v,v)}$; angle via $\cos\theta = g(v,w)/(\lVert
v\rVert_g\lVert w\rVert_g)$. $(M,g)$ is a **Riemannian manifold**.

### 1.1 Anchor construction: the musical isomorphisms

**Proposition 1.2.** For each $p$, the map

$$
\flat_p : T_pM \to T_p^*M, \qquad \flat_p(v) := g_p(v,\cdot),
$$

i.e. $\flat_p(v)(w) = g_p(v,w)$ for all $w\in T_pM$, is a linear isomorphism.

*Proof.* Linearity of $\flat_p$ is immediate from bilinearity of $g_p$. For
injectivity: if $\flat_p(v) = 0$ then $g_p(v,w)=0$ for all $w$, in particular
$w=v$, so $g_p(v,v)=0$, and positive-definiteness forces $v=0$. Since $\dim
T_pM = \dim T_p^*M = n < \infty$, injective $\implies$ bijective by
rank-nullity. $\blacksquare$

Write $\sharp_p := (\flat_p)^{-1} : T_p^*M \to T_pM$ for the inverse
("raising an index"). In coordinates, $\flat$ sends $v^i\partial_i \mapsto
g_{ij}v^i\,dx^j$, and $\sharp$ sends $\omega_i\,dx^i \mapsto g^{ij}\omega_i\,
\partial_j$, where $(g^{ij}) := (g_{ij})^{-1}$.

**Definition 1.3 (gradient, via $\sharp$).** For $f\in C^\infty(M)$, the
**gradient** is $\nabla f := \sharp(df) \in \Gamma(TM)$, i.e. the unique
vector field with $g(\nabla f, X) = df(X)$ for every $X\in\Gamma(TM)$
(existence and uniqueness are exactly Proposition 1.2 applied pointwise).

> **Type-check — primitive vs. derived, made precise.** $df \in T_p^*M$
> requires only $f \in C^\infty(M)$: no metric needed (`notes/02` §5.2).
> $\nabla f \in T_pM$ is $\sharp_p(df_p)$, and $\sharp_p$ exists *only because*
> $g_p$ is positive-definite (Proposition 1.2's proof uses
> positive-definiteness essentially, not just bilinearity — a merely
> nondegenerate symmetric form, e.g. a Lorentzian metric, still gives an
> isomorphism $\flat_p$, but a *positive-definite* one is what additionally
> guarantees $g(\nabla f,\nabla f) \geq 0$ behaves like a genuine squared
> length). Without choosing $g$, there is no canonical map $T_p^*M \to T_pM$
> at all — $df$ and $\nabla f$ are different *types* of object that happen to
> be identified once, and only once, a metric is fixed.

---

## 2. Connections and the Anchor Proof: Existence and Uniqueness of Levi-Civita

**Definition 2.1 (affine connection).** An affine connection $\nabla$ on $M$
is an $\mathbb{R}$-bilinear map $\Gamma(TM)\times\Gamma(TM)\to\Gamma(TM)$,
$(X,Y)\mapsto\nabla_XY$, satisfying:

$$
\nabla_{fX}Y = f\,\nabla_XY \qquad (\text{$C^\infty(M)$-linear in } X),
$$

$$
\nabla_X(fY) = X(f)\,Y + f\,\nabla_XY \qquad (\text{Leibniz rule in } Y),
$$

for all $f\in C^\infty(M)$, $X,Y\in\Gamma(TM)$.

> **Type-check.** $C^\infty(M)$-linearity in $X$ (unlike ordinary linearity
> in $Y$, which is only $\mathbb{R}$-linear plus Leibniz) is exactly what
> makes $\nabla_XY(p)$ depend on $X$ only through its *value* $X(p) \in
> T_pM$, while still depending on $Y$'s values in a whole neighborhood of
> $p$ (needed to differentiate it). This asymmetry is why $\nabla_XY$ is
> called "the covariant derivative of $Y$ in the direction $X$," not
> "in the direction of the vector field $X$."

In coordinates, $\nabla_{\partial_i}\partial_j = \Gamma^k_{ij}\,\partial_k$
defines the **Christoffel symbols** $\Gamma^k_{ij} \in C^\infty(U)$. These are
*not* the components of a tensor: under a change of chart they pick up an
inhomogeneous term (an explicit second-derivative correction), unlike, e.g.,
$g_{ij}$ or $R^\ell_{ijk}$ below, which transform homogeneously. Their entire
purpose is to absorb exactly that inhomogeneity so that $\nabla_XY$, the
object they help compute, *does* transform like a genuine vector field.

**Definition 2.2 (metric compatibility, torsion-freeness).** $\nabla$ is
**compatible with $g$** if $X(g(Y,Z)) = g(\nabla_XY,Z)+g(Y,\nabla_XZ)$ for all
$X,Y,Z \in \Gamma(TM)$. $\nabla$ is **torsion-free** if $\nabla_XY-\nabla_YX =
[X,Y]$ for all $X,Y$.

### Anchor Theorem: Levi-Civita Connection

**Theorem 2.3 (Fundamental theorem of Riemannian geometry).** Given $(M,g)$,
there is exactly one affine connection $\nabla$ on $M$ that is both
metric-compatible and torsion-free.

*Proof (existence and uniqueness via the Koszul formula).* Suppose $\nabla$
satisfies both conditions. Write out metric compatibility three times, cyclically
permuting $X,Y,Z$:

$$
X\,g(Y,Z) = g(\nabla_XY,Z) + g(Y,\nabla_XZ), \tag{a}
$$
$$
Y\,g(Z,X) = g(\nabla_YZ,X) + g(Z,\nabla_YX), \tag{b}
$$
$$
Z\,g(X,Y) = g(\nabla_ZX,Y) + g(X,\nabla_ZY). \tag{c}
$$

Compute (a) $+$ (b) $-$ (c):

$$
X g(Y,Z) + Y g(Z,X) - Z g(X,Y) = g(\nabla_XY+\nabla_YX,Z) + g(Y,\nabla_XZ-\nabla_ZX) + g(X,\nabla_YZ-\nabla_ZY).
$$

Now substitute torsion-freeness, $\nabla_XY - \nabla_YX = [X,Y]$, rewritten
as $\nabla_XY = \nabla_YX + [X,Y]$ wherever useful, into each of the three
bracketed differences: $\nabla_XY+\nabla_YX = 2\nabla_XY - [X,Y]$;
$\nabla_XZ-\nabla_ZX = [X,Z]$; $\nabla_YZ-\nabla_ZY=[Y,Z]$. Substituting,

$$
X g(Y,Z) + Y g(Z,X) - Z g(X,Y) = 2g(\nabla_XY,Z) - g([X,Y],Z) + g(Y,[X,Z]) + g(X,[Y,Z]).
$$

Solving for $g(\nabla_XY,Z)$ gives the **Koszul formula**:

$$
2\,g(\nabla_XY,Z) = X g(Y,Z) + Yg(Z,X) - Zg(X,Y) + g([X,Y],Z) - g([X,Z],Y) - g([Y,Z],X).
$$

*Uniqueness:* the right-hand side involves only $g$ and the Lie bracket
(itself determined by $M$'s smooth structure alone, `notes/02` §7), not
$\nabla$ — so if a metric-compatible, torsion-free $\nabla$ exists, the value
$g(\nabla_XY,Z)$ is *forced* for every $Z$. Since $g_p$ is
positive-definite (hence $Z\mapsto g(\cdot,Z)$ is, pointwise, the isomorphism
$\flat_p$ of Proposition 1.2), knowing $g(\nabla_XY,Z)$ for all $Z$
determines $\nabla_XY$ uniquely: $\nabla_XY = \sharp\big(\tfrac12[X g(Y,\cdot)
+ \cdots]\big)$, i.e. $\nabla_XY$ is the unique vector field whose
$\flat$-image is the right-hand side (with $Z$ treated as the free slot).

*Existence:* define $\nabla_XY$ by declaring $g(\nabla_XY,Z)$ to equal the
right-hand side of the Koszul formula for every $Z$ (possible since the
right-hand side, for fixed $X,Y$, is $C^\infty(M)$-linear in $Z$ — direct
check — hence by Proposition 1.2 pointwise it does define some $g(\cdot,Z)$
uniquely realized by a vector field). One then checks directly, term by term
in the Koszul formula, that this $\nabla$ is $\mathbb{R}$-bilinear,
$C^\infty(M)$-linear in $X$, Leibniz in $Y$, metric-compatible, and
torsion-free — each a direct (if lengthy) computation substituting the
Koszul formula back into Definition 2.1/2.2's defining identities and using
the derivation and Leibniz properties of the Lie bracket. $\blacksquare$

*(Lee, IRM, Thm 5.10, the standard reference for exactly this
Koszul-formula proof; do Carmo, RG, Ch. 2, Thm 3.6, an equivalent
coordinate-based existence argument via the Christoffel-symbol formula
$\Gamma^k_{ij} = \tfrac12 g^{k\ell}(\partial_ig_{j\ell}+\partial_jg_{i\ell}-
\partial_\ell g_{ij})$, which is the Koszul formula specialized to coordinate
vector fields, where $[\partial_i,\partial_j]=0$.)*

> **What this buys you.** Every geometric quantity built from "the"
> connection on a Riemannian manifold — geodesics (§3), curvature (§4),
> Laplace–Beltrami (`00` §21) — is unambiguous, because Theorem 2.3 says
> there is exactly one reasonable choice, singled out by two conditions that
> are each independently natural (compatibility says $\nabla$ respects
> lengths/angles; torsion-freeness says $\nabla$ has no built-in "twist").

---

## 3. Geodesics

**Definition 3.1.** $\gamma : I \to M$ is a **geodesic** if
$\nabla_{\dot\gamma}\dot\gamma = 0$, i.e. its velocity field is
parallel along itself. In coordinates:

$$
\frac{d^2x^k}{dt^2} + \Gamma^k_{ij}\frac{dx^i}{dt}\frac{dx^j}{dt} = 0.
$$

This is a second-order ODE system; existence and uniqueness of geodesics
with given initial position and velocity follows from the Picard–Lindelöf
theorem (`01` §10, proof anchor) applied to the equivalent first-order system
on $TM$.

| Space | Geodesics |
|---|---|
| $\mathbb{R}^n$, Euclidean $g$ | straight lines ($\Gamma^k_{ij}\equiv 0$) |
| $S^2$, round metric | great circles |
| cylinder, flat metric | helices / straight unwraps |

---

## 4. Curvature

**Definition 4.1 (Riemann curvature endomorphism).**

$$
R(X,Y)Z := \nabla_X\nabla_YZ - \nabla_Y\nabla_XZ - \nabla_{[X,Y]}Z, \qquad X,Y,Z \in \Gamma(TM), \quad R(X,Y)Z \in \Gamma(TM).
$$

**Proposition 4.2.** $R(X,Y)Z$ is $C^\infty(M)$-linear in each of $X,Y,Z$
separately (in particular, its value at $p$ depends only on $X(p),Y(p),Z(p)$,
not on their behavior nearby) — so $R$ is a genuine $(1,3)$-tensor field,
unlike $\Gamma^k_{ij}$.

*(Direct verification using the connection axioms of Definition 2.1 and the
Jacobi identity for $[\cdot,\cdot]$; standard, e.g. Lee, IRM, Prop. 7.4.)*

**Definition 4.3 (sectional curvature).** For a 2-plane $\Pi = \operatorname{span}(X,Y)
\subseteq T_pM$,

$$
K(\Pi) := \frac{g(R(X,Y)Y,X)}{g(X,X)g(Y,Y) - g(X,Y)^2},
$$

independent of the basis $(X,Y)$ chosen for $\Pi$ (standard check). For a
surface ($n=2$), $K(\Pi) = K$, the classical Gaussian curvature — this is
**Gauss's Theorema Egregium**: $K$, originally defined extrinsically via the
second fundamental form, equals an intrinsic quantity built purely from $g$
and its derivatives (do Carmo, DGCS, §4-3), so it is preserved by
isometries even if they are not restrictions of ambient rigid motions.

| Surface | $K$ |
|---|---:|
| plane | $0$ |
| cylinder | $0$ |
| sphere, radius $R$ | $1/R^2$ |
| saddle | negative |

> **Type-check.** The cylinder having $K=0$ despite visibly "curving" in
> $\mathbb{R}^3$ is exactly the extrinsic/intrinsic type distinction: $K$
> depends only on $g$ (intrinsic), not on how $M$ sits inside an ambient
> space (extrinsic) — a flat sheet of paper can be rolled into a cylinder
> without stretching, i.e. without changing $g$, hence without changing $K$.

Conceptually: $R(X,Y)Z$ measures the *noncommutativity* of $\nabla_X\nabla_Y$
vs. $\nabla_Y\nabla_X$ (corrected by the bracket term), which is the same
thing as measuring path-dependence of parallel transport around a small loop
spanned by $X,Y$.

---

## References Used

- **Lee, IRM** — Ch. 4 (connections, Christoffel symbols), Ch. 5 (Thm 5.10,
  the Koszul-formula existence/uniqueness proof reproduced in full above),
  Ch. 7–8 (curvature, Prop. 7.4, sectional curvature).
- **do Carmo, RG** — Ch. 2 (Thm 3.6, coordinate-based Levi-Civita
  construction, cross-checked against the Koszul formula above), Ch. 4
  (curvature tensor).
- **do Carmo, DGCS** — §3-3–3-4 (second fundamental form, Gaussian
  curvature), §4-3 (Theorema Egregium, cited for the $K$-is-intrinsic fact
  used in §4).

Full bibliographic data: `notes/NOTATION.md` §4.

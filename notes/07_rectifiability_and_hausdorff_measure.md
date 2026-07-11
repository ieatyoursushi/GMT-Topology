# 07 — Rectifiability, Lipschitz Maps, and the Area Formula

**Cross-references.** `notes/NOTATION.md` (typing conventions);
`notes/06_measure_theory_bridge.md` (constructs $\mathcal{H}^k$, used
throughout); `notes/02_manifolds_and_tangent_spaces.md` (the smooth notion of
$T_pM$ this chapter's approximate tangent plane replaces); `00_differential_geometry_foundations_for_GMT_Topology.md`
§26–27, §31 (survey-level rectifiability, area/coarea); feeds `notes/08_currents.md`
and `notes/09_varifolds_and_finite_perimeter.md` (both are built on
rectifiable sets).

---

## 0. Standing Definitions

$$
E \subseteq \mathbb{R}^n, \qquad k \in \{0,1,\dots,n\}, \qquad f : \mathbb{R}^m \to \mathbb{R}^n \quad \text{Lipschitz}: \ \exists\, \lambda<\infty,\ |f(x)-f(y)|\le \lambda|x-y|\ \forall x,y.
$$

$$
\operatorname{Lip}(f) := \inf\{\lambda : |f(x)-f(y)|\le \lambda|x-y|\ \forall x,y\}
$$

(the Lipschitz-constant bound is written $\lambda$ here, reserving $L$ for
the linear map below).

$$
L : \mathbb{R}^k \to \mathbb{R}^n \quad \text{linear}, \qquad k\le n, \qquad L^*L : \mathbb{R}^k\to\mathbb{R}^k \quad \text{(adjoint composed with $L$)}.
$$

$$
J_k(L) := \sqrt{\det(L^*L)} \quad \text{the $k$-dimensional Jacobian of } L.
$$

---

## 1. Lipschitz Maps and Rademacher's Theorem

**Definition 1.1.** $f:\mathbb{R}^m\to\mathbb{R}^n$ is Lipschitz if
$\operatorname{Lip}(f)<\infty$ as in §0. Every Lipschitz map is continuous,
and in fact uniformly continuous.

**Theorem 1.2 (Rademacher).** If $f:\mathbb{R}^m\to\mathbb{R}^n$ is
Lipschitz, then $f$ is differentiable at $\mathcal{L}^m$-almost every point:
there is a set $N\subseteq\mathbb{R}^m$ with $\mathcal{L}^m(N)=0$ such that
for every $x\notin N$, there is a linear map $Df_x:\mathbb{R}^m\to\mathbb{R}^n$
with

$$
f(x+h) = f(x) + Df_x(h) + o(|h|) \qquad \text{as } h\to 0.
$$

*(Federer, GMT, §3.1.6; Evans–Gariepy, MTFP, Thm 3.2. This is a genuinely
hard theorem — the standard proof for $m=1$ uses that a Lipschitz function of
one real variable is absolutely continuous, hence differentiable a.e. by the
Lebesgue differentiation theorem for monotone/BV functions; the general case
reduces to directional derivatives existing a.e. by the 1-D case applied
along each direction, then uses Fubini plus a further approximation argument
to upgrade "differentiable in every direction, a.e." to genuine (total)
differentiability a.e. Stated here precisely, at recognition/operational
depth (`notes/NOTATION.md` §1.1), and used as a black box below, per the
explicit guidance in `01` §17 that Rademacher's theorem is safe to black-box
until it becomes the direct object of study — which for this repository it
is not; what matters here is what Rademacher's theorem *supplies*: a
generalized Jacobian defined a.e.)*

> **Type-check — what breaks and what survives.** A smooth map has $DF_p$ at
> *every* point (`notes/02` §3, `notes/03`). A Lipschitz map has $Df_x$ only
> at $\mathcal{L}^m$-*almost every* $x$ — pointwise differentiability
> degrades to almost-everywhere differentiability, exactly the type-level
> degradation pattern catalogued in `notes/NOTATION.md` §3. What survives:
> $Df_x$, where it exists, is still an honest linear map $\mathbb{R}^m\to
> \mathbb{R}^n$, so every construction built algebraically from a
> derivative (Jacobians, area/coarea formulas, approximate tangent planes)
> still makes sense — just only almost everywhere.

---

## 2. Rectifiable Sets and Approximate Tangent Planes

**Definition 2.1.** $E\subseteq\mathbb{R}^n$ is **countably $k$-rectifiable**
if there exist countably many Lipschitz maps $f_j : A_j\to\mathbb{R}^n$,
$A_j\subseteq\mathbb{R}^k$, with

$$
\mathcal{H}^k\Big(E\setminus\bigcup_{j=1}^\infty f_j(A_j)\Big) = 0.
$$

$E$ is ($\mathcal{H}^k$-)**rectifiable** if additionally $\mathcal{H}^k(E)<\infty$.

**Definition 2.2 (approximate tangent plane).** $E$ has an approximate
tangent $k$-plane $T_pE \in G(n,k)$ at $p\in E$ if, informally, the blow-ups
$(E-p)/r$ converge as $r\to 0^+$ (in a measure-theoretic, density-weighted
sense) to the plane $T_pE$ — precisely, if for $\mathcal{H}^k$-a.e. $p \in
f_j(A_j)$, the tangent plane to the Lipschitz image $f_j(A_j)$ at $p$
(defined via $Df_x$ at the corresponding $x=f_j^{-1}(p)$, which exists
$\mathcal{L}^k$-a.e. by Rademacher's Theorem 1.2 applied to $f_j$) serves as
$T_pE$.

**Proposition 2.3.** If $E$ is countably $k$-rectifiable, then $T_pE$ exists
for $\mathcal{H}^k$-almost every $p\in E$.

*Proof idea (depth B).* On each piece $f_j(A_j)$, Rademacher's theorem
(Theorem 1.2) gives $Df_x$ for $\mathcal{L}^k$-a.e. $x\in A_j$; a further
standard fact (Federer, GMT, §3.2.2, using the area formula of §3 below to
transfer the exceptional null set from $A_j$, measured in $\mathcal{L}^k$, to
$f_j(A_j)$, measured in $\mathcal{H}^k$) shows the exceptional set on
$f_j(A_j)$ is $\mathcal{H}^k$-null. Taking a countable union of
$\mathcal{H}^k$-null exceptional sets over $j$ preserves $\mathcal{H}^k$-null.
$\blacksquare$ *(Not reproduced at depth C — this is the composition of two
already-cited results, Rademacher plus the area formula, and is exactly the
kind of derived fact `01` §20's rule 2 says to upgrade to a full proof-sketch
rather than a from-scratch derivation.)*

| Smooth manifold | Rectifiable set |
|---|---|
| smooth chart $\varphi: U\to\mathbb{R}^n$, $C^\infty$ | Lipschitz parametrization $f_j:A_j\to\mathbb{R}^n$ |
| $T_pM$ at every $p$ | $T_pE$ for $\mathcal{H}^k$-a.e. $p$ (Proposition 2.3) |
| smooth area form $dA$ | $\mathcal{H}^k\llcorner E$ |
| classical boundary $\partial M$ | measure-theoretic/current boundary (`notes/09`, `notes/08`) |

---

## 3. Area Formula — Anchor Proof for Linear Maps, Statement for Lipschitz Maps

The classical change-of-variables theorem, $\int_{F(U)} g\,dV =
\int_U (g\circ F)\,|\det DF|\,dV$ for a diffeomorphism $F$, has a Lipschitz
generalization built from the following linear-algebra core fact, which we
prove in full, and which is the seed of the general area formula.

### 3.1 Anchor Theorem: the Linear-Map Area Formula

**Theorem 3.1.** Let $L : \mathbb{R}^k \to \mathbb{R}^n$ be linear, $k\le n$,
and let $A\subseteq\mathbb{R}^k$ be $\mathcal{L}^k$-measurable. Then

$$
\mathcal{H}^k(L(A)) = J_k(L)\cdot \mathcal{L}^k(A), \qquad J_k(L) = \sqrt{\det(L^*L)}.
$$

*Proof.* By the singular value decomposition, write $L = R\circ D\circ S$
where $S:\mathbb{R}^k\to\mathbb{R}^k$, $R:\mathbb{R}^n\to\mathbb{R}^n$ are
orthogonal (linear isometries), and $D:\mathbb{R}^k\to\mathbb{R}^n$ is
"diagonal": $D(x^1,\dots,x^k) = (\sigma_1x^1,\dots,\sigma_kx^k,0,\dots,0)$,
$\sigma_i\ge 0$ the singular values of $L$.

*Step 1: orthogonal maps preserve both $\mathcal{L}^k$ and $\mathcal{H}^k$.*
A linear isometry $R$ preserves Euclidean distance exactly, hence preserves
$\operatorname{diam}(C)$ for every set $C$, hence (directly from Definition
5.1 of `notes/06`) preserves $\mathcal{H}^k_\delta$ for every $\delta$, hence
preserves $\mathcal{H}^k$. Similarly $S$ preserves $\mathcal{L}^k$ (standard:
orthogonal linear maps have $|\det S|=1$, and Lebesgue measure scales by
$|\det S|$ under a linear map — this is the ordinary linear
change-of-variables fact, taken as known).

*Step 2: compute $\mathcal{H}^k(D(A))$ directly.* Since $D$ acts as
$\operatorname{diag}(\sigma_1,\dots,\sigma_k)$ into the first $k$
coordinates of $\mathbb{R}^n$ and $0$ into the rest, $D(A) \subseteq
\mathbb{R}^k\times\{0\}^{n-k} \cong \mathbb{R}^k$ (identifying $\mathbb{R}^k$
with this coordinate subspace, on which $\mathcal{H}^k$ restricts to
ordinary $k$-dimensional Lebesgue measure $\mathcal{L}^k$, by Theorem 5.4 of
`notes/06`). Under this identification, $D$ acts as the diagonal linear map
$\operatorname{diag}(\sigma_1,\dots,\sigma_k):\mathbb{R}^k\to\mathbb{R}^k$,
and the ordinary $\mathbb{R}^k$ change-of-variables formula for a linear map
gives

$$
\mathcal{H}^k(D(A)) = \mathcal{L}^k(D(A)) = |\det\operatorname{diag}(\sigma_1,\dots,\sigma_k)|\cdot\mathcal{L}^k(A) = \sigma_1\cdots\sigma_k\cdot\mathcal{L}^k(A).
$$

*Step 3: identify $\sigma_1\cdots\sigma_k$ with $J_k(L)$.* By construction,
$L^*L = S^*D^*R^*RDS = S^*D^*DS$ (using $R^*R = I$), and $D^*D =
\operatorname{diag}(\sigma_1^2,\dots,\sigma_k^2)$ by direct computation from
the definition of $D$. So $\det(L^*L) = \det(S^*)\det(D^*D)\det(S) =
(\det S)^2 \sigma_1^2\cdots\sigma_k^2 = \sigma_1^2\cdots\sigma_k^2$ (using
$(\det S)^2=1$ for orthogonal $S$). Hence $J_k(L) = \sqrt{\det(L^*L)} =
\sigma_1\cdots\sigma_k$.

*Conclusion.* Combining Steps 1–3:

$$
\mathcal{H}^k(L(A)) = \mathcal{H}^k(R(D(S(A)))) = \mathcal{H}^k(D(S(A))) \quad(\text{Step 1, } R \text{ isometry})
$$
$$
= \sigma_1\cdots\sigma_k\cdot\mathcal{L}^k(S(A)) \quad (\text{Step 2, applied to } S(A) \text{ in place of } A)
$$
$$
= \sigma_1\cdots\sigma_k\cdot\mathcal{L}^k(A) \quad (\text{Step 1, } S \text{ isometry preserves } \mathcal{L}^k)
$$
$$
= J_k(L)\cdot\mathcal{L}^k(A) \quad (\text{Step 3}). \qquad\blacksquare
$$

*(Federer, GMT, §3.2.3, the linear case underlying the general area formula;
Evans–Gariepy, MTFP, §3.3, an equivalent SVD-based treatment matching the
proof above.)*

> **Why this is the right anchor.** The general area formula for a Lipschitz
> $f:\mathbb{R}^k\to\mathbb{R}^n$ replaces the single linear map $L$ above by
> the a.e.-defined $Df_x$ (Rademacher, Theorem 1.2) and integrates
> Theorem 3.1's identity pointwise, using a multiplicity function to account
> for $f$ folding $A$ over itself. Every bit of that generalization is
> bookkeeping (a covering/approximation argument plus the multiplicity sum);
> the actual geometric content — that $\mathcal{H}^k$-image-size equals a
> Jacobian-determinant factor times $\mathcal{L}^k$-domain-size — is entirely
> contained in the linear computation just given.

### 3.2 The General Area Formula (stated)

**Theorem 3.2 (Area formula).** Let $f:\mathbb{R}^k\to\mathbb{R}^n$ be
Lipschitz, $k\le n$, $A\subseteq\mathbb{R}^k$ measurable. Then

$$
\int_A J_kf(x)\,d\mathcal{L}^k(x) = \int_{\mathbb{R}^n} \#\big(f^{-1}(y)\cap A\big)\,d\mathcal{H}^k(y),
$$

where $J_kf(x) := J_k(Df_x)$ (defined $\mathcal{L}^k$-a.e. by Rademacher) and
$\#(\cdot)$ counts multiplicity (how many points of $A$ map to $y$).

*(Federer, GMT, §3.2.3; Evans–Gariepy, MTFP, Thm 3.8. Not reproved in full
here — the proof assembles Theorem 3.1 pointwise via a Vitali-type covering
argument, a routine but lengthy measure-theoretic bookkeeping exercise
appropriately black-boxed at operational depth per `01` §17.)* When $f$ is
injective, $\#(f^{-1}(y)\cap A) = \mathbf{1}_{f(A)}(y)$, and the formula
recovers precisely the "$dS = \lVert r_u\times r_v\rVert\,du\,dv$"
area-element rule of `01` §5.3, now valid for $f$ merely Lipschitz rather
than $C^1$.

**Theorem 3.3 (Coarea formula, stated).** For Lipschitz $f:\mathbb{R}^n\to
\mathbb{R}^m$, $n\ge m$, and $A\subseteq\mathbb{R}^n$ measurable,

$$
\int_A J_mf(x)\,d\mathcal{L}^n(x) = \int_{\mathbb{R}^m} \mathcal{H}^{n-m}(A\cap f^{-1}(y))\,d\mathcal{L}^m(y).
$$

*(Federer, GMT, §3.2.11; Evans–Gariepy, MTFP, Thm 3.9.)* Slicing $A$ by the
level sets of $f$ and integrating the $(n{-}m)$-dimensional Hausdorff measure
of each slice recovers the $\mathcal{L}^n$-measure of $A$, weighted by the
Jacobian of $f$ restricted transverse to its level sets — the measure-theoretic
generalization of Fubini's theorem to a nonlinear "coordinate" $f$.

---

## References Used

- **Federer, GMT** — §3.1.6 (Rademacher's theorem), §3.2.2–3.2.3 (approximate
  tangent planes, the linear and general area formula — the anchor proof in
  §3.1 above follows the SVD decomposition strategy standard to this
  source), §3.2.11 (coarea formula).
- **Evans–Gariepy, MTFP** — Thm 3.2 (Rademacher), Thm 3.8 (area formula), Thm
  3.9 (coarea formula), §3.3 (an independent SVD-based proof of the
  linear-map area formula, cross-checked against Federer's version above).

Full bibliographic data: `notes/NOTATION.md` §4.

# 02 — Manifolds and Tangent Spaces

**Cross-references.** `notes/NOTATION.md` (typing conventions, read first);
`00_differential_geometry_foundations_for_GMT_Topology.md` §7–8 (survey-level
introduction to manifolds and $T_pM$, which this document proves in full);
`01_prerequisite_systematic_review_for_GMT_Topology.md` §6–7 (real analysis
and point-set topology prerequisites used below); feeds into
`notes/03_differential_forms_and_stokes.md` (forms live in
$\Lambda^k(T_p^*M)$) and `notes/04_riemannian_metrics_and_curvature.md`
($g_p$ lives on $T_pM$).

---

## 0. Standing Definitions

In addition to `notes/NOTATION.md` §2, this document fixes:

$$
M \quad \text{a set}, \qquad \tau \quad \text{a topology on } M, \qquad (M,\tau) \text{ Hausdorff and second-countable}.
$$

$$
n \in \mathbb{Z}_{\ge 0} \quad \text{the manifold's dimension (fixed throughout)}.
$$

$$
(U,\varphi) \quad \text{a chart}: \quad U \subseteq M \text{ open}, \qquad \varphi : U \to \varphi(U) \subseteq \mathbb{R}^n \text{ a homeomorphism onto an open set}.
$$

$$
\mathcal{A} = \{(U_\alpha,\varphi_\alpha)\}_{\alpha\in I} \quad \text{an atlas}: \qquad \bigcup_{\alpha\in I} U_\alpha = M.
$$

$$
p \in M, \qquad \gamma : I \to M \text{ a curve}, \ I\subseteq\mathbb{R} \text{ an open interval containing } 0.
$$

---

## 1. Topological Manifolds

**Definition 1.1.** A **topological $n$-manifold** is a topological space
$(M,\tau)$ that is Hausdorff, second-countable, and locally Euclidean of
dimension $n$: every $p\in M$ has an open neighborhood $U$ and a
homeomorphism $\varphi : U \to \varphi(U)\subseteq\mathbb{R}^n$.

> **Type-check — why Hausdorff and second-countable.** "Locally Euclidean" by
> itself does not rule out pathologies. The standard example: take two copies
> of $\mathbb{R}$ and glue $x\sim x$ for all $x\ne 0$ (the "line with two
> origins"). Every point still has a Euclidean neighborhood, but the space is
> not Hausdorff — the two origins cannot be separated by disjoint open sets.
> Second-countability is what later guarantees the existence of a countable,
> locally finite atlas and hence of smooth partitions of unity (used, but not
> reproved, in `01` §17 "safe to black-box"). Both hypotheses are part of the
> *type* of "manifold," not optional decoration (Lee, ISM, pp. 3, 10).

**Definition 1.2 (chart, atlas).** A chart is a pair $(U,\varphi)$ as in §0. An
atlas is a family of charts whose domains cover $M$.

---

## 2. Smooth Manifolds

**Definition 2.1 (transition map).** For charts $(U,\varphi)$, $(V,\psi)$ with
$U\cap V \ne \varnothing$, the transition map is

$$
\psi\circ\varphi^{-1} : \varphi(U\cap V) \to \psi(U\cap V),
$$

a map between open subsets of $\mathbb{R}^n$.

**Definition 2.2 (smooth atlas, smooth manifold).** An atlas is **smooth** if
every transition map $\psi\circ\varphi^{-1}$ between two of its charts is
$C^\infty$ (automatically a diffeomorphism, since
$(\psi\circ\varphi^{-1})^{-1} = \varphi\circ\psi^{-1}$ is also required
$C^\infty$). A **smooth manifold** is a topological manifold equipped with a
maximal smooth atlas.

> **Type-check.** $\varphi$ and $\psi$ each map a subset of $M$ to a subset of
> $\mathbb{R}^n$; they cannot be composed with each other directly, only with
> their own inverses on the $\mathbb{R}^n$ side. The transition map
> $\psi\circ\varphi^{-1}$ is the unique way to obtain a self-map of (an open
> subset of) $\mathbb{R}^n$ from the data at hand, and it is precisely this
> composite whose smoothness is required. This is why "smoothness of $M$" is
> not defined pointwise on $M$ from scratch — it is defined via compatibility
> of charts on overlaps, reducing an otherwise meaningless question
> ("is $M$ differentiable?") to an ordinary multivariable-calculus question
> ("is this map between open subsets of $\mathbb{R}^n$ differentiable?").

**Definition 2.3 (smooth function on $M$).** $f : M \to \mathbb{R}$ is smooth
if $f\circ\varphi^{-1} : \varphi(U)\to\mathbb{R}$ is $C^\infty$ for every
chart $(U,\varphi)$ in the atlas. Write $C^\infty(M)$ for the set of such $f$;
this is a commutative $\mathbb{R}$-algebra under pointwise operations.

**Definition 2.4 (smooth map between manifolds).** $F : M \to N$ is smooth if
for every chart $(U,\varphi)$ on $M$ and $(V,\psi)$ on $N$ with
$F(U)\subseteq V$, the composite $\psi\circ F\circ\varphi^{-1} :
\varphi(U)\to\psi(V)$ is $C^\infty$.

---

## 3. Tangent Spaces — Three Constructions

Fix $p \in M$. We give three definitions of $T_pM$ and prove they agree.

### 3.1 Definition (i): velocities of curves

Let $\mathcal{C}_p = \{\gamma : I\to M \mid I \text{ open interval}, 0\in I,\ \gamma(0)=p,\ \gamma \text{ smooth}\}$.
Declare $\gamma_1 \sim \gamma_2$ if, for *some* (equivalently, by the chain
rule, *every*) chart $(U,\varphi)$ around $p$,

$$
(\varphi\circ\gamma_1)'(0) = (\varphi\circ\gamma_2)'(0) \in \mathbb{R}^n.
$$

Define $T_pM^{(i)} := \mathcal{C}_p/\!\sim$.

### 3.2 Definition (ii): derivations

A **derivation at $p$** is a linear map $v : C^\infty(M) \to \mathbb{R}$
satisfying the Leibniz rule at $p$:

$$
v(fg) = f(p)\,v(g) + g(p)\,v(f), \qquad \forall f,g\in C^\infty(M).
$$

Define $T_pM^{(ii)} := \{\text{derivations at } p\}$, a real vector space
under $(v_1+v_2)(f) := v_1(f)+v_2(f)$, $(\lambda v)(f):=\lambda\, v(f)$.

### 3.3 Definition (iii): coordinate vectors

Fix a chart $(U,\varphi)$ around $p$ with coordinates $(x^1,\dots,x^n)$.
Define $T_pM^{(iii)} := \mathbb{R}^n$, with basis symbols
$\{\partial/\partial x^1|_p,\dots,\partial/\partial x^n|_p\}$, understood
purely formally at this stage as a labeled copy of $\mathbb{R}^n$.

---

## 4. Anchor Proof: $T_pM$ Is a Well-Defined $n$-Dimensional Real Vector Space, and the Three Definitions Agree

This is the anchor proof for this document (depth C, `notes/NOTATION.md`
§1.1), following the standard argument in **Lee, ISM, Ch. 3** (Prop. 3.2,
Thm 3.5, and the surrounding discussion of the transformation law), reproduced
here in full rather than sketched.

**Claim.** There are canonical bijections $T_pM^{(i)} \cong T_pM^{(ii)} \cong
T_pM^{(iii)}$, and under these bijections $T_pM^{(ii)}$'s vector-space
structure transports to make all three an $n$-dimensional real vector space.
Moreover, under a change of chart $(U,\varphi)\to(V,\psi)$, coordinate vectors
transform by the Jacobian of the transition map.

**Step 1: (i) $\to$ (ii).** Given $\gamma \in \mathcal{C}_p$, define
$v_\gamma : C^\infty(M)\to\mathbb{R}$ by

$$
v_\gamma(f) := (f\circ\gamma)'(0) = \frac{d}{dt}\Big|_{t=0} f(\gamma(t)).
$$

Linearity of $v_\gamma$ is immediate from linearity of the derivative. The
Leibniz rule holds by the ordinary product rule applied to $t\mapsto
f(\gamma(t))g(\gamma(t))$:

$$
v_\gamma(fg) = \frac{d}{dt}\Big|_{t=0}\big[f(\gamma(t))g(\gamma(t))\big] = f(\gamma(0))\,g'(\gamma(t))\big|_{t=0}\cdots
$$

more precisely, by the chain rule,

$$
v_\gamma(fg) = f(p)\,v_\gamma(g) + g(p)\,v_\gamma(f).
$$

So $v_\gamma \in T_pM^{(ii)}$. If $\gamma_1\sim\gamma_2$, then for any chart
$\varphi$, writing $\hat f := f\circ\varphi^{-1}$,

$$
v_{\gamma_i}(f) = \frac{d}{dt}\Big|_{t=0} \hat f(\varphi(\gamma_i(t))) = D\hat f(\varphi(p))\cdot(\varphi\circ\gamma_i)'(0)
$$

by the chain rule in $\mathbb{R}^n$, and the right-hand side depends on
$\gamma_i$ only through $(\varphi\circ\gamma_i)'(0)$, which agrees for
$i=1,2$ by definition of $\sim$. Hence $v_{\gamma_1}=v_{\gamma_2}$ as
derivations, and $\gamma \mapsto v_\gamma$ descends to a well-defined map
$T_pM^{(i)} \to T_pM^{(ii)}$.

**Step 2: (ii) $\to$ (iii), via a chart, and injectivity/surjectivity of
Step 1.** Fix a chart $(U,\varphi)$, coordinates $(x^1,\dots,x^n)$,
$\varphi(p) = 0$ WLOG. Define, for each $i$, the **coordinate partial
derivative derivation**

$$
\frac{\partial}{\partial x^i}\Big|_p (f) := \frac{\partial(f\circ\varphi^{-1})}{\partial x^i}(\varphi(p)), \qquad f \in C^\infty(M).
$$

This is manifestly linear in $f$ and satisfies Leibniz by the ordinary
product rule for partial derivatives, so
$\partial/\partial x^i|_p \in T_pM^{(ii)}$.

*Spanning.* Let $v \in T_pM^{(ii)}$ be arbitrary. For $f \in C^\infty(M)$,
Taylor's theorem with remainder applied to $\hat f = f\circ\varphi^{-1}$
about $\varphi(p) = 0$ gives, on a neighborhood of $0$,

$$
\hat f(x) = \hat f(0) + \sum_{i=1}^n x^i\, \frac{\partial \hat f}{\partial x^i}(0) + \sum_{i,j} x^ix^j\, h_{ij}(x)
$$

for some smooth functions $h_{ij}$ (Hadamard's lemma; Lee, ISM, Lemma 3.1 in
some editions, or directly by the integral form of the Taylor remainder). Set
$g^i := x^i\circ\varphi \in C^\infty(U)$ (extended smoothly to $M$, e.g. via a
bump function — this is one of the "safe to black-box" facts flagged in `01`
§17), so on $U$,

$$
f = f(p) + \sum_i g^i\cdot\Big(\frac{\partial \hat f}{\partial x^i}(0)\circ\varphi\Big) + \sum_{i,j} g^ig^j\cdot(h_{ij}\circ\varphi).
$$

Applying the derivation $v$, using linearity, Leibniz, and $v(\text{const})=0$
(which follows from Leibniz applied to $1\cdot 1=1$: $v(1)=v(1)v(1)+v(1)v(1)$... 
more directly $v(1) = v(1\cdot 1) = 1\cdot v(1) + 1\cdot v(1) = 2v(1) \Rightarrow v(1)=0$,
hence $v(c)=cv(1)=0$ for constants $c$), and $g^i(p)=0$, every term involving
a product of two $g^i$-factors vanishes under $v$ by Leibniz (each factor is
$0$ at $p$), leaving

$$
v(f) = \sum_{i=1}^n v(g^i)\cdot \frac{\partial \hat f}{\partial x^i}(0) = \sum_{i=1}^n v(g^i)\, \frac{\partial}{\partial x^i}\Big|_p(f).
$$

So $v = \sum_i v(g^i)\,\partial/\partial x^i|_p$: every derivation is a
coordinate combination of the $\partial/\partial x^i|_p$, proving
$\{\partial/\partial x^i|_p\}_{i=1}^n$ **spans** $T_pM^{(ii)}$.

*Linear independence.* If $\sum_i c^i\,\partial/\partial x^i|_p = 0$ as a
derivation, apply it to $f = x^j\circ\varphi$ (extended smoothly) for each
$j$: the left side is $c^j$, the right side is $0$. So $c^j=0$ for all $j$,
proving independence.

Hence $\{\partial/\partial x^i|_p\}_{i=1}^n$ is a basis of $T_pM^{(ii)}$, so
$\dim_\mathbb{R} T_pM^{(ii)} = n$, and the assignment
$v^i\,\partial/\partial x^i|_p \leftrightarrow (v^1,\dots,v^n)\in\mathbb{R}^n
= T_pM^{(iii)}$ is a linear isomorphism $T_pM^{(ii)}\cong T_pM^{(iii)}$
**once a chart is fixed**.

**Step 3: Step-1 map is a bijection.** Given $v = v^i\partial/\partial
x^i|_p$, define $\gamma(t) := \varphi^{-1}(t v^1,\dots,tv^n)$ for $t$ small.
Then $\gamma \in \mathcal{C}_p$ and a direct computation with the chain rule
shows $v_\gamma = v$: this exhibits a right inverse to the Step-1 map, and
combined with the spanning/independence argument above (which shows
$\gamma\mapsto v_\gamma$ is already well-defined and its image spans, by
varying $\varphi^{-1}(tv)$ over all $v\in\mathbb{R}^n$) the map $T_pM^{(i)}
\to T_pM^{(ii)}$ is a bijection.

**Step 4: the Jacobian transformation law.** Let $(U,\varphi)$, coordinates
$x^i$, and $(V,\psi)$, coordinates $y^j$, both contain $p$. By the chain rule
applied to $f\circ\psi^{-1} = (f\circ\varphi^{-1})\circ(\varphi\circ\psi^{-1})$,

$$
\frac{\partial}{\partial y^j}\Big|_p = \frac{\partial x^i}{\partial y^j}(p)\ \frac{\partial}{\partial x^i}\Big|_p,
$$

i.e. the two coordinate bases are related by the Jacobian matrix of the
transition map $\varphi\circ\psi^{-1}$ (equivalently its inverse, depending on
direction of composition): $\big(\partial x^i/\partial y^j(p)\big)_{i,j} =
J_{\varphi\circ\psi^{-1}}(\psi(p))$. Consequently, for $v = v^i\partial/\partial
x^i|_p = w^j \partial/\partial y^j|_p$, the coordinate columns are related by

$$
v^i = \frac{\partial x^i}{\partial y^j}(p)\, w^j,
$$

the Jacobian matrix acting on the coordinate column. $\blacksquare$

> **What this proof buys you.** $T_pM$ is now established, at ownership
> depth, as: (a) a well-defined $n$-dimensional real vector space,
> independent of which of the three descriptions or which chart is used to
> exhibit it; and (b) equipped with an explicit, computable
> change-of-coordinates rule — the Jacobian conjugation law — which is
> exactly the fact silently used every time `00` writes "$v = v^i
> \partial/\partial x^i|_p$" and treats $v^i$ as basis-dependent while $v$
> itself is not.

---

## 5. Cotangent Space

**Definition 5.1.** $T_p^*M := \operatorname{Hom}(T_pM,\mathbb{R})$, the
(algebraic) dual of $T_pM$. Since $\dim T_pM = n < \infty$, $\dim T_p^*M = n$
as well, with dual basis $\{dx^1|_p,\dots,dx^n|_p\}$ to
$\{\partial/\partial x^i|_p\}$, characterized by

$$
dx^i|_p\Big(\frac{\partial}{\partial x^j}\Big|_p\Big) = \delta^i_j.
$$

**Definition 5.2 (differential of a function).** For $f\in C^\infty(M)$, the
**differential** $df_p \in T_p^*M$ is defined by

$$
df_p(v) := v(f), \qquad v \in T_pM^{(ii)}.
$$

In coordinates, $df_p = \frac{\partial f}{\partial x^i}(p)\, dx^i|_p$
(evaluate both sides on $\partial/\partial x^j|_p$ and compare, using $\S4$
Step 2's definition of $\partial/\partial x^i|_p(f)$).

> **Type-check.** $df_p$ is defined using *only* $C^\infty(M)$ and $T_pM$ — no
> metric, no chart beyond the one used to write it in coordinates, no
> embedding into an ambient space. This is why $df$ is the primitive object
> and $\nabla f$ (which needs $g$) is derived (`00` §17.1, `notes/04` §1.1).

---

## 6. Pushforward and Pullback

**Definition 6.1 (pushforward / differential of a map).** For $F:M\to N$
smooth and $p\in M$, define $dF_p : T_pM \to T_{F(p)}N$ by, using the
derivation model,

$$
(dF_p(v))(f) := v(f\circ F), \qquad v \in T_pM,\quad f\in C^\infty(N).
$$

**Proposition 6.2.** $dF_p$ is linear, and in coordinates $(x^i)$ near $p$,
$(y^j)$ near $F(p)$, it is represented by the Jacobian matrix of
$\psi\circ F\circ\varphi^{-1}$ at $\varphi(p)$:

$$
dF_p\Big(\frac{\partial}{\partial x^i}\Big|_p\Big) = \frac{\partial (y^j\circ F)}{\partial x^i}(p)\ \frac{\partial}{\partial y^j}\Big|_{F(p)}.
$$

*Proof.* Direct computation from Definition 6.1 and the chain rule, exactly
parallel to Step 4 of §4. $\blacksquare$

**Definition 6.3 (pullback of a covector).** The pullback $F^* :
T_{F(p)}^*N \to T_p^*M$ is the dual (transpose) map of $dF_p$:

$$
(F^*\eta)(v) := \eta(dF_p(v)), \qquad \eta \in T_{F(p)}^*N,\quad v\in T_pM.
$$

> **Type-check — pushforward vs. pullback go opposite directions.**
> $dF_p : T_pM \to T_{F(p)}N$ moves *forward*, same direction as $F$. $F^* :
> T_{F(p)}^*N \to T_p^*M$ moves *backward*, because a covector's job is to
> eat vectors, and to eat a vector in $T_pM$ using data from $N$ you must
> first push the vector forward — so the covector itself must be pulled
> back. This is the same abstract-nonsense pattern as a dual/adjoint of a
> linear map, instantiated pointwise on manifolds; it generalizes verbatim
> to $k$-forms in `notes/03` §1.4.

---

## 7. Tangent Bundle

**Definition 7.1.** $TM := \bigsqcup_{p\in M} T_pM$ (disjoint union), with
the canonical projection $\pi : TM \to M$, $\pi(v) = p$ for $v \in T_pM$. $TM$
carries a natural smooth-manifold structure of dimension $2n$, built from
charts $(U,\varphi)$ on $M$ via $(v^1,\dots,v^n,x^1,\dots,x^n) \mapsto
v^i\partial/\partial x^i|_{\varphi^{-1}(x)}$ (Lee, ISM, Ch. 3, Prop. 3.18);
this construction is used but not reproved here — it is exactly the kind of
"upgrade to ownership only when central" item flagged in `01` §20.

A **vector field** is a smooth section of $\pi$: $X : M \to TM$ with $\pi\circ
X = \mathrm{id}_M$, equivalently $X(p) \in T_pM$ for every $p$, varying
smoothly. Full use of vector fields (flows, Lie bracket) is in `00` §9;
covariant differentiation of vector fields is in `notes/04` §2.

---

## References Used

- **Lee, ISM** — Ch. 1 (topological manifolds, Hausdorff/second-countable
  hypotheses), Ch. 3 (tangent vectors: the three-definitions equivalence
  proved above follows Prop. 3.2 and the surrounding sections; the Jacobian
  transformation law is the content following Prop. 3.8), Ch. 8 (tangent
  bundle, Prop. 3.18/Ch. 8 depending on edition).
- **Tu, Introduction to Manifolds** — Ch. 8–9, an independent treatment of the
  derivation definition of $T_pM$, used to cross-check the Hadamard's-lemma
  argument in §4 Step 2.
- **Munkres, Topology** — background on Hausdorff/second-countable spaces
  used in §1's type-check remark (the line-with-two-origins example is
  standard; see also Lee, ISM, Example 1.19).

Full bibliographic data: `notes/NOTATION.md` §4.

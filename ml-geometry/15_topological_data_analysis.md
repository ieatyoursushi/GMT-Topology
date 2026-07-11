# 15 — Topological Data Analysis: Persistent Homology and Stability

**Cross-references.** `ml-geometry/NOTATION_ML.md` (typing §1.6, citation key
§3.1–3.2); `notes/05_topology_and_de_rham_cohomology.md` (homology/cohomology
backdrop, at recognition depth); `notes/07_rectifiability_and_hausdorff_measure.md`
§1 (Lipschitz maps — the geometry-to-function transfer step in §4 below
reuses this directly); `notes/06_measure_theory_bridge.md` (Hausdorff-distance
vocabulary, recognition depth).

> **A note on scope, stated up front for honesty.** The genuinely hard
> content of the classical stability theorem for persistence diagrams is the
> statement "interleaved persistence modules have bottleneck-close diagrams"
> — this requires a real matching construction and is not reproved here (it
> is cited at recognition depth, §4). What *is* proved here in full, and is
> itself a nontrivial and instructive fact, is the easier but load-bearing
> half: that **functions close in sup-norm induce interleaved persistence
> modules** (Theorem 3.1) — this is the fact that turns a geometric statement
> ("two point clouds are close") into an algebraic one ("their persistence
> modules are close"), and it is where this document's reuse of `notes/07`'s
> Lipschitz machinery happens (§4).

---

## 0. Standing Definitions

$$
Z \quad \text{a fixed topological space (e.g. } \mathbb{R}^n \text{ or a compact domain)}, \qquad f:Z\to\mathbb{R} \quad \text{a filtering function}.
$$

$$
K_t := f^{-1}\big((-\infty,t]\big) \quad \text{the sublevel set at scale } t, \qquad K_s\subseteq K_t \text{ for } s\le t \quad \text{(a filtration)}.
$$

$$
V_t := H_k(K_t;\mathbb{F}) \quad \text{(fixed homological degree } k\text{, field coefficients } \mathbb{F}\text{)}, \qquad v_{s,t}:V_s\to V_t \quad \text{induced by the inclusion } K_s\hookrightarrow K_t.
$$

$$
\{(V_t,v_{s,t})\}_{t\in\mathbb{R}} \quad \text{a persistence module}: \qquad v_{t,t}=\operatorname{id}_{V_t}, \qquad v_{t',t''}\circ v_{t,t'} = v_{t,t''} \ \ (t\le t'\le t'').
$$

$$
\mathrm{Dgm}(f) \quad \text{persistence diagram of the module } \{V_t\} \quad \text{(a multiset of (birth,death) pairs; well-defined under standard finiteness hypotheses, cited)}.
$$

$$
d_B(\mathrm{Dgm}_1,\mathrm{Dgm}_2) := \inf_{\gamma} \sup_{p\in\mathrm{Dgm}_1} \lVert p-\gamma(p)\rVert_\infty \quad \text{bottleneck distance, infimum over bijections } \gamma \text{ (allowing matching to the diagonal)}.
$$

$$
d_H(P,Q) := \max\Big(\sup_{p\in P}\operatorname{dist}(p,Q),\ \sup_{q\in Q}\operatorname{dist}(q,P)\Big) \quad \text{Hausdorff distance between compact } P,Q\subseteq\mathbb{R}^n.
$$

---

## 1. From Point Cloud to Persistence Diagram

```text
point cloud  X ⊆ ℝⁿ
      │  (build a filtering function, e.g. f(x) = dist(x, X), or a Rips/Čech complex parametrized by scale t)
      ▼
filtration  {K_t}_{t∈ℝ},  K_s ⊆ K_t for s ≤ t
      │  (apply H_k, a functor Top → Vect_𝔽, to every K_t and every inclusion K_s⊆K_t)
      ▼
persistence module  {V_t, v_{s,t}}_{t∈ℝ}
      │  (structure theorem: decompose into interval summands, cited at recognition depth)
      ▼
persistence diagram  Dgm(f)  (multiset of (birth, death) pairs in the half-plane)
```

`notes/05` already built the algebraic-topology backdrop this pipeline sits
on top of, at the level of de Rham cohomology of smooth manifolds; the
$H_k$ used here is instead simplicial/singular homology with field
coefficients — a different but parallel theory (`01` §12 already flags
homology and de Rham cohomology as measuring "holes" via dual
constructions). The "closed $\not\Rightarrow$ exact" intuition of `notes/05`
§2 has a direct analogue here: a homology class that is "born" at some
scale $t$ and "dies" at a later scale $t'$ is exactly a cycle that becomes a
boundary (in the sense of `notes/05` §1's $Z^k/B^k$ quotient) as the
filtration grows — persistence tracks *when* this happens, across the whole
filtration, rather than at a single fixed scale.

### 1.1 Homology at recognition depth — the prerequisite `notes/05` did not cover, supplied

`notes/05` built *de Rham cohomology* (differential forms, $d$). The $H_k$
used in this document is **simplicial homology with field coefficients** — a
parallel theory this repo has not otherwise needed, so its definitions are
recorded here once, at recognition depth (`notes/NOTATION.md` §1.1):

$$
C_k(K;\mathbb{F}) := \text{the } \mathbb{F}\text{-vector space with basis the $k$-simplices of a simplicial complex } K,
$$

$$
\partial_k : C_k(K;\mathbb{F}) \to C_{k-1}(K;\mathbb{F}), \qquad \partial_k[v_0,\dots,v_k] := \sum_{i=0}^k (-1)^i\,[v_0,\dots,\widehat{v_i},\dots,v_k] \quad (\text{hat = omit}),
$$

the alternating sum of a simplex's faces. Direct computation gives
$\partial_{k-1}\circ\partial_k = 0$ (each $(k{-}2)$-face appears twice, with
opposite signs — the same symmetric-cancels-against-antisymmetric mechanism
as $d^2=0$, `notes/03` Theorem 2.4, and $\partial\partial=0$ for currents,
`notes/08` Theorem 2.2). So $\operatorname{im}\partial_{k+1}\subseteq
\ker\partial_k$ and

$$
H_k(K;\mathbb{F}) := \ker\partial_k / \operatorname{im}\partial_{k+1}
$$

— cycles modulo boundaries (`01` §12.2), the same quotient pattern as
$H^k_{\mathrm{dR}} = Z^k/B^k$ (`notes/05` §1) with all arrows reversed
(chains map *down* in degree; forms map *up*). Two facts are used below,
both cited rather than proved: (a) a continuous map — in particular an
inclusion of spaces — induces a linear map on $H_k$, functorially
(Edelsbrunner–Harer, Ch. IV; this is the property Theorem 3.1's proof
leans on); (b) de Rham's theorem (black-boxed per `01` §17) identifies
$H^k_{\mathrm{dR}}(M)$ with the dual of degree-$k$ homology for smooth $M$ —
the precise sense in which the two theories "measure the same holes."

---

## 2. Interleavings of Persistence Modules

**Definition 2.1 ($\delta$-interleaving).** Persistence modules $\{V_t,v_{s,t}\}$
and $\{W_t,w_{s,t}\}$ are **$\delta$-interleaved** ($\delta\ge0$) if there
exist linear maps $\varphi_t:V_t\to W_{t+\delta}$ and $\psi_t:W_t\to V_{t+\delta}$
for every $t$, such that for all $t\le t'$:

$$
\varphi_{t'}\circ v_{t,t'} = w_{t+\delta,t'+\delta}\circ\varphi_t, \qquad \psi_{t'}\circ w_{t,t'} = v_{t+\delta,t'+\delta}\circ\psi_t \quad \text{(commuting with the internal maps)},
$$

and for every $t$:

$$
\psi_{t+\delta}\circ\varphi_t = v_{t,t+2\delta}, \qquad \varphi_{t+\delta}\circ\psi_t = w_{t,t+2\delta} \quad \text{(the two triangle identities)}.
$$

> **Type-check.** A $0$-interleaving ($\delta=0$) forces $\varphi_t,\psi_t$
> to be mutually inverse for every $t$ (the triangle identities collapse to
> $\psi_t\circ\varphi_t=\operatorname{id}$, $\varphi_t\circ\psi_t=\operatorname{id}$),
> i.e. an isomorphism of persistence modules. A $\delta$-interleaving is
> exactly the "up to a uniform shift of $\delta$ in the filtration
> parameter" relaxation of isomorphism — the algebraic notion of "these two
> modules agree, modulo sliding everything by at most $\delta$."

---

## 3. Anchor Proof: Functions Close in Sup-Norm Induce Interleaved Persistence Modules

**Theorem 3.1.** Let $f,g:Z\to\mathbb{R}$ with $\lVert f-g\rVert_\infty\le\delta$.
Let $\{V_t\}=\{H_k(f^{-1}((-\infty,t]))\}$, $\{W_t\}=\{H_k(g^{-1}((-\infty,t]))\}$
be the induced sublevel-set persistence modules (fixed degree $k$). Then
$\{V_t\}$ and $\{W_t\}$ are $\delta$-interleaved.

*Proof.* Write $K_t:=f^{-1}((-\infty,t])$, $L_t:=g^{-1}((-\infty,t])$. Since
$\lVert f-g\rVert_\infty\le\delta$, $f(z)\le g(z)+\delta$ for every $z\in Z$;
consequently, if $z\in K_t$ (i.e. $f(z)\le t$) then $g(z)\le f(z)+\delta\le
t+\delta$, i.e. $z\in L_{t+\delta}$. So

$$
K_t \subseteq L_{t+\delta} \qquad \text{for every } t,
$$

as an inclusion of topological spaces. Symmetrically, $g(z)\le f(z)+\delta$
gives $L_t\subseteq K_{t+\delta}$ for every $t$.

Homology $H_k(-;\mathbb{F})$ is a functor $\mathbf{Top}\to\mathbf{Vect}_\mathbb{F}$:
it sends a continuous map to a linear map, sends the identity map to the
identity linear map, and sends a composite of continuous maps to the
composite of the induced linear maps (this functoriality is exactly the
mechanism already used implicitly, for de Rham cohomology instead of
homology, throughout `notes/05`). Define

$$
\varphi_t := H_k(\iota_{K_t\hookrightarrow L_{t+\delta}}) : V_t \to W_{t+\delta}, \qquad \psi_t := H_k(\iota_{L_t\hookrightarrow K_{t+\delta}}) : W_t \to V_{t+\delta},
$$

the linear maps induced by the two inclusions just established.

*Commutativity with internal maps.* Fix $t\le t'$. The composite inclusion
$K_t\hookrightarrow K_{t'}\hookrightarrow L_{t'+\delta}$ and the composite
inclusion $K_t\hookrightarrow L_{t+\delta}\hookrightarrow L_{t'+\delta}$ are
**the same map** of topological spaces: both send each point of $K_t$ to
itself, regarded as a point of $L_{t'+\delta}$ — there is only one inclusion
map between two nested sets, so any two ways of factoring it through an
intermediate nested set agree. Applying the functor $H_k$ to both
factorizations of this single map, and using that $H_k$ sends composites to
composites,

$$
H_k(\iota_{K_{t'}\hookrightarrow L_{t'+\delta}})\circ v_{t,t'} = w_{t+\delta,t'+\delta}\circ H_k(\iota_{K_t\hookrightarrow L_{t+\delta}}), \quad\text{i.e.}\quad \varphi_{t'}\circ v_{t,t'} = w_{t+\delta,t'+\delta}\circ\varphi_t,
$$

exactly Definition 2.1's first commutativity condition. The identical
argument, with the roles of $K,L$ swapped, gives
$\psi_{t'}\circ w_{t,t'}=v_{t+\delta,t'+\delta}\circ\psi_t$.

*The two triangle identities.* The composite inclusion $K_t\hookrightarrow
L_{t+\delta}\hookrightarrow K_{t+2\delta}$ (first map defining $\varphi_t$,
second map defining $\psi_{t+\delta}$) is, by the same "only one inclusion
between nested sets" observation, the single inclusion $K_t\hookrightarrow
K_{t+2\delta}$. Applying $H_k$ (functoriality again):

$$
H_k(\iota_{L_{t+\delta}\hookrightarrow K_{t+2\delta}})\circ H_k(\iota_{K_t\hookrightarrow L_{t+\delta}}) = H_k(\iota_{K_t\hookrightarrow K_{t+2\delta}}), \quad\text{i.e.}\quad \psi_{t+\delta}\circ\varphi_t = v_{t,t+2\delta},
$$

the first triangle identity. The symmetric argument (swap $K,L$) gives
$\varphi_{t+\delta}\circ\psi_t=w_{t,t+2\delta}$. All four conditions of
Definition 2.1 hold, so $\{V_t\}$ and $\{W_t\}$ are $\delta$-interleaved. $\blacksquare$

---

## 4. From Geometric Perturbation to Functional Perturbation: Reusing `notes/07`'s Lipschitz Machinery

Theorem 3.1 is an algebraic statement about two *functions*. To make it a
statement about two *point clouds* — the actual object of interest in TDA
applications — requires converting a geometric perturbation (point clouds
close in Hausdorff distance) into a sup-norm perturbation of a filtering
function. This is exactly the kind of Lipschitz-continuity fact `notes/07`
built machinery for.

**Lemma 4.1.** For a nonempty compact $P\subseteq\mathbb{R}^n$, the distance
function $d_P(x):=\min_{p\in P}|x-p|$ is $1$-Lipschitz (`notes/07` §0's
$\operatorname{Lip}(f)\le1$).

*Proof.* For any $x,y\in\mathbb{R}^n$ and any $p\in P$, the triangle
inequality gives $d_P(x)\le|x-p|\le|x-y|+|y-p|$; taking the infimum over
$p\in P$ on the right, $d_P(x)\le|x-y|+d_P(y)$. Swapping $x,y$ gives
$d_P(y)\le|x-y|+d_P(x)$. Combining, $|d_P(x)-d_P(y)|\le|x-y|$. $\blacksquare$

**Lemma 4.2.** If $P,Q\subseteq\mathbb{R}^n$ are nonempty compact with
$d_H(P,Q)\le\rho$, then $\lVert d_P-d_Q\rVert_\infty\le\rho$.

*Proof.* Fix $x\in\mathbb{R}^n$. Since $\sup_{p\in P}\operatorname{dist}(p,Q)\le\rho$,
for any $p\in P$ there is $q\in Q$ with $|p-q|\le\rho$, so $d_Q(x)\le|x-q|\le
|x-p|+\rho$; taking the infimum over $p\in P$, $d_Q(x)\le d_P(x)+\rho$. The
symmetric argument (using $\sup_{q\in Q}\operatorname{dist}(q,P)\le\rho$)
gives $d_P(x)\le d_Q(x)+\rho$. Combining, $|d_P(x)-d_Q(x)|\le\rho$ for every
$x$, i.e. $\lVert d_P-d_Q\rVert_\infty\le\rho$. $\blacksquare$

**Corollary 4.3.** If $P,Q$ are point clouds with $d_H(P,Q)\le\rho$, then by
Lemma 4.2, $\lVert d_P-d_Q\rVert_\infty\le\rho$, so by Theorem 3.1 (applied
with $f=d_P,g=d_Q,\delta=\rho$), the sublevel-set persistence modules of
$d_P$ and $d_Q$ (equivalently, the Čech-filtration persistence modules of
$P$ and $Q$, since $d_P^{-1}((-\infty,t])$ is exactly the union of
radius-$t$ balls around $P$) are $\rho$-interleaved.

**Theorem 4.4 (algebraic stability, cited at recognition depth — the harder
half flagged at the top of this document).** If two persistence modules are
$\delta$-interleaved (and satisfy standard finiteness hypotheses, e.g.
pointwise finite dimensionality, guaranteeing well-defined persistence
diagrams), then $d_B(\mathrm{Dgm}_1,\mathrm{Dgm}_2)\le\delta$.

*(Cohen-Steiner–Edelsbrunner–Harer 2007, the original result, proved via an
explicit matching/elder-rule argument; Chazal et al. 2016 for the modern,
purely algebraic proof via interval-module decomposition. Not reproved here
— this is the genuinely technical half of the stability theorem.)*

**Putting it together:** combining Corollary 4.3 with Theorem 4.4,

$$
d_H(P,Q)\le\rho \quad\Longrightarrow\quad d_B\big(\mathrm{Dgm}(P),\mathrm{Dgm}(Q)\big)\le\rho,
$$

exactly the robustness guarantee — *small geometric perturbations of a point
cloud produce small changes in its persistence diagram* — that makes
persistent homology a mathematically well-posed feature of noisy or sampled
data, not merely a heuristic.

---

## 5. TDA Features in ML (Recognition Depth)

Persistence diagrams are not, by themselves, inputs a standard ML model can
consume (a variable-size multiset with no natural vector-space structure).
**Vectorization** methods — persistence images, persistence landscapes,
and kernel methods on diagrams (using $d_B$ or the closely related
$p$-Wasserstein-on-diagrams distance, `ml-geometry/NOTATION_ML.md` §1.6) —
convert a diagram into a fixed-dimensional feature usable downstream. This
sub-area is flagged as fast-moving (cited only, not developed further here);
Chazal–Michel 2021 is a current, practically oriented survey.

---

## References Used

- **Cohen-Steiner–Edelsbrunner–Harer 2007** — the original stability
  theorem (Theorem 4.4, cited); the elder-rule matching-construction proof.
- **Edelsbrunner–Harer** — *Computational Topology* — background on
  simplicial complexes, filtrations, persistence diagrams (§0–§1).
- **Chazal et al. 2016** — *The Structure and Stability of Persistence
  Modules* — the modern algebraic-stability proof (Theorem 4.4, cited) and
  the interleaving-distance formalism this document's Definition 2.1 and
  Theorem 3.1 directly follow.
- **Carlsson 2009** — "Topology and Data" — the foundational survey framing
  persistent homology as a data-analysis tool (§1).
- **Chazal–Michel 2021** — a current, practically-oriented survey (§5).
- **`notes/05_topology_and_de_rham_cohomology.md`** — the algebraic-topology
  backdrop (§1); **`notes/07_rectifiability_and_hausdorff_measure.md`** §1 —
  Lipschitz maps, reused directly in Lemma 4.1–4.2 (§4).

Full bibliographic data: `ml-geometry/NOTATION_ML.md` §3.

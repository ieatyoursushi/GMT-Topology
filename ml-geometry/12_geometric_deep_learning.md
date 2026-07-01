# 12 — Geometric Deep Learning: Symmetry, Equivariance, and Graph Neural Networks

**Cross-references.** `ml-geometry/NOTATION_ML.md` (typing §1.3, citation
key §3.1–3.2); `notes/02_manifolds_and_tangent_spaces.md` (tangent
spaces/pushforward, reused at recognition depth for the "geodesics/gauges"
pillar in §4); `notes/04_riemannian_metrics_and_curvature.md` (connections,
reused at recognition depth for gauge-equivariant networks in §4).

---

## 0. Standing Definitions

$$
G \quad \text{a group}, \qquad \rho:G\to\operatorname{Homeo}(\mathcal{X}) \quad \text{an action on the input domain}, \qquad \pi:G\to\operatorname{GL}(\mathcal{Y}) \quad \text{a representation on the feature/output space}.
$$

$$
\Phi:\mathcal{X}\to\mathcal{Y} \text{ is $G$-equivariant} \iff \Phi(\rho_g(x)) = \pi_g(\Phi(x)) \ \ \forall g\in G, x\in\mathcal{X}.
$$

$$
\Phi \text{ is $G$-invariant} \iff \pi_g = \operatorname{id}_{\mathcal{Y}} \ \ \forall g\in G \quad \text{(the special case of equivariance with trivial output action)}.
$$

$$
\mathcal{G}=(V,E) \quad \text{a graph}, \qquad N(v):=\{u\in V : (u,v)\in E\}, \qquad h_v\in\mathbb{R}^d \quad \text{node feature}, \qquad \sigma\in S_{|V|} \quad \text{a node relabeling permutation}.
$$

---

## 1. Symmetry as the Organizing Principle

Bronstein et al. 2021 (flagged per `ml-geometry/NOTATION_ML.md` §3.2 as a
current, arXiv-only reference and the field's current organizing proposal,
not settled consensus) organize a large fraction of deep learning
architectures around five geometric domains sharing a common symmetry
group: **Grids** (translation group, classical CNNs), **Groups** (general
group convolutions), **Graphs** (permutation group $S_n$, GNNs),
**Geodesics** (isometry group of a Riemannian manifold, mesh/surface
networks), **Gauges** (local frame changes, gauge-equivariant networks on
manifolds with no global symmetry group). This document proves the anchor
result for the Graphs case (§3) and treats the others at recognition depth
(§4), since the Graphs case is both the most widely deployed and the
cleanest to state precisely.

> **Type-check — invariance vs. equivariance, made unambiguous by the
> types.** Given the definitions in §0: an invariant $\Phi$ has type
> "output does not see $g$ at all" ($\pi_g=\operatorname{id}$); an
> equivariant $\Phi$ has type "output transforms *predictably*, by the same
> $g$, acting through $\pi$ rather than $\rho$." A common conflation in
> informal treatments is to call a network "invariant" when it is actually
> only equivariant (e.g. many GNN *layers* are equivariant; it is typically
> only a final pooling layer, e.g. a permutation-invariant readout, that
> makes the *whole network* invariant to node relabeling). Writing out
> $\Phi\circ\rho_g$ vs. $\pi_g\circ\Phi$ explicitly, as this document does
> throughout, removes the ambiguity.

---

## 2. Group-Equivariant Linear Maps and Convolution

**Proposition 2.1 (recognition depth).** For $G$ a locally compact group
acting on itself by translation (so $\mathcal{X}=\mathcal{Y}=G$, $\rho_g=\pi_g=$
left translation by $g$), a bounded linear map $\Phi$ is $G$-equivariant if
and only if $\Phi$ is convolution against some fixed kernel: $\Phi(f)(x) =
\int_G f(y)\,k(y^{-1}x)\,d\mu_{\mathrm{Haar}}(y)$.

*(Cited, not reproved — a standard representation-theoretic fact requiring
Haar measure on $G$; Cohen–Welling 2016 §2–3 for the finite/discrete-group
specialization used in group-equivariant CNNs.)* This proposition is the
precise statement behind "convolution is the unique linear operation
compatible with translation symmetry," and it is why classical CNNs are
already a (translation-)geometric-deep-learning architecture, predating the
name.

---

## 3. Anchor Proof: Message-Passing GNN Layers Are Permutation-Equivariant

**Definition 3.1 (message-passing layer).** A message-passing update on
$\mathcal{G}=(V,E)$ with features $\{h_v\}_{v\in V}$ is

$$
h_v' = \varphi\Big(h_v,\ \bigoplus_{u\in N(v)} \psi(h_v,h_u)\Big), \qquad \varphi,\psi \text{ fixed functions (shared across all } v\text{)}, \qquad \bigoplus \text{ a symmetric (permutation-invariant) aggregator}.
$$

$\bigoplus$ "symmetric" means: for any finite multiset $\{z_1,\dots,z_k\}$
and any permutation $\tau\in S_k$, $\bigoplus_i z_i = \bigoplus_i z_{\tau(i)}$
— e.g. sum, mean, or max satisfy this; a fixed-order concatenation does not.

**Theorem 3.2.** Let $\sigma\in S_{|V|}$ act on $\mathcal{G}=(V,E)$ by
relabeling: $\sigma\cdot\mathcal{G} = (V, \{(\sigma(u),\sigma(v)):(u,v)\in E\})$,
and correspondingly relabel features $(\sigma\cdot h)_{\sigma(v)} := h_v$.
Then one layer of message passing (Definition 3.1) is equivariant:

$$
H'(\sigma\cdot\mathcal{G},\ \sigma\cdot h) = \sigma\cdot H'(\mathcal{G},h),
$$

where $H'(\mathcal{G},h)$ denotes the collection of updated features
$\{h_v'\}_{v\in V}$ produced by Definition 3.1.

*Proof.* Fix $\sigma$. Write $\tilde v:=\sigma(v)$ for the relabeled index of
node $v$, and $\tilde h_{\tilde v} := h_v$ for the relabeled feature
(so $\tilde h = \sigma\cdot h$ by definition). The neighborhood of $\tilde v$
in $\sigma\cdot\mathcal{G}$ is, by construction of the relabeled edge set,

$$
N_{\sigma\cdot\mathcal{G}}(\tilde v) = \{\sigma(u) : u\in N_\mathcal{G}(v)\} = \sigma(N_\mathcal{G}(v)).
$$

Applying Definition 3.1 to $(\sigma\cdot\mathcal{G}, \tilde h)$ at node $\tilde v$:

$$
\tilde h_{\tilde v}' = \varphi\Big(\tilde h_{\tilde v},\ \bigoplus_{\tilde u\in N_{\sigma\cdot\mathcal{G}}(\tilde v)} \psi(\tilde h_{\tilde v},\tilde h_{\tilde u})\Big) = \varphi\Big(h_v,\ \bigoplus_{u\in N_\mathcal{G}(v)} \psi(h_v,h_u)\Big),
$$

where the last equality substitutes $\tilde u=\sigma(u)$ for $u\in
N_\mathcal{G}(v)$ (a relabeling of the *index set* the aggregator ranges
over, from $N_{\sigma\cdot\mathcal{G}}(\tilde v)$ to $N_\mathcal{G}(v)$ via
the bijection $\sigma$), uses $\tilde h_{\tilde u}=h_u$ and
$\tilde h_{\tilde v}=h_v$ by definition of $\tilde h$, and uses that
$\bigoplus$ evaluated over the multiset $\{\psi(h_v,h_u):u\in N_\mathcal{G}(v)\}$
does not depend on the order/labeling in which its arguments are presented
— exactly the symmetric-aggregator hypothesis, applied to the relabeling
bijection $\sigma$ restricted to $N_\mathcal{G}(v)\to N_{\sigma\cdot\mathcal{G}}(\tilde v)$.
The right-hand side is exactly $h_v'$ from Definition 3.1 applied to the
*original* $(\mathcal{G},h)$. So $\tilde h_{\tilde v}' = h_v'$ for every $v$,
i.e. $\tilde H' = \sigma\cdot H'$, which is the claimed equivariance. $\blacksquare$

> **Why this is the load-bearing fact, not a side remark.** Every GNN
> architecture built by stacking message-passing layers (Definition 3.1) —
> GCN, GraphSAGE, GAT, GIN, and the general message-passing neural network
> framework of Gilmer et al. 2017 — inherits Theorem 3.2 layer-by-layer
> (composition of equivariant maps is equivariant, a one-line check), and a
> final permutation-*invariant* readout (e.g. summing all $h_v$ at the last
> layer) converts the whole equivariant stack into a graph-level invariant
> function. This is *why* GNNs generalize across differently-labeled but
> isomorphic graphs without needing to see every possible labeling during
> training — the architecture is equivariant/invariant by construction, not
> by having learned it from data. Theorem 3.2 is exactly the fact that
> licenses this claim, made precise instead of asserted.

---

## 4. Beyond Permutations (Recognition Depth)

**Steerable/gauge-equivariant networks.** On a manifold $M$ with no global
symmetry group (unlike $\mathbb{R}^n$'s translation group or a graph's
permutation group), one instead demands equivariance under *local* frame
changes (gauge transformations) at each point — using exactly the
`notes/04` apparatus of a connection $\nabla$ to relate feature vectors at
nearby points consistently. Cohen et al.'s gauge-equivariant CNNs (cited at
recognition depth) are the direct geometric-deep-learning analogue of
parallel transport (`notes/04` §2) applied to learned features rather than
tangent vectors.

**$E(3)$-equivariance for molecular/point-cloud data.** For inputs in
$\mathbb{R}^3$ where the relevant symmetry is the Euclidean group $E(3)$
(rotations, translations, reflections), architectures explicitly build in
equivariance under $\rho_g=$ rigid motion, $\pi_g=$ the corresponding action
on vector/tensor features — a direct higher-dimensional analogue of
Proposition 2.1's translation-equivariance-is-convolution fact, but for a
noncommutative, continuous group. Flagged explicitly as fast-moving:
architectural specifics in this sub-area are under active revision as of
this writing.

**Weisfeiler–Leman expressivity.** A separate, harder question (cited only,
not addressed here): *which* pairs of non-isomorphic graphs can a given
equivariant GNN architecture distinguish? This connects GNN expressivity to
the classical Weisfeiler–Leman graph-isomorphism heuristic (Xu et al. 2019,
cited at recognition depth) and is a genuinely active research frontier —
flagged, not attempted, since it requires substantially more combinatorial
machinery than the equivariance statement of §3.

---

## References Used

- **Bronstein et al. 2021** — the "5 G's" organizing framework (§1), cited
  with the explicit caveat (per `ml-geometry/NOTATION_ML.md` §3.2) that this
  is an arXiv proto-book and the field's current, not final, organizing
  proposal.
- **Bronstein et al. 2017** — the earlier, peer-reviewed survey covering
  the same ground at lower ambition (§1, foundational).
- **Cohen–Welling 2016** — group-equivariant convolutional networks;
  Proposition 2.1's finite/discrete-group case (§2).
- **Gilmer et al. 2017** — the message-passing neural network framework
  that Definition 3.1 and Theorem 3.2 directly formalize (§3).
- **`notes/02_manifolds_and_tangent_spaces.md`**, **`notes/04_riemannian_metrics_and_curvature.md`**
  — the tangent-space/connection apparatus reused at recognition depth for
  gauge-equivariant networks (§4).

Full bibliographic data: `ml-geometry/NOTATION_ML.md` §3.

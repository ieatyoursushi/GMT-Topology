# Notation and Standing Conventions — Phase 3 (Geometric ML)

**Role of this file.** This is the Phase-3 analogue of `notes/NOTATION.md`.
It does **not** restate the house style, the pure-math typing context, the
depth scale, or the pure-math citation key — all of those remain in force
exactly as written there. This file adds only what Phase 3 (the geometry–ML
intersection) needs on top: ML-specific typed objects, an ML-facing citation
key, and — the one genuinely new piece of infrastructure this phase
requires — a convention for citing a *different, external* git repository
precisely.

---

## 0. Inheritance Declaration

Every document in `ml-geometry/` is bound by, without restating:

- **`notes/NOTATION.md` §1** — the ten house-style rules (redundant typing,
  §0 Standing Definitions per document, type tables, typed pipeline
  diagrams, type-check callouts, primitive-vs-derived discipline, precise
  inline citations + References Used, selective rigor with ≥1 anchor proof,
  tone).
- **`notes/NOTATION.md` §1.1** — the three-level depth scale (A-recognition,
  B-operational, C-ownership). Every Phase-3 deep dive carries ≥1 result at
  depth C, exactly as `notes/02`–`09` do.
- **`notes/NOTATION.md` §2** — the global typing context ($M$, $T_pM$,
  $\Omega^k(M)$, $g$, $\nabla$, $\mathcal{H}^k$, $D_k(\mathbb{R}^n)$,
  $V_k(\mathbb{R}^n)$, $BV(\Omega)$, Einstein summation, etc.). Phase-3
  documents use these bindings directly — e.g. `notes/04`'s musical
  isomorphism $\sharp$ is reused, not redefined, in `10_information_geometry.md`
  and `13_riemannian_optimization.md`.
- **`notes/NOTATION.md` §4** — the pure-math Citation Key (Lee, Federer,
  Evans–Gariepy, etc.). Cited by the same short keys throughout Phase 3.

Cross-references from a Phase-3 document into Phase 1–2 material use the
same bracketed-section convention already established, e.g. `notes/04 §1.1`,
`notes/09 Thm 2.3`.

---

## 1. New Typing Context for Phase 3

### 1.1 Statistical manifolds and information geometry

$$
\Theta \subseteq \mathbb{R}^p \quad \text{parameter space (open)}, \qquad p_\theta : \mathcal{X}\to[0,\infty) \quad \text{a density}, \qquad \int_{\mathcal{X}} p_\theta \,d\mu = 1 \ \ \forall\theta\in\Theta.
$$

$$
\mathcal{P} = \{p_\theta : \theta\in\Theta\} \quad \text{a statistical manifold, with } \theta \text{ serving as a global chart}, \qquad \dim\mathcal{P}=p.
$$

$$
s_\theta(x) := \nabla_\theta \log p_\theta(x) \in \mathbb{R}^p \quad \text{the score function}, \qquad \mathbb{E}_{p_\theta}[s_\theta] = 0 \quad \text{(proved in `10' §2)}.
$$

$$
g^F_\theta : T_\theta\Theta\times T_\theta\Theta\to\mathbb{R}, \qquad g^F_{ij}(\theta) := \mathbb{E}_{p_\theta}\!\big[\partial_i\log p_\theta\cdot \partial_j\log p_\theta\big] \quad \text{the Fisher information metric}.
$$

### 1.2 Optimal transport

$$
\mu,\nu \quad \text{Borel probability measures on } \mathbb{R}^n, \qquad \Pi(\mu,\nu) := \{\pi \in \mathcal{M}(\mathbb{R}^n\times\mathbb{R}^n) : \pi(\cdot\times\mathbb{R}^n)=\mu,\ \pi(\mathbb{R}^n\times\cdot)=\nu\} \quad \text{couplings}.
$$

$$
c:\mathbb{R}^n\times\mathbb{R}^n\to[0,\infty) \quad \text{a cost function}, \qquad W_p(\mu,\nu) := \Big(\inf_{\pi\in\Pi(\mu,\nu)} \int |x-y|^p\,d\pi(x,y)\Big)^{1/p} \quad p\ge 1.
$$

$$
a\in\Delta^{n-1}, \ b\in\Delta^{m-1} \quad \text{discrete marginals (probability simplices)}, \qquad C\in\mathbb{R}^{n\times m}_{\ge0} \quad \text{cost matrix}, \qquad P\in\Pi(a,b)\subseteq\mathbb{R}^{n\times m}_{\ge 0} \quad \text{transport plan}.
$$

$$
H(P) := -\sum_{i,j} P_{ij}(\log P_{ij}-1) \quad \text{entropy}, \qquad \varepsilon>0 \quad \text{entropic-regularization parameter}, \qquad u,v \quad \text{Sinkhorn scaling potentials}.
$$

### 1.3 Group equivariance and geometric deep learning

$$
G \quad \text{a group}, \qquad \rho: G\to \operatorname{Homeo}(\mathcal{X}) \quad \text{an action on the input space}, \qquad \pi: G\to\operatorname{GL}(\mathcal{Y}) \quad \text{a representation on the output/feature space}.
$$

$$
\Phi:\mathcal{X}\to\mathcal{Y} \quad \text{is $G$-equivariant} \iff \Phi(\rho_g(x)) = \pi_g(\Phi(x)) \ \ \forall g\in G,\, x\in\mathcal{X}; \qquad \text{invariant} \iff \pi_g = \operatorname{id}\ \forall g.
$$

$$
\mathcal{G}=(V,E) \quad \text{a graph}, \qquad N(v) \subseteq V \quad \text{neighborhood of } v, \qquad h_v \in \mathbb{R}^d \quad \text{node feature}, \qquad \sigma \in S_{|V|} \quad \text{a permutation of node labels}.
$$

### 1.4 Riemannian optimization

$$
M \quad \text{a Riemannian manifold (`notes/NOTATION.md' §2.2–2.3)}, \qquad f:M\to\mathbb{R} \quad C^1, \qquad \operatorname{grad}f(x) := \sharp_x(df_x) \in T_xM
$$

— **directly reusing** the musical isomorphism $\sharp$ of `notes/04` §1.1
with $g$ the manifold's given metric; no new definition, only a new name
("Riemannian gradient") for the same typed object.

$$
R_x : T_xM \to M \quad \text{a retraction}: \quad R_x(0)=x, \qquad dR_x|_0 = \operatorname{id}_{T_xM} \quad \text{(a computationally cheap local surrogate for } \exp_x\text{)}.
$$

$$
\operatorname{St}(n,p) := \{X\in\mathbb{R}^{n\times p} : X^\top X = I_p\} \quad \text{Stiefel manifold}, \qquad \operatorname{Gr}(n,p) \quad \text{Grassmann manifold (quotient of } \operatorname{St}(n,p) \text{ by } O(p)\text{)}.
$$

### 1.5 GMT-for-shapes

$$
X = \{x_1,\dots,x_N\}\subseteq\mathbb{R}^n \quad \text{a point cloud}, \qquad \hat\mu_X := \tfrac1N\sum_{i=1}^N \delta_{x_i} \quad \text{its empirical measure (`01' §13.3)}.
$$

$$
T_h \in D_{n-1}(\mathbb{R}^n) \quad \text{the current of a mesh/triangulated approximation at mesh size } h>0 \quad \text{(`ml-geometry/14' §3)}.
$$

### 1.6 Topological data analysis

$$
K_t \quad \text{a filtration of simplicial complexes indexed by } t\in\mathbb{R} \ (K_s\subseteq K_t \text{ for } s\le t), \qquad H_k(K_t) \quad \text{degree-}k\text{ homology}.
$$

$$
\mathrm{Dgm}(f) \quad \text{persistence diagram of a filtering function } f: \mathbb{R}^n\to\mathbb{R} \quad \text{(multiset of (birth, death) pairs)}, \qquad d_B(\mathrm{Dgm}_1,\mathrm{Dgm}_2) \quad \text{bottleneck distance}.
$$

---

## 2. Master Dictionary Supplement: Geometry Object $\to$ ML Role

Same table shape as `notes/NOTATION.md` §3 (smooth $\to$ GMT); here the
correspondence runs pure geometry $\to$ its ML instantiation.

| Geometric object | Type | ML instantiation | Type | Reference |
|---|---|---|---|---|
| Riemannian metric $g$ | $g_p:T_pM\times T_pM\to\mathbb{R}$ | Fisher–Rao metric $g^F$ | $g^F_\theta:T_\theta\Theta\times T_\theta\Theta\to\mathbb{R}$ | `10` |
| Geodesic distance | $d_g(p,q)=\inf_\gamma L(\gamma)$ | Wasserstein distance | $W_p(\mu,\nu)$ | `11` |
| Musical isomorphism $\sharp$ | $\sharp_p:T_p^*M\to T_pM$ | Natural-gradient preconditioning | $\tilde\nabla f = \sharp_{g^F}(df)$ | `10`, `notes/04` §1.1 |
| Group action on tangent spaces / equivariant structure | $dF_p:T_pM\to T_{F(p)}N$ | Equivariant neural network layer | $\Phi\circ\rho_g=\pi_g\circ\Phi$ | `12` |
| Connection / parallel transport | $\nabla_XY$ | Riemannian (S)GD update | $x_{k+1}=R_{x_k}(-\eta\operatorname{grad}f(x_k))$ | `13`, `notes/04` §2 |
| Rectifiable set / current | $E$, $T\in D_k(\mathbb{R}^n)$ | Point cloud / mesh as shape representation | $\hat\mu_X$, $T_h$ | `14`, `notes/07`, `notes/08` |
| Varifold | $V\in V_k(\mathbb{R}^n)$ | Unoriented shape representation for registration | — | `14`, `notes/09` |
| Reduced boundary $\partial^*E$ | $\mathcal{H}^{n-1}$-rectifiable | Learned decision boundary / level set of $\hat\eta$ | $L_c=\{\hat\eta=c\}$ | `16`, `notes/09` |
| Homology / cohomology (holes) | $H_k$, $H^k_{\mathrm{dR}}$ | Persistent homology of data | $H_k(K_t)$, $\mathrm{Dgm}$ | `15`, `notes/05` |

---

## 3. ML Citation Key

Split, as house style requires for this literature, into **foundational/
classic** references (stable across decades) and **current (2020s)**
references (flagged as more likely to be superseded or reframed as the field
moves — cite the survey/monograph as the anchor, individual papers as
supporting examples).

### 3.1 Foundational / classic

- **Amari 1998** — S. Amari, "Natural Gradient Works Efficiently in
  Learning," *Neural Computation* 10(2), 1998.
- **Amari, IG** — S. Amari, *Information Geometry and Its Applications*,
  Applied Mathematical Sciences 194, Springer, 2016.
- **Amari–Nagaoka** — S. Amari, H. Nagaoka, *Methods of Information
  Geometry*, AMS/Oxford University Press, 2000.
- **Rao 1945** — C. R. Rao, "Information and the Accuracy Attainable in the
  Estimation of Statistical Parameters," *Bulletin of the Calcutta
  Mathematical Society* 37, 1945.
- **Villani, OT** — C. Villani, *Optimal Transport: Old and New*,
  Grundlehren der mathematischen Wissenschaften 338, Springer, 2009.
- **Kantorovich 1942** — L. Kantorovich, "On the Translocation of Masses,"
  *C.R. (Doklady) Acad. Sci. URSS* 37, 1942.
- **Absil–Mahony–Sepulchre** — P.-A. Absil, R. Mahony, R. Sepulchre,
  *Optimization Algorithms on Matrix Manifolds*, Princeton University Press,
  2008.
- **Edelsbrunner–Harer** — H. Edelsbrunner, J. Harer, *Computational
  Topology: An Introduction*, American Mathematical Society, 2010.
- **Cohen-Steiner–Edelsbrunner–Harer 2007** — D. Cohen-Steiner, H.
  Edelsbrunner, J. Harer, "Stability of Persistence Diagrams," *Discrete &
  Computational Geometry* 37(1), 2007.
- **Charon–Trouvé 2013** — N. Charon, A. Trouvé, "The Varifold
  Representation of Nonoriented Shapes for Diffeomorphic Registration,"
  *SIAM Journal on Imaging Sciences* 6(4), 2013.
- **Vaillant–Glaunès 2005** — M. Vaillant, J. Glaunès, "Surface Matching via
  Currents," *Information Processing in Medical Imaging (IPMI)*, 2005.
- **Carlsson 2009** — G. Carlsson, "Topology and Data," *Bulletin of the
  American Mathematical Society* 46(2), 2009.
- **Bronstein et al. 2017** — M. Bronstein, J. Bruna, Y. LeCun, A. Szlam, P.
  Vandergheynst, "Geometric Deep Learning: Going Beyond Euclidean Data,"
  *IEEE Signal Processing Magazine* 34(4), 2017.

### 3.2 Current (2020s) — anchor on the monograph/survey, treat individual results as supporting examples that may be superseded

- **Peyré–Cuturi** — G. Peyré, M. Cuturi, "Computational Optimal
  Transport," *Foundations and Trends in Machine Learning* 11(5–6), 2019
  (also arXiv:1803.00567).
- **Cuturi 2013** — M. Cuturi, "Sinkhorn Distances: Lightspeed Computation
  of Optimal Transport," *NeurIPS*, 2013.
- **Bronstein et al. 2021** — M. Bronstein, J. Bruna, T. Cohen, P.
  Veličković, "Geometric Deep Learning: Grids, Groups, Graphs, Geodesics,
  and Gauges," arXiv:2104.13478, 2021. *(Flag: arXiv preprint/proto-book, not
  a peer-reviewed venue — the field's current reference point, likely to be
  reframed as the area matures.)*
- **Cohen–Welling 2016** — T. Cohen, M. Welling, "Group Equivariant
  Convolutional Networks," *ICML*, 2016.
- **Gilmer et al. 2017** — J. Gilmer, S. Schoenholz, P. Riley, O. Vinyals,
  G. Dahl, "Neural Message Passing for Quantum Chemistry," *ICML*, 2017.
- **Boumal, ISRO** — N. Boumal, *An Introduction to Optimization on Smooth
  Manifolds*, Cambridge University Press, 2023.
- **Bonnabel 2013** — S. Bonnabel, "Stochastic Gradient Descent on
  Riemannian Manifolds," *IEEE Transactions on Automatic Control* 58(9),
  2013.
- **Kaltenmark–Charon–Trouvé 2017** — I. Kaltenmark, N. Charon, A. Trouvé,
  "A General Framework for Curve and Surface Comparison and Registration
  with Oriented Varifolds," *CVPR*, 2017.
- **Chazal et al. 2016** — F. Chazal, V. de Silva, M. Glisse, S. Oudot, *The
  Structure and Stability of Persistence Modules*, SpringerBriefs in
  Mathematics, Springer, 2016.
- **Chazal–Michel 2021** — F. Chazal, B. Michel, "An Introduction to
  Topological Data Analysis: Fundamental and Practical Aspects for Data
  Scientists," *Frontiers in Artificial Intelligence* 4, 2021.
- **Santambrogio 2015** — F. Santambrogio, *Optimal Transport for Applied
  Mathematicians*, Progress in Nonlinear Differential Equations and Their
  Applications 87, Birkhäuser, 2015.

---

## 4. Cross-Repo Citation Convention: DirectIndexing_ML

The capstone document (`16_capstone_directindexing_gmt.md`) cites a
*separate* personal research repository, not a subdirectory of this one:
**DirectIndexing_ML**, `https://github.com/ieatyoursushi/DirectIndexing_ML`,
locally cloned at `/Users/gabewkung/Desktop/PSTAT131/FinalProject/`.

**Citation key:** `DIML`.

**Inline form:**

$$
\texttt{(DIML, <path/from/repo/root>, \S/description, pinned <date> @ <short commit>)}
$$

Example: `(DIML, DataMemo/data_memo_theory.md §4.4, pinned 2026-07-01 @ 7a15f09)`.

**Pinning statement (stated once, here, binding for all `DIML` citations in
this repo):** all `DIML` citations in Phase 3 refer to the state of that
repository as investigated and directly verified on **2026-07-01**, at
short commit **`7a15f09`**. The live repository has continued past this
point — notably, commit `c421c4f` ("dynamic data-collection and simulation
ranges for models instead of the fixed 2 years beforehand") widened the
simulation window from roughly 2 years to roughly 20 years, which changed
several *empirical* findings quoted in the memos (e.g. which model wins on
which target). This drift does not affect the *theoretical* claims cited
here (the oracle's gate structure, the soft-label constructions, the GMT
scaffolding in §4 of `data_memo_theory.md`), but a reader consulting a later
revision of DIML should relocate cited passages by **file and section**,
not by line number, since line numbers are not stable across revisions.

---

## 5. Register Note for Phase 3

Two tones apply, stated once here so no individual document needs to
re-justify them:

**For the hub and the six topic deep dives:** honest "young field" framing.
The geometry–ML intersection is a genuinely active, fast-moving research
area. These documents anchor on stable monographs/surveys and treat
individual conference-paper citations as illustrative and possibly dated by
the time they are read — this is stated explicitly rather than implied, so
a reader calibrates confidence correctly.

**For the capstone (`16`):** constructive, not a takedown. Both
GMT-topology and DirectIndexing_ML are the same author's own work. Where the
capstone's claim ledger finds that a DIML memo claim needs correction or
refinement, it says so plainly and precisely — matching the rigor this
whole repo is built on — but frames it as *closing the loop* (supplying
rigor a self-aware memo already flagged as aspirational), not as an
external critique.

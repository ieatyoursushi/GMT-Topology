# 16 — Capstone: Closing the Loop on DirectIndexing_ML's GMT Scaffolding

**Cross-references.** `ml-geometry/NOTATION_ML.md` §4 (the `DIML` cross-repo
citation convention — every citation below uses this form), §5 (the
constructive/co-author register this document adopts); `ml-geometry/14_gmt_for_shapes.md`
(the currents-vs-varifolds discussion this document specializes);
`notes/07_rectifiability_and_hausdorff_measure.md`, `notes/08_currents.md`,
`notes/09_varifolds_and_finite_perimeter.md` (the machinery this document
applies directly — Rademacher/area formula, currents and boundary by
duality, finite perimeter and De Giorgi's structure theorem).

**What this document is.** DirectIndexing_ML (`ieatyoursushi/DirectIndexing_ML`,
locally at `/Users/gabewkung/Desktop/PSTAT131/FinalProject/`) is a separate
personal ML research project: a pipeline for tax-loss-harvesting decisions
in a simulated direct-indexing portfolio. Its own theory memo
(`DataMemo/data_memo_theory.md` §4) sketches an interpretation of its
"oracle boundary" using currents, generalized Stokes, and a Dirichlet-problem
framing for regularization — but its own author is explicit
(`data_memo_theory_part2.md`, quoted precisely in §3 below) that this
machinery *"was never operationalized in code or computed numerically... it
is honest to call this scaffolding that shaped the conceptual design but did
not enter the implementation."* This document supplies that rigor, using
`notes/07`–`09`, and is honest — via an explicit claim ledger — about which
parts of the original framing are now justified, which remain useful
analogies, and which need a precise correction. Both repos are the same
author's own work; the tone throughout is constructive.

---

## 0. Standing Definitions

### 0.1 The feature space and its provenance

$$
\mathcal{X}\subset\mathbb{R}^{15} \quad \text{(DIML, `LotStateVector.cs`, pinned 2026-07-01 @ 7a15f09)}.
$$

Fifteen named coordinates, in four groups:

| Group | Coordinates | Type |
|---|---|---|
| Lot-level (6) | $L,H,S,B,W,K$ | $L\in(-1,\infty)$, $H\in\mathbb{Z}_{\ge0}$, $S\in\{0,1\}$, $B\in\mathbb{R}_{>0}$, $W\in(0,1)$, $K\in\mathbb{Z}_{>0}$ |
| Portfolio-level (3) | $G^{YTD},\sigma_{TE},\mathcal{W}_{\mathrm{wash}}$ | $G^{YTD}\in\mathbb{R}$, $\sigma_{TE}\in\mathbb{R}_{\ge0}$, $\mathcal{W}_{\mathrm{wash}}\in\mathbb{Z}_{\ge0}\cup\{999\}$ |
| Asset-level (4) | $R_t,\sigma_{\mathrm{range}},\Delta_{MA50},\Delta_{MA200}$ | continuous |
| Derived (2) | $\alpha_{\mathrm{tax}}, D_{YE}$ | $\alpha_{\mathrm{tax}}\in\mathbb{R}_{\ge0}$, $D_{YE}\in\mathbb{Z}_{\ge0}$ |

> **Notation clash, resolved.** The lot-level portfolio-weight coordinate
> $W\in(0,1)$ (fraction of portfolio value held in this lot) and the
> portfolio-level wash-sale-clock coordinate (days since last harvest of
> this ticker) are two *different* features that DIML's own memo both
> denotes with variants of "$W$"/"$\mathcal{W}$." This document writes
> $\mathcal{W}_{\mathrm{wash}}$ for the wash-clock throughout, reserving
> plain $W$ for the portfolio weight, to prevent exactly the confusion the
> shared symbol invites.

> **Standing caveat on continuity.** Several coordinates ($H,S,K,\mathcal{W}_{\mathrm{wash}},D_{YE}$)
> are integer- or binary-valued in the underlying data-generating process.
> Everything below treats $\mathcal{X}$ as embedded in $\mathbb{R}^{15}$ by
> simply regarding every coordinate as real-valued — the standard, harmless
> move already implicit whenever a kernel method, spline, or (as here)
> geometric-measure-theoretic argument is applied to mixed continuous/count
> data. This is a modeling choice, not a claim that the data is literally
> continuously distributed on every axis; it is stated once, here, rather
> than re-flagged at each use.

### 0.2 The oracle

$$
f^*(x) := \mathbb{1}[L\le-\theta_1]\cdot\mathbb{1}[\sigma_{TE}\le\theta_2]\cdot\mathbb{1}[G^{YTD}>0]\cdot\mathbb{1}[\mathcal{W}_{\mathrm{wash}}\ge\theta_3], \qquad f^*:\mathcal{X}\to\{0,1\}
$$

(the first gate constrains the lot-level unrealized-return coordinate $L$
of §0.1's table — an unrealized loss of at least $\theta_1$; the memo's
lowercase $\ell$ for the same coordinate is not used here, matching the
$W$/$\mathcal{W}_{\mathrm{wash}}$ disambiguation above),

$$
\theta_1=0.02,\quad \theta_2=0.05,\quad\theta_3=30 \quad \text{(DIML, `src/Core/Oracle/OracleBoundary.cs`, pinned 2026-07-01 @ 7a15f09)}.
$$

$$
\Omega := \{x\in\mathcal{X} : f^*(x)=1\} = H_1\cap H_2\cap H_3\cap H_4, \qquad H_1=\{L\le-\theta_1\},\ H_2=\{\sigma_{TE}\le\theta_2\},\ H_3=\{G^{YTD}>0\},\ H_4=\{\mathcal{W}_{\mathrm{wash}}\ge\theta_3\}.
$$

### 0.3 Soft labels and learned posterior

$$
\tilde y_{BT}(x) := \tfrac{1}{30}\textstyle\sum_{s=1}^{30} f^*(\cdot\,;s) \quad \text{(30-day forward firing frequency along one real price path)},
$$

$$
\tilde y_{GBM}(x) := \tfrac{1}{200}\textstyle\sum_{p=1}^{200}\mathbb{1}\big[\exists s\le30: f^*(\cdot\,;S^{(p)}_s)=1\big] \quad \text{(200-path first-passage probability)}
$$

$$
\text{(DIML, `src/Core/Simulation/SoftLabelBuilder.cs`, pinned 2026-07-01 @ 7a15f09)}.
$$

$$
\hat\eta:\mathcal{X}\to[0,1] \quad \text{the learned posterior}, \qquad L_c:=\{x:\hat\eta(x)=c\} \quad \text{its level sets, } c\in[0,1].
$$

### 0.4 Provenance pin

All `DIML` citations in this document are pinned to **2026-07-01, short
commit `7a15f09`** (`ml-geometry/NOTATION_ML.md` §4). The live repository has
since moved past this point (notably commit `c421c4f`, widening the
simulation window from ~2 to ~20 years, changing some *empirical* findings
but not the theoretical claims addressed here). Quoted passages below are
transcribed directly from the pinned files as read on the investigation
date.

---

## 1. $\Omega$ Is a Convex Polyhedron — and, Taken Literally, an Unbounded One

**Proposition 1.1.** $\Omega$ is convex (a finite intersection of half-spaces
is always convex). Moreover, since the oracle's four gates constrain only
four of $\mathcal{X}$'s fifteen coordinates, $\Omega$ has a product structure:
relabeling coordinates as $(L,\sigma_{TE},G^{YTD},\mathcal{W}_{\mathrm{wash}},\,\text{the
other 11})$,

$$
\Omega = \Omega_4 \times \mathbb{R}^{11}, \qquad \Omega_4 := H_1\cap H_2\cap H_3\cap H_4 \ \subseteq\ \mathbb{R}^4 \text{ (in the four gate coordinates)}.
$$

$\Omega_4$ itself is unbounded: $H_3=\{G^{YTD}>0\}$ has no upper bound, and
$H_4=\{\mathcal{W}_{\mathrm{wash}}\ge30\}$ has no upper bound; only $H_1,H_2$
bound their respective coordinates from one side. Consequently $\Omega$ is
non-compact, and $\mathcal{H}^{14}(\partial\Omega)=\infty$ if computed over
all of $\mathbb{R}^{15}$ (e.g. the facet $\{G^{YTD}=0\}\cap\Omega$ is
unbounded in the 11 free directions, hence has infinite 14-dimensional
Hausdorff measure).

> **This is not a defect to route around silently — it is the honest
> statement of what "$\Omega$ has finite perimeter" can and cannot mean.**
> Real data is finite: only finitely many (lot, day) observations are ever
> realized, so the empirically populated region of $\mathcal{X}$ has compact
> closure. Fix any bounded box $R_0 = \prod_{i=1}^{15}[a_i,b_i]\subseteq
> \mathbb{R}^{15}$ containing this empirically realized range (any such box
> works for what follows; it need not be canonical), and define

$$
\Omega_0 := \Omega\cap R_0.
$$

$\Omega_0$ is a **bounded** convex polyhedron — the object this document
actually computes the perimeter of. $\Omega$ itself, and the questions the
DIML memo poses about it, are best read as questions about $\Omega_0$ for
whatever $R_0$ the data in fact populates.

**Proposition 1.2.** $\partial\Omega_0$ is a union of at most $4+2\cdot15=34$
flat facets (four from the oracle's gates, up to two per coordinate from
$R_0$'s box constraints — many will be inactive/redundant in practice, e.g.
if a box bound is looser than the corresponding oracle gate), meeting at
lower-dimensional edges and corners where $\ge2$ constraints bind
simultaneously. So $\partial\Omega_0$ is only **piecewise** $C^1$ — not
globally $C^1$ — exactly the hypothesis gap in `notes/09` Theorem 3.1, which
assumed a globally $C^1$ boundary. §2 proves the needed replacement.

> **One further simplification, noted once.** $H_3=\{G^{YTD}>0\}$ uses a
> strict inequality, making $H_3$ (and hence naively $\Omega$) an open set
> in that coordinate. Since $\{G^{YTD}=0\}$ is a hyperplane, it has Lebesgue
> measure zero, so $\mathbf{1}_\Omega$ and $\mathbf{1}_{\overline\Omega}$
> (replacing $H_3$ with its closure $\{G^{YTD}\ge0\}$) agree
> $\mathcal{L}^{15}$-a.e. and therefore have the identical distributional
> derivative — perimeter theory (`notes/09` Definition 2.1) does not
> distinguish a set from its Lebesgue-a.e. modifications. Below, $\Omega$
> (and $\Omega_0$) may be freely replaced by its closure without changing
> any perimeter computation.

---

## 2. Anchor Proof: Finite Perimeter of a Bounded Convex Polyhedron via De Giorgi's Structure Theorem

**Theorem 2.1.** Let $P=\bigcap_{i=1}^m H_i\subseteq\mathbb{R}^d$ be a bounded
convex polyhedron with nonempty interior, $H_i=\{x:\langle n_i,x\rangle\le c_i\}$
closed half-spaces with unit outward normals $n_i$. Let $F_i := P\cap\{x:
\langle n_i,x\rangle=c_i\}$ (the $i$-th candidate facet), and call $H_i$
**active** if $F_i$ has nonempty relative interior within its hyperplane
(equivalently, $F_i$ is $(d{-}1)$-dimensional — a redundant constraint gives
$F_i$ empty or lower-dimensional). Then $P$ has finite perimeter,

$$
\partial^*P = \bigcup_{i \text{ active}} F_i^\circ \quad (\text{relative interiors, pairwise disjoint}), \qquad \nu_P = n_i \text{ on } F_i^\circ,
$$

$$
P(P) = \mathcal{H}^{d-1}(\partial^*P) = \sum_{i\text{ active}} \mathcal{H}^{d-1}(F_i).
$$

*Proof.* **Step 1: the non-facet part of $\partial P$ is $\mathcal{H}^{d-1}$-null.**
$\partial P$ (topological boundary) decomposes as $\partial P = \big(\bigcup_i
F_i^\circ\big) \cup \Sigma$, where $\Sigma := \partial P \setminus \bigcup_i
F_i^\circ$ consists of points lying on $\ge2$ of the constraint hyperplanes
simultaneously (the "edges and corners"). Each such point lies in an
intersection of $\ge2$ distinct hyperplanes in $\mathbb{R}^d$, which is an
affine subspace of dimension $\le d-2$; $\Sigma$ is a finite union
(over pairs, triples, etc. of the $m$ constraints) of subsets of such
low-dimensional affine subspaces. A $k$-dimensional affine subspace has
$\mathcal{H}^{d-1}$-measure zero whenever $k\le d-2<d-1$ (direct from the
linear-map area formula, `notes/07` Theorem 3.1, applied to a parametrization
of the affine subspace by $\mathbb{R}^k$: the image of any bounded subset of
$\mathbb{R}^{d-1}$ under a map into a $k$-dimensional subspace with $k<d-1$
has $(d{-}1)$-dimensional Hausdorff measure $0$, since the relevant Jacobian
factor vanishes — the map's rank is too low). So $\mathcal{H}^{d-1}(\Sigma)=0$.

**Step 2: each active facet's relative interior is locally exactly a
half-space.** Fix $i$ active and $x\in F_i^\circ$. Since $x$ is in the
*relative interior* of $F_i$, $x$ lies strictly inside every other
constraint ($\langle n_j,x\rangle<c_j$ for $j\ne i$), so a sufficiently small
ball $B_r(x)$ meets none of the other hyperplanes: $P\cap B_r(x) = H_i\cap
B_r(x)$ exactly. This is the flattest possible special case of a $C^1$
boundary (a literal hyperplane), so the argument of `notes/09` Theorem 3.1
Steps 1–2 applies verbatim with $\nu=n_i$ constant on $B_r(x)$: $x\in
\partial^*P$ with $\nu_P(x)=n_i$.

**Step 3: assemble via De Giorgi.** By Steps 1–2, $P$'s topological boundary
agrees, up to an $\mathcal{H}^{d-1}$-null set, with $\bigcup_i F_i^\circ$,
and the measure-theoretic normal is $n_i$ (constant) on each $F_i^\circ$. By
De Giorgi's structure theorem (`notes/09` Theorem 2.3), $\mathbf{1}_P\in
BV(\mathbb{R}^d)$, $\partial^*P$ is $(d{-}1)$-rectifiable with $D\mathbf{1}_P
= -\nu_P\,\mathcal{H}^{d-1}\llcorner\partial^*P$ (sign per `notes/09`
Definition 2.2's outward-normal convention), and $P(P)=\mathcal{H}^{d-1}(\partial^*P)$.
Since the $F_i^\circ$ (for active $i$) are pairwise disjoint (a point in the
relative interior of $F_i$ lies strictly off every other constraint
hyperplane, by the same argument as Step 2, so cannot simultaneously lie in
$F_j^\circ$ for $j\ne i$) and $\partial^*P=\bigcup_iF_i^\circ$ up to an
$\mathcal{H}^{d-1}$-null set,

$$
P(P) = \mathcal{H}^{d-1}(\partial^*P) = \sum_{i\text{ active}}\mathcal{H}^{d-1}(F_i^\circ) = \sum_{i\text{ active}}\mathcal{H}^{d-1}(F_i),
$$

the last equality because $F_i\setminus F_i^\circ$ is itself contained in a
$\le(d{-}2)$-dimensional affine piece (the same Step-1 argument, applied
within $F_i$'s own hyperplane), hence $\mathcal{H}^{d-1}$-null. $\blacksquare$

**Corollary 2.2 (applying Theorem 2.1 to DIML).** $\Omega_0 = \Omega\cap R_0$
(Proposition 1.1–1.2) is a bounded convex polyhedron with $m\le34$
half-space constraints in $d=15$. By Theorem 2.1, $\Omega_0$ has finite
perimeter,

$$
P(\Omega_0) = \sum_{i\text{ active}} \mathcal{H}^{14}(F_i) \ <\ \infty,
$$

a finite, well-defined, and in principle numerically computable quantity —
in sharp contrast to $\Omega$ itself, whose perimeter over all of
$\mathcal{X}$ is infinite (§1). This is the rigorous replacement for
`notes/09` Theorem 3.1 in the one respect that theorem does not cover
(boundary corners), applied to the specific object DIML's own memo calls
"a $(d{-}1)$-current."

---

## 3. The Claim Ledger

Each row cites a specific claim from the pinned DIML memos verbatim (or
near-verbatim), assigns a verdict, and states the justification.

### Row 1 — "The mechanical oracle is the boundary current; the ML model is the interior current"

> *"The mechanical direct indexing rule is precisely the boundary current
> $[\![\partial\Omega]\!]$... The ML model approximates the interior
> current $[\![\Omega]\!]$"* (DIML, `data_memo_theory.md` §4.3, pinned).

**Verdict: requires a precision, not a correction.** As a literal
set-indicator, $f^*=\mathbb{1}_\Omega$ fires on **all** of $\Omega$, not
only on $\partial\Omega$ — so in the strict GMT sense of `notes/08`
Definition 1.1/1.3, $f^*$ corresponds to the **interior** current
$[\![\Omega]\!]$ (or rather, $\mathbb{1}_\Omega$ as an integrand — the
$d$-current $T_\Omega(\omega)=\int_\Omega\omega$), not the boundary current
$[\![\partial\Omega]\!]$. However, the same memo's later "Sources of
tax-alpha" section makes a *different*, coherent claim that likely motivated
this language:

> *"Source 1 — Boundary placement ($\partial\Omega$): how much $\alpha$ is
> available depends on where $\theta_1$ sits... Source 2 — Interior
> prioritization ($\Omega$): given the boundary is fixed, which lots inside
> $\Omega$ do you harvest first..."* (DIML, `data_memo_theory.md` §10, lines
> 375–380, pinned).

This is a genuine, useful distinction — **which piece of the pipeline
controls what** (calibrating $\theta_1$ controls $\partial\Omega$'s
placement; soft-label ranking controls prioritization within $\Omega$) —
but it is a design-sensitivity statement, not a current-theoretic identity.
This document keeps both readings but does not conflate them: $f^*$ *is*
(the indicator of) the interior current in the strict sense; the
boundary/interior *design* split is a separate, correct, and worth-keeping
observation.

### Row 2 — "Generalized Stokes applies: $\partial[\![\Omega]\!]=[\![\partial\Omega]\!]$"

> (DIML, `data_memo_theory.md` §4.2, pinned.)

**Verdict: now rigorous, with the caveat this document supplies.** The
identity holds by `notes/08` Definition 2.1 (boundary by duality) — but
since $\partial\Omega$ has corners (§1), the applicable justification is
**not** the globally-$C^1$ Stokes argument (`notes/03` Theorem 4.2,
`notes/08` Theorem 2.3), but rather the De Giorgi-based argument of §2 above,
which is exactly the adaptation needed for a piecewise-flat boundary.

### Row 3 — "Currents agreeing on $\partial\Omega$ differ by an element of $\ker(\partial)$, representing a class in $H_d(\mathcal{X};\mathbb{R})$"

> (DIML, `data_memo_theory.md` §4.3, pinned.)

**Verdict: the $\ker(\partial)$ statement is correct; the cohomological
attribution should be dropped, not merely caveated.** `notes/08` Theorem
2.2 ($\partial\partial=0$) does guarantee that two currents with the same
boundary differ by a closed current. But $\mathcal{X}$ (or its continuous
relaxation, §0.1) is convex, hence star-shaped about any of its points, so
`notes/05` Theorem 3.1 (the Poincaré Lemma — stated for exactly this class
of domains, not merely for $\mathbb{R}^n$ as the narrower `notes/05`
Corollary 3.2 is) gives $H^k_{\mathrm{dR}}(\mathcal{X})=0$ for every $k\ge1$.
Since $\mathcal{X}$ is convex it is also contractible in the ordinary
topological sense, and — by de Rham's theorem identifying de Rham
cohomology with singular cohomology (a standard fact listed among the
results this repo's `01` §17 flags as safe to black-box) together with the
elementary fact that a contractible space has the singular homology of a
point — the singular homology the memo actually invokes,
$H_d(\mathcal{X};\mathbb{R})$, also vanishes for every $d>0$. Either way,
there is no genuine topological obstruction here, and invoking a nonzero
homology class is not supported. The real source of "many interior
extensions agreeing on the boundary" is simpler and does not need
cohomology at all: $\partial$ has a large kernel among currents supported in
a fixed compact region, regardless of whether that region has holes — this
is an inductive-bias/uniqueness phenomenon (which hypothesis class
$\mathcal{H}$ is searched, §5 of `data_memo_theory.md`, already the memo's
own correct framing elsewhere), not a hole in $\mathcal{X}$.

### Row 4 — "Oracle defines Dirichlet boundary data $\eta(x)=1$ on $\partial\Omega$"

> *"$\eta(x)=1,\ x\in\partial\Omega$ (oracle fires with certainty on the
> threshold)"* (DIML, `data_memo_theory.md` §4.4, pinned).

**Verdict: in tension with the memo's own later addendum.** A separate
passage in the same document states instead:

> *"$\partial\Omega=\{x\in\mathcal{X}:\eta(x)=0.5\}$"* (DIML,
> `data_memo_theory.md` §5.A, pinned).

These describe two different sets in general. The oracle's own transition
locus (where $f^*$ jumps from 0 to 1) is $\partial\Omega$ by definition of
$\Omega$; the learned model's $c=0.5$ contour $L_{0.5}$ is a level set of the
*smooth relaxation* $\hat\eta$, and need not coincide with $\partial\Omega$
unless $\hat\eta$ happens to be calibrated to reproduce $f^*$ exactly at the
hard threshold. Reconciled reading: $\partial\Omega$ is where the *oracle's
own* Dirichlet-type data (`§4.4`'s $\eta=1$, or more precisely — since a
hard indicator has no smooth boundary trace to speak of — the locus where
$f^*$ transitions) sits, while $L_{0.5}$ is a candidate operating point for
the *learned, soft* $\hat\eta$; treating the two as the same set (as §4.4
and §5.A do implicitly, without flagging the difference) is the imprecision
this document corrects.

### Row 5 — "Tikhonov/RKHS penalty is minimized by the harmonic extension"

> *"The unique minimizer of $\int_\Omega\|\nabla\eta\|^2\,dx$ subject to
> Dirichlet conditions on $\partial\Omega$ is the harmonic extension — a
> smooth, unique solution to $\Delta\eta=0$ in $\Omega^\circ$"* (DIML,
> `data_memo_theory.md` §4.4, pinned).

**Verdict: rigorous for the idealized variational problem; heuristic as a
description of actual training.** The classical Dirichlet principle (fixed
boundary trace, minimize Dirichlet energy over $H^1(\Omega)$ $\Rightarrow$
unique harmonic minimizer) is standard potential theory and is correctly
stated as an idealization — reusing `01` §9.6's weak-derivative/Sobolev-space
vocabulary, the minimizer lives in $H^1(\Omega)$ with prescribed trace on
$\partial\Omega$. But real Tikhonov/RKHS-regularized ERM (as DIML actually
implements it, per its own `MLNetPipeline.md`) optimizes a **data-fit term
plus a penalty over a specific parametric hypothesis class**
($\mathcal{H}_\Omega$, `data_memo_theory.md` §9.1), not literally this
boundary-constrained variational problem over all of $H^1(\Omega)$ — no
boundary trace is enforced during training at all. The gap between the
idealization and the implementation is real and is exactly what the memo's
own honesty caveat (Row 7 below) already concedes.

### Row 6 — "Level sets are $(d{-}1)$-dimensional contours"

> *"each level set $L_c$ represents a $(d-1)$ hypersurface... a contour of
> harvest urgency"* (DIML, `data_memo_theory.md` §5.A, pinned).

**Verdict: now rigorous, with an a.e.-in-$c$ qualifier the memo omits.** By
the coarea formula (`notes/07` Theorem 3.3, applicable since $\hat\eta$ is
at least Lipschitz as a trained GBT/logistic model's output composed with
standard feature maps), $L_c$ is $(d{-}1)$-rectifiable ($d=15$ here) for
$\mathcal{L}^1$-almost every $c\in[0,1]$ — not necessarily for *every* $c$
(a level set at a critical value can degenerate). The qualifier is a small
but genuine addition.

### Row 7 — "The formal machinery was never operationalized"

> *"the formal machinery — currents as dual to differential forms, the de
> Rham complex, the Hodge decomposition — was never operationalized in code
> or computed numerically. It is honest to call this scaffolding that
> shaped the conceptual design but did not enter the implementation."*
> (DIML, `data_memo_theory_part2.md`, line 327, pinned — quoted verbatim,
> directly re-verified against the source file during the writing of this
> document.)

**Verdict: acknowledged, and now partially closed.** This document supplies
rigor for the pieces that are genuinely justifiable (Rows 2, 5, 6, and the
new §2 proof), corrects the pieces that over-claimed (Rows 1, 3, 4), and is
explicit that this closes the loop at the level of **justification**, not
**numerical computation** — nothing here has been run against DIML's actual
data or model outputs; that remains open (§4).

---

## 4. What Is Rigorously Added, and What Remains Open

**Now rigorously established, using this repo's own machinery:** $\Omega_0$
(the empirically-realized-range restriction of $\Omega$) has finite
perimeter, with an explicit facet-sum formula for $P(\Omega_0)$ (§2); the
generalized-Stokes/boundary-by-duality identity for $\Omega$ holds, justified
through the corner-case adaptation of §2 rather than the smooth case; the
Dirichlet-energy/harmonic-extension claim is a correct statement about an
idealized variational problem (Row 5); level sets are rectifiable for a.e.
threshold (Row 6); and the cohomological over-claim (Row 3) and the
boundary-data inconsistency (Row 4) are identified precisely rather than
silently inherited.

**Genuinely open, flagged as PhD-direction leads rather than claims made
here:** whether the facet-sum perimeter of $\Omega_0$ (§2) correlates with
anything empirically useful (e.g. as a regularization diagnostic, or a
complexity measure for the oracle-constrained hypothesis class
$\mathcal{H}_\Omega$); whether the varifold/kernel-metric machinery of `14`
would improve robustness of the learned decision boundary to the label noise
DIML's own memo identifies (`data_memo_theory.md` §5.3, the oracle-vs-true
misspecification gap); and whether optimal-transport tools (`11`) could give
a principled distance between the feature-cloud distributions across
different simulation windows (directly relevant given the 2yr$\to$20yr data
drift noted in §0.4) — none of these are claims this document makes, only
directions the rigor built here makes newly askable with some precision.

---

## 5. A Note on Tone

Both GMT-topology and DirectIndexing_ML are the same author's own work, and
this document's corrections (Rows 1, 3, 4) are offered in that spirit: the
original memo was self-aware enough to flag its own GMT section as
unoperationalized scaffolding (Row 7) — that is precisely the invitation
this capstone takes up. Nothing here is a critique of the ML project's
empirical results (which this document does not touch); it is a rigor pass
on a theoretical appendix the project's own author already marked as
provisional.

---

## References Used

- **`notes/07_rectifiability_and_hausdorff_measure.md`** Theorem 3.1
  (linear-map area formula, used in §2 Step 1's dimension-counting lemma),
  Theorem 3.3 (coarea formula, used in Row 6).
- **`notes/08_currents.md`** Definition 2.1, Theorem 2.2, 2.3 (boundary by
  duality, $\partial\partial=0$, $\partial T_M=T_{\partial M}$; used in Row 2).
- **`notes/09_varifolds_and_finite_perimeter.md`** Theorem 2.3 (De Giorgi
  structure theorem, the core input to §2's anchor proof), Theorem 3.1 (the
  smooth-boundary case this document's §2 directly generalizes to corners).
- **`notes/05_topology_and_de_rham_cohomology.md`** Theorem 3.1 (Poincaré
  Lemma: $H^k_{\mathrm{dR}}$ vanishes on star-shaped, in particular convex,
  domains — the general statement needed for Row 3, not the narrower
  $\mathbb{R}^n$-only Corollary 3.2).
- **`01_prerequisite_systematic_review_for_GMT_Topology.md`** §9.6 (weak
  derivatives/Sobolev-space vocabulary, used in Row 5).
- **DIML** (`ieatyoursushi/DirectIndexing_ML`), pinned 2026-07-01 @ `7a15f09`:
  `src/Core/Oracle/OracleBoundary.cs`; `src/Core/Simulation/SoftLabelBuilder.cs`;
  `DataMemo/data_memo_theory.md` §4.2–4.4, §5.A, §9.1, §10 (lines 375–380);
  `DataMemo/data_memo_theory_part2.md` (line 327). All quotations above were
  directly read and verified against these pinned source files.

Full bibliographic data for pure-math citations: `notes/NOTATION.md` §4; the
`DIML` convention: `ml-geometry/NOTATION_ML.md` §4.

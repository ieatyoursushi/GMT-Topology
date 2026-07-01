# Phase 3 Hub — Geometric Machine Learning

**Cross-references.** `ml-geometry/NOTATION_ML.md` (typing conventions,
citation key, `DIML` cross-repo convention — read first); `notes/NOTATION.md`
(base typing context, house style, depth scale — inherited); `00_differential_geometry_foundations_for_GMT_Topology.md`
and `01_prerequisite_systematic_review_for_GMT_Topology.md` (Phase 1: smooth
DG); `notes/02`–`09` (Phase 2: the GMT bridge). This document is the
survey/hub for **Phase 3**: the intersection of differential geometry/GMT
and machine learning.

---

## 0. Standing Definitions

This document uses `ml-geometry/NOTATION_ML.md` §1's typing context
throughout, plus everything inherited from `notes/NOTATION.md` §2. No new
objects are introduced here — this is a survey-altitude map, in the same
role `00` and `01` play for Phases 1–2. No anchor proof is carried in this
document, matching that precedent exactly; proofs live in the six deep dives
`10`–`15` and the capstone `16`.

---

## 1. What Phase 3 Is, and What It Is Not

Phases 1 and 2 of this repo cover settled classical mathematics: smooth
differential geometry and its measure-theoretic generalization (GMT) are
each over a century old (curvature: 19th century; Hausdorff measure,
currents, varifolds: mid-20th century), with canonical textbook treatments
and no serious open disagreement about the core definitions and theorems.

Phase 3 is different in kind. The intersection of geometry and machine
learning is a **genuinely young, fast-moving research area** — most of the
material below postdates 2013, much of it postdates 2019, and some of the
field's own organizing frameworks (e.g. the "5 G's" blueprint cited in `12`)
are recent proposals, not settled consensus. This matters for how to read
what follows:

> Phase 3 is **PhD-direction reconnaissance**, not a settled canon. Each deep
> dive anchors on a stable monograph or survey where one exists, and treats
> individual conference-paper citations as illustrative examples that may be
> superseded, reframed, or simply forgotten within a few years — this is
> stated explicitly in each document rather than left for the reader to
> discover.

What Phase 3 *is*: two directions of interaction, both represented below.

- **Geometry *of* ML**: parameter spaces, loss landscapes, and optimization
  trajectories are themselves geometric objects (a statistical manifold with
  the Fisher–Rao metric; a constrained optimization domain that is a
  Riemannian manifold).
- **ML *of* geometry**: learning tasks whose *inputs* are geometric objects
  in the sense Phases 1–2 already built machinery for (point clouds and
  shapes as rectifiable sets/currents/varifolds; graphs and grids as domains
  with symmetry groups; topological invariants of data as features).

---

## 2. The Six-Topic Survey

| Topic | Core geometric object | Core ML question | Anchor citation | File | Reuses from Phases 1–2 |
|---|---|---|---|---|---|
| Information geometry | Fisher–Rao metric $g^F$ on a statistical manifold | How should parameter updates account for the geometry of the model class, not just Euclidean distance? | Amari 1998; Amari, IG | `10` | `notes/04` §1.1 ($\sharp$) |
| Optimal transport | Wasserstein distance $W_p$ between measures | How do we compare two probability distributions geometrically, and compute that comparison efficiently? | Villani, OT; Peyré–Cuturi | `11` | `notes/06` (measures), `notes/07` (Lipschitz) |
| Geometric deep learning | Group action $\rho$, equivariant map $\Phi$ | How does symmetry (permutation, translation, rotation) constrain and simplify a learnable function? | Bronstein et al. 2021 | `12` | `notes/02` (tangent spaces, at recognition depth) |
| Riemannian optimization | Manifold $M$, retraction $R_x$ | How do we optimize a function subject to a geometric constraint (orthogonality, low rank, positive-definiteness)? | Absil–Mahony–Sepulchre | `13` | `notes/04` ($\sharp$, geodesics, $\exp$), `notes/02` |
| GMT for shapes | Currents $D_k(\mathbb{R}^n)$, varifolds $V_k(\mathbb{R}^n)$ | How do we represent and compare 3D shapes/point clouds in a way that is robust to sampling and inconsistent orientation? | Charon–Trouvé 2013 | `14` | `notes/07`, `notes/08`, `notes/09` (direct, heavy reuse) |
| Topological data analysis | Persistence diagram $\mathrm{Dgm}$ | What robust, multi-scale shape/hole information can be extracted from a point cloud or function? | Cohen-Steiner–Edelsbrunner–Harer 2007 | `15` | `notes/05` (homology backdrop), `notes/07` (Lipschitz, for stability) |

---

## 3. Dependency Diagram

```text
Phase 1 (00, 01)              Phase 2 (notes/02–09)
  smooth DG                     GMT bridge
       │                              │
       │  T_pM, ♯, ∇, exp             │  H^k, rectifiable, currents,
       │  (notes/02, notes/04)        │  varifolds, finite perimeter
       ▼                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                         Phase 3 (ml-geometry/)                   │
│                                                                   │
│  notes/04 §1.1 (♯) ───────► 10 information geometry               │
│                                     │                             │
│  notes/06, notes/07 (Lipschitz) ──► 11 optimal transport          │
│                                                                    │
│  notes/02 (tangent spaces) ───────► 12 geometric deep learning     │
│                                                                    │
│  notes/04 (♯, geodesics, exp) ────► 13 riemannian optimization      │
│                                                                    │
│  notes/07, 08, 09 (heavy reuse) ──► 14 gmt for shapes ──┐          │
│                                                          │          │
│  notes/05, notes/07 (Lipschitz) ──► 15 topological data analysis   │
│                                                          │          │
│                                                          ▼          │
│                              16 capstone: DirectIndexing_ML         │
│                              (closes the loop: notes/07–09 supply   │
│                               rigor for DIML's own GMT scaffolding) │
└─────────────────────────────────────────────────────────────────┘
```

---

## 4. The Capstone, Previewed

`16_capstone_directindexing_gmt.md` is a case study applying this repo's own
machinery to the author's separate personal ML research project,
**DirectIndexing_ML** (a tax-loss-harvesting decision pipeline; see
`ml-geometry/NOTATION_ML.md` §4 for the citation convention). That project's
own theory memo already sketches a GMT interpretation of its "oracle
boundary" — treating the harvest decision region as bounded by a
hypersurface, invoking currents and generalized Stokes, and framing
regularized training as an implicit Dirichlet problem — but its own author
is explicit that *"the formal machinery... was never operationalized in
code or computed numerically... scaffolding that shaped the conceptual
design but did not enter the implementation"* (quoted precisely, with
citation, in `16`). The capstone's job is to actually supply that rigor
using `notes/07`–`09`'s machinery (rectifiability, currents, finite
perimeter), and to be honest — via an explicit claim-by-claim ledger — about
which parts of the original memo's GMT framing are now rigorously justified,
which remain useful analogies, and which need a precise correction.

---

## 5. How to Read Phase 3

Suggested order: `ml-geometry/NOTATION_ML.md` $\to$ this hub $\to$ pick
topics `10`–`15` in any order that matches your interest (they are largely
independent of each other, though `14` is worth reading before `16`) $\to$
`16`. Each deep dive is self-contained enough to read on its own, given the
shared notation.

---

## References Used

No new citations in this survey document beyond those tabulated in §2; see
`ml-geometry/NOTATION_ML.md` §3 for full bibliographic data on every
citation key used throughout Phase 3.

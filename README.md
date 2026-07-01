# Manifolds Analyzed Through the Lens of Geometric Measure Theory

Personal, self-directed notes on geometric measure theory (GMT) in the
context of manifolds outside the realm of smoothness and differentiability —
similar in spirit to how the Lebesgue integral extends the reach of the
Riemann integral. Most content is inspired by the time I was introduced to PSTAT 126 (intro linear regression) but instead taught by a slightly more rigorous professor who went into measure theory, functional data analysis (mainly around Mercer's thm & Hilbert projections), continuous stochastic processes, machine learning methods via neural nets and manifolds. Content is directly inspired by the content taught in my PSTAT 126 (PSTAT 210, MATH 228B, PSTAT 221A, MATH 240B, PSTAT 213, PSTAT 220C, PSTAT 236 )

## How to read these notes

```text
00_differential_geometry_foundations_for_GMT_Topology.md   ← start here (survey/hub)
01_prerequisite_systematic_review_for_GMT_Topology.md       ← then this (prerequisite map/index)
notes/NOTATION.md                                            ← keep open as a reference throughout
notes/02_manifolds_and_tangent_spaces.md          ─┐
notes/03_differential_forms_and_stokes.md           │
notes/04_riemannian_metrics_and_curvature.md        │  Phase 2 deep dives, read in order;
notes/05_topology_and_de_rham_cohomology.md          │  each proves at least one
notes/06_measure_theory_bridge.md                    │  theorem from scratch
notes/07_rectifiability_and_hausdorff_measure.md      │
notes/08_currents.md                                  │
notes/09_varifolds_and_finite_perimeter.md          ─┘

ml-geometry/NOTATION_ML.md                                   ← Phase 3: extends notes/NOTATION.md
ml-geometry/00_geometric_machine_learning_hub.md              ← Phase 3 survey/hub
ml-geometry/10_information_geometry.md            ─┐
ml-geometry/11_optimal_transport.md                 │
ml-geometry/12_geometric_deep_learning.md           │  the geometry–ML intersection;
ml-geometry/13_riemannian_optimization.md           │  each proves ≥1 result from scratch
ml-geometry/14_gmt_for_shapes.md                     │
ml-geometry/15_topological_data_analysis.md         ─┘
ml-geometry/16_capstone_directindexing_gmt.md                 ← capstone: applies notes/07–09
                                                                 to the author's own ML project
```

`00` and `01` move fast, at survey altitude, covering the whole arc from
Euclidean calculus to the GMT bridge. `notes/02`–`09` are deep dives: each one
owns a single band of that arc, states its theorems with full hypotheses, and
proves at least one of them from scratch rather than citing it.

Phases 1–2 (`00`, `01`, `notes/`) cover settled classical mathematics —
smooth differential geometry and its measure-theoretic generalization are
each over a century old, with no serious open disagreement about the core
definitions and theorems. **Phase 3** (`ml-geometry/`) is different in kind:
it surveys the intersection of this machinery with machine learning, a
genuinely young and fast-moving research area, at the same rigor and typing
discipline but with the added honesty that individual results there may be
superseded as the field moves — it is PhD-direction reconnaissance, not a
settled canon. Its capstone applies the pure-math machinery of `notes/07`–`09`
to a separate personal ML research project, closing the loop on that
project's own (self-flagged, aspirational) geometric-measure-theory framing.

**Notational convention.** Every object in these notes is given an explicit,
redundant type signature at the moment it's introduced — domain, codomain,
and the type of every free symbol — rather than left to be inferred from
surrounding prose. `notes/NOTATION.md` states this convention once, along
with the shared typing context and citation key used throughout Phases 1–2;
`ml-geometry/NOTATION_ML.md` extends it for Phase 3's ML-specific objects and
citations.

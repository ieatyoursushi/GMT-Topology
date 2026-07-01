# Manifolds Analyzed Through the Lens of Geometric Measure Theory

Personal, self-directed notes on geometric measure theory (GMT) in the
context of manifolds outside the realm of smoothness and differentiability —
similar in spirit to how the Lebesgue integral extends the reach of the
Riemann integral.

## How to read these notes

```text
00_differential_geometry_foundations_for_GMT_Topology.md   ← start here (survey/hub)
01_prerequisite_systematic_review_for_GMT_Topology.md       ← then this (prerequisite map/index)
notes/NOTATION.md                                            ← keep open as a reference throughout
notes/02_manifolds_and_tangent_spaces.md          ─┐
notes/03_differential_forms_and_stokes.md           │
notes/04_riemannian_metrics_and_curvature.md        │  deep dives, read in order;
notes/05_topology_and_de_rham_cohomology.md          │  each proves at least one
notes/06_measure_theory_bridge.md                    │  theorem from scratch
notes/07_rectifiability_and_hausdorff_measure.md      │
notes/08_currents.md                                  │
notes/09_varifolds_and_finite_perimeter.md          ─┘
```

`00` and `01` move fast, at survey altitude, covering the whole arc from
Euclidean calculus to the GMT bridge. `notes/02`–`09` are deep dives: each one
owns a single band of that arc, states its theorems with full hypotheses, and
proves at least one of them from scratch rather than citing it.

**Notational convention.** Every object in these notes is given an explicit,
redundant type signature at the moment it's introduced — domain, codomain,
and the type of every free symbol — rather than left to be inferred from
surrounding prose. `notes/NOTATION.md` states this convention once, along
with the shared typing context and citation key used throughout.

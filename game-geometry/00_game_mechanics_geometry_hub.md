# Phase 5 Hub — Game-Mechanics Geometry

**Cross-references.** `game-geometry/NOTATION_GAME.md` (typing
conventions, citation key, the `ZT` pinning convention — read first);
`notes/NOTATION.md`, `ml-geometry/NOTATION_ML.md`,
`stochastic/NOTATION_STOCH.md` (inherited in full); `notes/02`, `notes/04`
(the charts/geodesics/musical-isomorphism machinery this phase runs
hardest); `notes/06`–`07` (measure, Lipschitz, area formula — the
phase's quiet workhorses); `ml-geometry/15` Lemma 4.1 (reused verbatim in
`32`); `ml-geometry/14` §3 (reused in `34`). This document is the
survey/hub for **Phase 5**: the mathematics running common Unity-engine
mechanics, with the author's own game **Zombtoy** (`ZT`, pinned
2026-07-19 @ `ce956b0`) woven through as the case study.

**Status.** Phase 5 is complete: this hub, `NOTATION_GAME.md`, and deep
dives `30`–`34`.

---

## 0. Standing Definitions

This document uses `game-geometry/NOTATION_GAME.md` §1's typing context
plus everything inherited from the three prior NOTATION files. No new
objects, and no anchor proof — survey altitude, per the hub precedent of
`00`/`01` and Phases 3–4; proofs live in `30`–`34`.

---

## 1. What Phase 5 Is, and What It Is Not

Phases 1–4 built machinery and pointed it at research areas. Phase 5
points it at a **running engine**: each deep dive owns a family of Unity
mechanics, states the mathematics they execute, and proves the
load-bearing facts at anchor depth. Two directions of reading, as
always:

- **Geometry running the engine**: `Quaternion.Slerp` *is* a geodesic of
  $S^3$; the shader's normal matrix *is* pullback of covectors; the
  navmesh planner *is* metric geometry on a polyhedral surface; the
  renderer *is* Lebesgue integration against $\mathcal{H}^2$.
- **The engine as a geometry lab**: gimbal lock is the cleanest chart
  singularity in the repository; the non-uniform-scale lighting bug is a
  type error the `notes/NOTATION.md` §1.7 discipline catches; frame-rate
  dependent lerp smoothing is a discretized-flow error term made
  playable.

What Phase 5 is *not*: a game-engine tutorial, or new mathematics. It is
the repo's oldest mathematics — the register note in `NOTATION_GAME.md`
§5 — doing shift work, cited two-tier: settled math by the classic keys,
versioned engineering by access date and pinned commit.

### 2.1 The Wager, Adjudicated ("more Riemannian than Lebesgue?")

The phase was commissioned with a guess: game math should be *more
Riemannian than Lebesgue*. Scorecard, by deep dive:

| File | Dominant machinery | Side |
|---|---|---|
| `30` rotations/slerp | Lie group $S^3\to SO(3)$, geodesics, charts | **Riemannian** ✓ |
| `31` transforms/cameras | affine/projective geometry, covector pullback | linear-algebraic (the `notes/02` layer; neither, honestly) |
| `32` collision/SDFs | closed sets, 1-Lipschitz distance functions, projection theorems, a.e. gradients | **Lebesgue/GMT** ✗ |
| `33` navigation | metric geometry, and curvature condensed into a **Radon measure** on the vertex set | **Lebesgue/GMT** ✗ |
| `34` rendering | $\sigma = \mathcal{H}^2\llcorner S^2$, the area formula, Monte Carlo on empirical measures | **Lebesgue** ✗ |

Verdict: the *control* layer of a game (attitude, cameras, blending) is
Riemannian exactly as guessed — but the *world* layers (collision,
navigation, light) run on measure theory and Lipschitz analysis, and the
single most-used identity in rendering ($\cos\theta/r^2$) is literally
`notes/07`'s area-formula anchor. Roughly half the engine is Lebesgue in
a trench coat; the surprise is delivered in `32` §2's type-check, `33`
§4's curvature-as-measure box, and `34` §2.

---

## 2. The Five-Topic Survey

| Topic | Core objects | Unity mechanics owned | Anchor proofs (depth C) | File | ZT hooks (pinned @ `ce956b0`) |
|---|---|---|---|---|---|
| Rotations | $S^3\to SO(3)$, slerp, Euler charts | LookRotation, Slerp/Lerp, pitch clamp, lerp smoothing | $R_q$ rotation + kernel $\{\pm1\}$ + Rodrigues; slerp $=\exp_{q_0}(t\Omega w)$ | `30` | PlayerMovement.Turning(); CameraMovement pitch clamp; CamerFollow smoothing |
| Transforms & cameras | $SE(3)$/Aff(3), $\mathbb{RP}^3$, covectors | Transform hierarchy, ScreenPointToRay, perspective↔ortho | normals transform by $A^{-\top}$; the screen-ray derivation | `31` | Turning()'s floor raycast; CamerPOV perspective switch; frame-relative FP movement |
| Collision & bodies | $d_S$, SDFs, convexity, inertia | Raycast, OverlapSphere, convex narrow phase | sphere-tracing correctness; separating hyperplane via projection thm | `32` | PlayerShooting hitscan; Rocket two-tier blast (+ the speed² flag) |
| Navigation | polyhedral $\Sigma$, $A^*$, angle defect | NavMesh baking, NavMeshAgent | $A^*$ optimality; discrete Gauss–Bonnet $\sum\delta = 2\pi\chi$ | `33` | EnemyMovement chase loop, isOnNavMesh guard |
| Rendering & sampling | $\sigma=\mathcal{H}^2\llcorner S^2$, MC estimators | lights/attenuation, shadows, spawning | $\cos\theta/r^2$ via the area formula; MC unbiasedness/variance/optimal proposal | `34` | flashlight cone; SpawnClown sampling |

---

## 3. Dependency Diagram

```text
Phase 1–2 (notes/)                          Phases 3–4
  charts, T_pM, pullback (02)                 15 Lem 4.1 (1-Lipschitz d_S)
  geodesics, exp, ♯/♭ (04)                    14 §3 (mesh currents)
  ℋ^k, Lipschitz, area formula (06, 07)       20 §2 (L² estimators), 01 §13.3
       │                                            │
┌──────▼────────────────────────────────────────────▼───────────────────┐
│                        Phase 5 (game-geometry/)                        │
│                                                                        │
│  04 §3 (geodesics) ──────────► 30 rotations & slerp                    │
│  02 §6 (pullback), 04 §1.1 ──► 31 transforms & cameras                 │
│  15 Lem 4.1, 01 §4/§9 ───────► 32 collision & SDFs ──► 33 navigation   │
│  06 §5, 07 §3 (area formula) ► 34 rendering & Monte Carlo              │
│                                                                        │
│  woven case study: ZT (Zombtoy), pinned @ ce956b0 — every deep dive    │
│  carries a ZT section; the mechanics cited are read, not imagined      │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 4. The ZT Weave, and What Stays Open

Unlike Phase 3, this phase carries no separate capstone: `ZT` is woven
per-document (the Phase-4 finance pattern), with the pinning and
file+method citation rules of `NOTATION_GAME.md` §4. Two flags raised
along the way, in the `16` claim-ledger spirit:

- `32` §5: at the pinned commit, `Rocket.cs` applies its speed twice —
  realized projectile velocity scales as $\text{speed}^2$; a one-line
  normalization restores linear tuning.
- `30` §5: `CamerFollow.cs`'s `Lerp(a, b, smoothing·Δt)` is frame-rate
  dependent, with the exact $e^{-\lambda\Delta t}$ replacement derived.

**Open leads (not claims):** a `ZT` capstone in the style of
`ml-geometry/16` — a claim-by-claim pass once the in-progress refactor
(`*Refactored.cs`) settles, checking the movement/attitude code against
`30`–`31` and the blast/damage kernels against `32`/`34`; and, further
out, the multiplayer direction on `ZT`'s roadmap raises state
interpolation across a network — slerp and `30` §5's flows under delay
and jitter, a genuinely nice extension problem.

---

## 5. How to Read Phase 5

Suggested order: `NOTATION_GAME.md` $\to$ this hub $\to$ `30` $\to$ `31`
(they share the attitude/transform thread) $\to$ `32` $\to$ `33` (the
world thread; `33` §1 leans on `32` §3) $\to$ `34` (pulls in everything).
Readers here for the wager can go straight to `32` §2, `33` §4, and `34`
§2 — the three places the Lebesgue half lands. Readers here from the
engine side can enter through any deep dive's "Unity mechanics this
document owns" header and follow cross-references backward into
Phases 1–2.

---

## References Used

No new citations beyond those tabulated in §2; see
`game-geometry/NOTATION_GAME.md` §3–§4 for full bibliographic data and
the `ZT` pinning statement.

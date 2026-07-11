# Phase 4 Hub — Stochastic Analysis Meets the Geometry

**Cross-references.** `stochastic/NOTATION_STOCH.md` (typing conventions,
citation key, maturity ladder — read first); `notes/NOTATION.md` and
`ml-geometry/NOTATION_ML.md` (inherited in full); `00` §21 (Laplace–Beltrami,
heat equation — the single most reused Phase-1 section below); `notes/06`
(measure theory, DCT); `01` §13 (the probability/statistics bridge this
phase completes); `ml-geometry/10` (score), `ml-geometry/11` (optimal
transport, Sinkhorn), `ml-geometry/16` (the capstone whose GBM typing gap
this phase closes).

**Status.** Phase 4 is a **skeleton**: this hub and `NOTATION_STOCH.md`
exist; the four deep dives (`20`–`23`) are planned but not yet written.
Every "(planned)" marker below is a commitment of scope, not a citation to
an existing document.

---

## 0. Standing Definitions

This document uses `stochastic/NOTATION_STOCH.md` §1's typing context
throughout, plus everything inherited from `notes/NOTATION.md` §2 and
`ml-geometry/NOTATION_ML.md` §1. No new objects are introduced here, and no
anchor proof is carried in this document — matching the precedent of `00`,
`01`, and the Phase-3 hub exactly; proofs will live in the deep dives.

---

## 1. What Phase 4 Is, and What It Is Not

Phases 1–2 built calculus on smooth and then merely-measurable spaces.
Phase 3 pointed that machinery at machine learning. Phase 4 adds the
remaining axis this repo's own prerequisite review flagged from the start:
**time and randomness**. `01` §2's "parallel statistical/probabilistic
line" and `01` §13's probability bridge stop exactly where continuous-time
processes begin; this phase starts there.

As with Phase 3, there are two directions of interaction:

- **Stochastics *of* geometry**: random processes as geometric objects —
  Brownian motion's generator is $\tfrac12\Delta$ (half the Laplacian `00`
  §21 already built); diffusion semigroups are heat flow; Brownian motion
  on a manifold is driven by $\Delta_g$ (Hsu, at recognition depth).
- **Geometry *of* stochastics**: spaces of random objects carry geometry —
  the Fokker–Planck equation is a *gradient flow* in the Wasserstein
  geometry of `ml-geometry/11` (JKO/Otto); the Schrödinger bridge is
  entropic OT (`ml-geometry/11` §4) in disguise; a path's signature is a
  sequence of `notes/03`-style iterated integrals.

**What Phase 4 is not:** a standalone mathematical-finance phase. The
distinctively geometric finance material is *woven in* rather than given
its own phase — see §4 for why.

Unlike Phase 3 (uniformly young), Phase 4 spans a **maturity ladder** —
from Itô calculus (settled classical, Phase-2 register) to score-based
diffusion models (2020s, Phase-3 reconnaissance register). The ladder is
tabulated once in `NOTATION_STOCH.md` §4; each deep dive will state its
rung.

---

## 2. The Four-Topic Survey (planned)

| Topic | Core object | Core question | Anchor citation | File | Reuses from Phases 1–3 |
|---|---|---|---|---|---|
| Brownian motion & Itô calculus | $B_t$, $[X]_t$, $\int H\,dB$ | What replaces the chain rule when paths are continuous but nowhere differentiable? | Karatzas–Shreve; Le Gall | `20` *(planned)* | `notes/06` (measure, DCT), `01` §13.9 ($L^2$ geometry), `00` §21 (heat), `notes/07` (the a.e.-differentiability contrast) |
| Diffusions & Wasserstein gradient flows | $\mathcal{L}$, $\rho_t$, $W_2$ | In what geometry is diffusion the steepest descent of entropy? | JKO 1998; AGS | `21` *(planned)* | `ml-geometry/11` ($W_p$, Sinkhorn$\to$Schrödinger bridge), `00` §21 ($\Delta_g$), `ml-geometry/13` (gradient flows) |
| Score-based diffusion models | forward/reverse SDE, $\nabla_x\log p_t$ | How does learning the score turn a diffusion into a generative sampler? | Song et al. 2021 (anchored on Anderson 1982) | `22` *(planned)* | `ml-geometry/10` (score), `20`, `21` |
| Path signatures & rough paths | $S(X)$, Chen's identity | What feature map linearizes continuous functions of a path? | Lyons 1998; Friz–Hairer | `23` *(planned)* | `notes/03` (iterated line integrals of 1-forms), `01` §13 (time-series/ML bridge) |

Candidate anchor proofs, one per deep dive (subject to revision when
written): the **Itô isometry** from scratch (`20`); the **one-step JKO
minimizer exists and is unique** via the direct method, reusing
`ml-geometry/11` Proposition 4.2's strict-convexity pattern (`21`); the
**exactness of the reverse-time SDE for a finite-state or
Ornstein–Uhlenbeck special case**, where Anderson's identity can be
verified by direct computation (`22`); **Chen's identity**
$S(X * Y) = S(X)\otimes S(Y)$ for concatenation of BV paths (`23`).

---

## 3. Dependency Diagram

```text
Phase 1 (00, 01)           Phase 2 (notes/)            Phase 3 (ml-geometry/)
  Δ_g, heat eq (00 §21)      measure, DCT (06)           score s_θ (10)
  L² projection (01 §13)     Lipschitz/a.e. (07)         W_p, Sinkhorn (11)
       │                          │                       gradient descent (13)
       ▼                          ▼                            │
┌──────────────────────────────────────────────────────────────▼──────────┐
│                        Phase 4 (stochastic/) — planned                    │
│                                                                            │
│  01 §13.9 (L²) ── 06 (measure) ──► 20 Brownian motion & Itô calculus       │
│                                        │  (GBM: closes 16 §0.3's gap)      │
│  00 §21 (Δ_g) ── 11 (W₂) ─────────► 21 diffusions & Wasserstein flows      │
│                                        │  (Sinkhorn ↔ Schrödinger bridge)  │
│  10 (score) ──── 20, 21 ──────────► 22 score-based diffusion models        │
│                                                                            │
│  notes/03 (line integrals) ───────► 23 signatures & rough paths            │
└────────────────────────────────────────────────────────────────────────────┘
```

---

## 4. Where Finance Lives in This Phase

A standalone finance phase would collide with `ml-geometry/16`, which
already owns this repo's finance application (DIML). Instead, the
distinctively geometric finance material is placed where its mathematics
lives:

- **`20`**: GBM (typed in `NOTATION_STOCH.md` §1.3) and first-passage
  machinery — directly rigorizing the $\tilde y_{GBM}$ soft-label
  construction of `ml-geometry/16` §0.3; the Black–Scholes PDE as a
  boundary-value cousin of the heat equation (`00` §21), via Itô's formula
  (Black–Scholes 1973; Shreve, SCF II).
- **`21`**: the Schrödinger bridge / martingale-OT corner of mathematical
  finance, at recognition depth (Léonard 2014) — the same entropic-OT
  object as `ml-geometry/11` §4's Sinkhorn iteration.
- Stochastic portfolio theory (Fernholz) is flagged as an optional lead,
  not scoped: genuinely geometric, but niche relative to the four topics
  above.

---

## 5. The DIML Patch, Previewed

`ml-geometry/16` §0.3 uses 200 simulated GBM paths to define
$\tilde y_{GBM}$ — a *first-passage probability* — with GBM itself never
defined in this repository. `NOTATION_STOCH.md` §1.3 now carries the typed
definition, and `20` is scoped to carry the mathematics: existence and the
closed form $S_t = S_0\exp((\mu-\sigma^2/2)t+\sigma B_t)$ as an Itô-formula
computation, plus what can and cannot be said about first-passage
probabilities through the oracle's gate structure. When `20` lands, `16`
§0.3 should gain a cross-reference to it, closing the last untyped
primitive in the capstone.

---

## 6. How to Read Phase 4

Suggested order, once the deep dives exist: `NOTATION_STOCH.md` $\to$ this
hub $\to$ `20` $\to$ `21` $\to$ `22` (which depends on both) $\to$ `23`
(independent of `21`–`22`; needs only `20` and `notes/03`). Until then,
`NOTATION_STOCH.md` §1 is usable on its own as the typed vocabulary — in
particular §1.3's GBM definition, which `ml-geometry/16` already needs.

---

## References Used

No new citations beyond those tabulated in §2 and §4; see
`stochastic/NOTATION_STOCH.md` §3 for full bibliographic data on every
Phase-4 citation key.

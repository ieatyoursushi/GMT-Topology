# Phase 4 Hub — Stochastic Analysis Meets the Geometry

**Cross-references.** `stochastic/NOTATION_STOCH.md` (typing conventions,
citation key, maturity ladder — read first); `notes/NOTATION.md` and
`ml-geometry/NOTATION_ML.md` (inherited in full); `00` §21 (Laplace–Beltrami,
heat equation — the single most reused Phase-1 section below); `notes/06`
(measure theory, DCT); `01` §13 (the probability/statistics bridge this
phase completes); `ml-geometry/10` (score), `ml-geometry/11` (optimal
transport, Sinkhorn), `ml-geometry/16` (the capstone whose GBM typing gap
this phase closes).

**Status.** Phase 4 is **complete**: this hub, `NOTATION_STOCH.md`, and
the four deep dives `20`–`23`, each carrying its anchor proofs and its
finance/options-practitioner weave. The phase was built map-first (`01`
§1): notation and hub came first, deep dives followed.

---

## 0. Standing Definitions

This document uses `stochastic/NOTATION_STOCH.md` §1's typing context
throughout, plus everything inherited from `notes/NOTATION.md` §2 and
`ml-geometry/NOTATION_ML.md` §1. No new objects are introduced here, and no
anchor proof is carried in this document — matching the precedent of `00`,
`01`, and the Phase-3 hub exactly; proofs live in the deep dives `20`–`23`.

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

## 2. The Four-Topic Survey

| Topic | Core object | Core question | Anchor citation | File | Reuses from Phases 1–3 |
|---|---|---|---|---|---|
| Brownian motion & Itô calculus | $B_t$, $[X]_t$, $\int H\,dB$ | What replaces the chain rule when paths are continuous but nowhere differentiable? | Karatzas–Shreve; Le Gall | `20` | `notes/06` (measure, DCT), `01` §13.9 ($L^2$ geometry), `00` §21 (heat), `notes/07` (the a.e.-differentiability contrast) |
| Diffusions & Wasserstein gradient flows | $\mathcal{L}$, $\rho_t$, $W_2$ | In what geometry is diffusion the steepest descent of entropy? | JKO 1998; AGS | `21` | `ml-geometry/11` ($W_p$, Sinkhorn$\to$Schrödinger bridge), `00` §21 ($\Delta_g$), `ml-geometry/13` (gradient flows) |
| Score-based diffusion models | forward/reverse SDE, $\nabla_x\log p_t$ | How does learning the score turn a diffusion into a generative sampler? | Song et al. 2021 (anchored on Anderson 1982) | `22` | `ml-geometry/10` (score), `20`, `21` |
| Path signatures & rough paths | $S(X)$, Chen's identity | What feature map linearizes continuous functions of a path? | Lyons 1998; Friz–Hairer | `23` | `notes/03` (iterated line integrals of 1-forms), `01` §13 (time-series/ML bridge) |

Anchor proofs as delivered: the **Itô isometry** (`20` Theorem 2.1; plus
quadratic variation, the GBM closed form, and the reflection principle in
the same document); the **free-energy dissipation identity** (`21` Theorem
2.1) together with **Breeden–Litzenberger** (`21` Theorem 5.1); the
**denoising-score-matching identity** and the **OU transition law** (`22`
Theorem 3.1, Proposition 2.1), with Anderson's reverse SDE verified
directly on stationary OU; and **Chen's identity** (`23` Theorem 3.1, plus
the tensor-exponential and reparametrization-invariance anchors). One
honest swap against the original scoping: the one-step JKO
existence/uniqueness anchor first planned for `21` needs AGS-level
lower-semicontinuity machinery this repo has not built, so JKO is stated
and cited at recognition depth (`21` §3) and the dissipation identity
carries that document's anchor weight instead.

---

## 3. Dependency Diagram

```text
Phase 1 (00, 01)           Phase 2 (notes/)            Phase 3 (ml-geometry/)
  Δ_g, heat eq (00 §21)      measure, DCT (06)           score s_θ (10)
  L² projection (01 §13)     Lipschitz/a.e. (07)         W_p, Sinkhorn (11)
       │                          │                       gradient descent (13)
       ▼                          ▼                            │
┌──────────────────────────────────────────────────────────────▼──────────┐
│                          Phase 4 (stochastic/)                            │
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

- **`20`** §5–§6: the delta-hedging derivation of the Black–Scholes PDE
  and its reduction to `00` §21's heat equation; implied volatility as
  the market's coordinate system; barrier/first-passage laws (reflection
  principle, inverse-Gaussian barrier formula), directly rigorizing the
  $\tilde y_{GBM}$ soft label of `ml-geometry/16` §0.3 (Black–Scholes
  1973; Merton 1973; Shreve, SCF II).
- **`21`** §4–§5: the volatility surface as a database of risk-neutral
  densities — Breeden–Litzenberger proved, Dupire's local-volatility
  forward equation as Fokker–Planck read in contract variables, and
  martingale OT as `ml-geometry/11`'s couplings with a hedging constraint
  (Breeden–Litzenberger 1978; Dupire 1994; BHLP 2013); the Schrödinger
  bridge as the stochastic face of entropic OT (Léonard 2014; CGP 2021).
- **`22`** §6 and **`23`** §6: deep hedging and generative market
  simulators (flagged current; Buehler et al. 2019); rough volatility and
  signature-based pricing, hedging, execution, and calibration (GJR 2018;
  BFG 2016; LNP 2020; KLP 2020; Cuchiero et al. 2023, 2025).
- Stochastic portfolio theory (Fernholz) remains flagged as an optional
  lead, not scoped: genuinely geometric, but niche relative to the four
  topics above.

---

## 5. The DIML Patch, Previewed

`ml-geometry/16` §0.3 uses 200 simulated GBM paths to define
$\tilde y_{GBM}$ — a *first-passage probability* — and GBM was, until this
phase, never defined in this repository. The patch is now in three parts:
`NOTATION_STOCH.md` §1.3 carries the typed definition; `20` Theorem 4.1
solves the process (the closed form as an Itô-formula computation, with
uniqueness); and `20` §6.2 settles what is analytically available — the
oracle's one-gate idealization has an exact inverse-Gaussian closed form
(usable as a unit test on DIML's simulator), while the four-gate,
path-dependent oracle provably has no such form, so `16` §0.3's Monte
Carlo is justified rather than merely convenient. `16` §0.3
cross-references all of this; no untyped primitive remains in the
capstone.

---

## 6. How to Read Phase 4

Suggested order: `NOTATION_STOCH.md` $\to$ this hub $\to$ `20` $\to$ `21`
$\to$ `22` (which depends on both) $\to$ `23` (independent of `21`–`22`;
needs only `20` and `notes/03`). The finance thread reads in the same
order: Black–Scholes and barriers (`20` §5–§6) $\to$ the volatility
surface as densities (`21` §5) $\to$ simulators and deep hedging (`22`
§6) $\to$ rough volatility and signature trading (`23` §6).

---

## References Used

No new citations beyond those tabulated in §2 and §4; see
`stochastic/NOTATION_STOCH.md` §3 for full bibliographic data on every
Phase-4 citation key.

# 21 — Fokker–Planck, Free Energy, and Wasserstein Gradient Flows

**Cross-references.** `stochastic/NOTATION_STOCH.md` (typing §1.3–§1.4,
§1.7, citation key §3); `stochastic/20` (Itô's formula, the martingale
property — both used in §1); `ml-geometry/11` ($W_p$, Kantorovich duality,
entropic OT/Sinkhorn — §3–§4 build directly on it); `00_differential_geometry_foundations_for_GMT_Topology.md`
§21 (Laplacian, heat equation); `ml-geometry/10` §0 (Fisher information —
§2's dissipation rate is its location-family cousin); `notes/06` Theorem
4.3 (DCT — the justification pattern for every differentiation under an
integral below); feeds `stochastic/22` (forward/reverse diffusions).

**Register (maturity ladder, `NOTATION_STOCH.md` §4).** The
Fokker–Planck/generator material of §1–§2 is settled classical. The
Wasserstein-gradient-flow reading (§3) is settled-with-a-monograph
(JKO 1998 $\to$ AGS 2008): the theorems are firm, the viewpoint still
actively producing research. §4 (Schrödinger bridges) and the
martingale-OT corner of §5 are current.

---

## 0. Standing Definitions

Everything from `stochastic/NOTATION_STOCH.md` §1.3–§1.4 and §1.7. This
document works with the **gradient-form SDE**

$$
dX_t = -\nabla V(X_t)\,dt + \sqrt{2}\,dB_t, \qquad V \in C^\infty(\mathbb{R}^n) \ \text{a potential}, \qquad B \ d\text{-dimensional BM } (d=n),
$$

whose generator (`NOTATION_STOCH.md` §1.3) is $\mathcal{L}f = -\nabla
V\cdot\nabla f + \Delta f$.

**Standing regularity hypothesis** (the analogue of `ml-geometry/10` §0's,
stated once): all densities $\rho_t$ below are smooth, strictly positive,
with enough decay ($\rho_t$ and its derivatives dominated by an integrable
envelope, uniformly for $t$ in compacts) that (a) differentiation under
the integral sign is justified via `notes/06` Theorem 4.3, and (b)
integrations by parts over $\mathbb{R}^n$ carry no boundary terms.
Everything in §1–§2 is proved *under this hypothesis*; removing it is real
work (AGS, Ch. 10–11) and is exactly what the metric-space formulation of
§3 was invented for.

---

## 1. From the SDE to the Forward Equation

**Proposition 1.1 (Kolmogorov/Dynkin identity).** For $f\in
C_c^2(\mathbb{R}^n)$ and $X$ solving §0's SDE,

$$
\frac{d}{dt}\,\mathbb{E}[f(X_t)] = \mathbb{E}[\mathcal{L}f(X_t)].
$$

*Proof.* Itô's formula (`20` Theorem 3.1, in its $n$-dimensional version,
cited) gives

$$
f(X_t) = f(X_0) + \int_0^t \mathcal{L}f(X_s)\,ds + \sqrt2\int_0^t \nabla f(X_s)\cdot dB_s.
$$

Since $f\in C_c^2$, $\nabla f$ is bounded, so the Itô integral has finite
$L^2$-norm (`20` Theorem 2.1) and is a mean-zero martingale (`20`
Corollary 2.2). Taking expectations kills it:

$$
\mathbb{E}[f(X_t)] = \mathbb{E}[f(X_0)] + \int_0^t \mathbb{E}[\mathcal{L}f(X_s)]\,ds,
$$

(the $ds$-integral and $\mathbb{E}$ swap by Fubini, `01` §8.5, using
boundedness of $\mathcal{L}f$), and differentiating in $t$ — the integrand
is continuous in $s$ — gives the claim. $\blacksquare$

**Corollary 1.2 (Fokker–Planck / Kolmogorov forward equation).** If
$\operatorname{Law}(X_t)$ has density $\rho_t$ (standing hypothesis), then

$$
\partial_t\rho_t = \mathcal{L}^*\rho_t = \nabla\cdot\big(\rho_t\nabla V\big) + \Delta\rho_t = \nabla\cdot\Big(\rho_t\,\nabla\big(\log\rho_t + V\big)\Big).
$$

*Proof.* Rewrite Proposition 1.1 as $\int f\,\partial_t\rho_t = \int
(\mathcal{L}f)\,\rho_t$ for all $f\in C_c^2$, and integrate by parts twice
on the right (no boundary terms; test function compactly supported):
$\int(-\nabla V\cdot\nabla f)\rho_t = \int f\,\nabla\cdot(\rho_t\nabla V)$
and $\int(\Delta f)\rho_t = \int f\,\Delta\rho_t$. Since $f$ is arbitrary,
the densities agree pointwise (both sides continuous). The last equality
in the display is the algebraic identity $\Delta\rho = \nabla\cdot(\rho
\nabla\log\rho)$, valid for $\rho>0$. $\blacksquare$

> **Type-check — one process, two PDEs.** The *backward* equation
> $\partial_t u = \mathcal{L}u$ propagates **observables** ($u(t,x) =
> \mathbb{E}[f(X_t)\mid X_0=x]$); the *forward* equation $\partial_t\rho =
> \mathcal{L}^*\rho$ propagates **densities**. They are adjoint to each
> other in exactly the sense currents are dual to forms (`notes/08` §2):
> the pairing $\int u\,\rho$ is what both equations preserve. For $V=0$
> the forward equation is the heat equation of `00` §21 — Brownian motion
> *is* heat flow, with $\mathcal{L} = \Delta$ here ($\sqrt2$-normalization
> absorbing the usual $\tfrac12$).

---

## 2. Anchor Proof: Free-Energy Dissipation

$$
\mathcal{F}(\rho) := \underbrace{\int_{\mathbb{R}^n} \rho\log\rho\ d\mathcal{L}^n}_{\text{(neg-)entropy } H(\rho)} \ +\ \underbrace{\int_{\mathbb{R}^n} V\rho\ d\mathcal{L}^n}_{\text{potential energy}}, \qquad \mathcal{F} : \{\text{densities}\}\to\mathbb{R}.
$$

**Theorem 2.1 (dissipation identity).** Under the standing hypothesis,
along any solution of the Fokker–Planck equation (Corollary 1.2),

$$
\frac{d}{dt}\,\mathcal{F}(\rho_t) \ =\ -\int_{\mathbb{R}^n} \rho_t\,\big|\nabla(\log\rho_t + V)\big|^2\,d\mathcal{L}^n \ \le\ 0,
$$

with equality at time $t$ iff $\rho_t = Z^{-1}e^{-V}$ (the Gibbs density,
$Z = \int e^{-V}$, assumed finite).

*Proof.* Differentiate under the integral (standing hypothesis + `notes/06`
Theorem 4.3):

$$
\frac{d}{dt}\mathcal{F}(\rho_t) = \int \partial_t\rho_t\,\big(\log\rho_t + 1 + V\big)\,d\mathcal{L}^n.
$$

The constant $1$ contributes $\int\partial_t\rho_t = \frac{d}{dt}\int\rho_t
= 0$ (mass is conserved: $\partial_t\rho_t$ is a divergence, and the decay
hypothesis kills the flux at infinity). Substitute the divergence form of
Corollary 1.2 and integrate by parts once (again no boundary terms):

$$
\frac{d}{dt}\mathcal{F}(\rho_t) = \int \nabla\cdot\Big(\rho_t\nabla(\log\rho_t+V)\Big)\,(\log\rho_t+V)\,d\mathcal{L}^n = -\int \rho_t\,\big|\nabla(\log\rho_t+V)\big|^2\,d\mathcal{L}^n.
$$

Nonpositivity is manifest. Equality forces $\nabla(\log\rho_t+V) = 0$ on
$\{\rho_t>0\} = \mathbb{R}^n$ (strict positivity), i.e. $\log\rho_t + V$
constant, i.e. $\rho_t = Z^{-1}e^{-V}$ after normalizing. $\blacksquare$

**Corollary 2.2 (de Bruijn's identity).** For $V=0$ (heat flow),
$\frac{d}{dt}H(\rho_t) = -\int\rho_t|\nabla\log\rho_t|^2 =: -I(\rho_t)$:
heat flow dissipates entropy at exactly the rate of the **Fisher
information** of the current density.

> **Type-check — which "Fisher information."** $I(\rho) =
> \int\rho\,|\nabla\log\rho|^2$ is `ml-geometry/10` §0's Fisher information
> matrix evaluated on the **location family** $p_\theta(x) := \rho(x-\theta)$:
> there, $\partial_{\theta^i}\log p_\theta = -\partial_{x^i}\log\rho$, so
> $g^F_{ij}(0) = \int\rho\,\partial_i\log\rho\,\partial_j\log\rho$ and
> $I(\rho) = \operatorname{tr}\,g^F(0)$. Same object, two roles — the
> statistically canonical metric of `10` reappears as the *dissipation
> rate* of diffusion. This is not a pun; it is the first join between the
> information-geometry and optimal-transport threads of Phase 3, and the
> Otto calculus below is where they meet properly.

---

## 3. The Otto/JKO Reading: Fokker–Planck Is a Wasserstein Gradient Flow

Theorem 2.1 says $\mathcal{F}$ decreases along Fokker–Planck flow and
stalls exactly at the Gibbs measure. The modern sharpening says *how* it
decreases: **steepest descent, in the $W_2$ geometry of
`ml-geometry/11`**.

**Theorem 3.1 (JKO scheme; stated, recognition depth).** Fix a step
$\tau>0$ and define recursively

$$
\rho_{k+1}^\tau := \operatorname*{arg\,min}_{\rho}\Big\{\mathcal{F}(\rho) + \frac{1}{2\tau}W_2^2(\rho,\rho_k^\tau)\Big\}
$$

(minimum over probability densities with finite second moment; a unique
minimizer exists — direct method plus strict displacement convexity). As
$\tau\to0$, the piecewise-constant interpolation of $(\rho_k^\tau)_k$
converges to the solution of the Fokker–Planck equation with initial
datum $\rho_0$. *(JKO 1998, the original theorem; AGS, Part II, for the
general metric-space theory; Santambrogio 2015, Ch. 8, for a readable
account. The existence/uniqueness argument for one JKO step is the same
strict-convexity-plus-compactness pattern as `ml-geometry/11` Proposition
4.2, but run in $W_2$ topology with lower-semicontinuity inputs from AGS —
genuinely heavier machinery than this repo has built, hence cited rather
than proved; the anchor weight of this document is carried by Theorem 2.1
instead.)*

The scheme is the exact analogue, one level up, of implicit Euler /
proximal descent: compare `ml-geometry/13` §2's iteration $x_{k+1} =
R_{x_k}(-t_k\operatorname{grad}f(x_k))$ — there the space was a
finite-dimensional manifold and the metric was $g$; here the "points" are
probability densities, the metric is $W_2$, and the objective is
$\mathcal{F}$. Otto's insight (Otto 2001, recognition depth) is that this
is not just an analogy: the formal Riemannian metric on densities whose
gradient flow of $\mathcal{F}$ *is* Fokker–Planck is exactly $W_2$'s
infinitesimal form, with tangent vectors $\partial_t\rho = -\nabla\cdot
(\rho\nabla\varphi)$ parametrized by potentials $\varphi$ — and under this
metric, Theorem 2.1's dissipation rate $\int\rho|\nabla(\log\rho+V)|^2$ is
precisely $\lVert\operatorname{grad}_{W_2}\mathcal{F}\rVert^2$, making the
dissipation identity the density-space instance of `ml-geometry/13`'s
$\hat f_k'(0) = -\lVert\operatorname{grad}f\rVert_g^2$.

```text
SDE dX = −∇V dt + √2 dB ──(Itô + martingale, §1)──► ∂_t ρ = ∇·(ρ∇(log ρ + V))
        │                                                   │
        │                                    (Thm 2.1)  dF/dt = −‖grad_{W₂}F‖²  ≤ 0
        ▼                                                   ▼
  Gibbs density e^{−V}/Z  ◄──(stalls exactly there)── steepest descent of F in (P₂, W₂)
                                                            ▲
                              (JKO, Thm 3.1: implicit Euler in W₂ — cf. 13 §2's retraction step)
```

---

## 4. Schrödinger Bridges: Entropic OT, Rediscovered by Diffusions

**The Schrödinger problem** (recognition depth): among all path
distributions of a diffusion on $[0,1]$ with prescribed initial and final
marginals $\mu,\nu$, find the one closest in relative entropy to the
reference Brownian path measure. Its *static* projection — the joint law
of endpoints — is exactly the **entropic optimal transport** problem of
`ml-geometry/11` §4 with $\varepsilon$ proportional to the diffusion's
temperature, and the classical Fortet/IPF iteration that solves it is
Sinkhorn's algorithm, seventy years early. *(Léonard 2014 for the survey
and history; CGP 2021 for the stochastic-control formulation — the title
"Sinkhorn meets Monge on a Schrödinger bridge" is the literal content;
De Bortoli et al. 2021 for the generative-modeling use, previewed in
`22`.)* As $\varepsilon\to0$ the bridge converges to $W_2$'s optimal
coupling — the deterministic-transport limit of a stochastic problem,
completing the circle with `ml-geometry/11` §1.

---

## 5. The Options Desk, Part III: the Volatility Surface Reads Densities

`20` §5 ended with the implied-volatility surface as the market's
coordinate system. This section makes precise the practitioner's working
creed: *the surface is a database of risk-neutral densities, and
Fokker–Planck is its query engine.*

### 5.1 Anchor Proof: Breeden–Litzenberger

**Theorem 5.1.** Fix $T$ and suppose $S_T$ has a continuous risk-neutral
density $q_T$ with $\int s\,q_T(s)\,ds<\infty$. Then the call-price curve
$K\mapsto C(K) = e^{-rT}\int_0^\infty(s-K)^+q_T(s)\,ds$ is convex,
differentiable where $q_T$ is continuous, and

$$
\frac{\partial C}{\partial K} = -e^{-rT}\,\mathbb{Q}(S_T>K), \qquad \boxed{\ \frac{\partial^2 C}{\partial K^2} = e^{-rT}\,q_T(K).\ }
$$

*Proof.* The integrand $(s-K)^+$ is, for each $s$, Lipschitz in $K$ with
$\partial_K(s-K)^+ = -\mathbf{1}_{\{s>K\}}$ except at the single point
$s=K$ (a $q_T$-null set). Difference quotients in $K$ are dominated by the
integrable envelope $q_T(s)$ (Lipschitz constant $1$), so `notes/06`
Theorem 4.3 (DCT, applied to difference quotients exactly as in
`ml-geometry/10` §0's standing hypothesis) lets the derivative pass inside:

$$
\frac{\partial C}{\partial K} = e^{-rT}\int_0^\infty \big(-\mathbf{1}_{\{s>K\}}\big)\,q_T(s)\,ds = -e^{-rT}\,\mathbb{Q}(S_T>K).
$$

The right side is $e^{-rT}(F_{Q}(K)-1)$ with $F_Q$ the CDF of $q_T$;
differentiating once more (fundamental theorem of calculus, $q_T$
continuous) gives $\partial_K^2C = e^{-rT}q_T(K)\ge0$ — which
simultaneously proves convexity. $\blacksquare$

> **Type-check — what the market quotes, typed.** $K\mapsto C(K)$ is
> observable (a strip of quoted option prices); Theorem 5.1 says its
> second derivative *is* the marginal density of $S_T$ under $\mathbb{Q}$,
> up to discounting. No model was assumed beyond the existence of $q_T$ —
> in particular, no GBM and no Black–Scholes. Practitioner corollaries:
> convexity of $C$ in $K$ is an *arbitrage constraint* (a butterfly spread
> has nonnegative price, and its price is a second difference of calls);
> and "fitting the smile" is density estimation wearing a volatility
> costume. Breeden–Litzenberger 1978 is the source; Gatheral, TVS, Ch. 1,
> for the desk-side reading.

### 5.2 Dupire: Fokker–Planck run on the surface

Given the *whole* surface $(K,T)\mapsto C(K,T)$, one can ask for a
diffusion consistent with **all** of it at once. Postulate $dS_t =
rS_t\,dt + \sigma_{\mathrm{loc}}(S_t,t)\,S_t\,dB_t$ under $\mathbb{Q}$;
its marginal densities obey the Fokker–Planck equation (Corollary 1.2,
with this SDE's coefficients), and pushing that forward equation through
Theorem 5.1's identity $q_T = e^{rT}\partial_K^2C$ yields **Dupire's
forward equation and formula** (operational depth):

$$
\partial_TC = \tfrac12\,\sigma_{\mathrm{loc}}^2(K,T)\,K^2\,\partial_K^2C - rK\,\partial_KC, \qquad \sigma_{\mathrm{loc}}^2(K,T) = \frac{\partial_TC + rK\,\partial_KC}{\tfrac12 K^2\,\partial_K^2C}.
$$

*(Dupire 1994; Gatheral, TVS, Ch. 1–3, including the well-posedness
caveats — the division by $\partial_K^2C$ is exactly as fragile as
Theorem 5.1's density is close to zero in the wings, which is where
practitioners regularize.)* Conceptually: §1 derived Fokker–Planck
*forward in the state variable*; Dupire is the same equation read *forward
in the contract variables* $(K,T)$ — the desk's daily use of this
document's §1.

### 5.3 Martingale optimal transport: `11`'s couplings with a finance constraint

Theorem 5.1 recovers the marginals $q_{T_1}, q_{T_2},\dots$ from quoted
surfaces, but not the joint law across dates. Model-independent bounds on
a path-dependent payoff $\Phi(S_{T_1},S_{T_2})$ are then exactly an
optimal-transport problem over couplings $\pi\in\Pi(q_{T_1},q_{T_2})$
(`ml-geometry/11` §0) with one additional linear constraint — the
**martingale condition** $\mathbb{E}_\pi[S_{T_2}\mid S_{T_1}] = S_{T_1}$
(discounted prices must be a $\mathbb{Q}$-martingale, `20` §5):

$$
\sup / \inf_{\pi\in\Pi_{\mathrm{mart}}(q_{T_1},q_{T_2})} \ \mathbb{E}_\pi\big[\Phi(S_{T_1},S_{T_2})\big],
$$

with a Kantorovich-type duality whose dual variables are *tradable
hedges* (static option portfolios plus dynamic forward positions) — the
no-arbitrage reading of `ml-geometry/11` §3's "feasible pricing"
type-check, now literal. *(BHLP 2013, the founding paper; recognition
depth — the duality needs care exactly where `11` §3 flagged the
continuous case as heavier.)*

```text
quoted surface C(K,T)
      │ (Thm 5.1: ∂²_K C = e^{−rT} q_T — Breeden–Litzenberger)
      ▼
risk-neutral marginals q_T          (density estimation, no model yet)
      │ (Corollary 1.2 read in (K,T): Dupire forward equation)
      ▼
local-vol diffusion σ_loc(K,T)      (one diffusion fitting the whole surface)
      │ (drop the diffusion, keep the marginals)
      ▼
martingale OT over Π_mart(q_{T₁}, q_{T₂})   →  model-free price bounds + static hedges (BHLP 2013)
```

---

## References Used

- **Karatzas–Shreve**; **Le Gall** — $n$-dimensional Itô formula and
  martingale property used in Proposition 1.1 (via `stochastic/20`).
- **JKO 1998** — Theorem 3.1's minimizing-movement scheme and convergence.
- **Otto 2001** — the formal Riemannian ($W_2$) reading of §3.
- **AGS** — Part II (the rigorous metric-space gradient-flow theory §3
  cites in place of proving).
- **Santambrogio 2015** — Ch. 8 (readable JKO/gradient-flow account;
  cross-checked against AGS).
- **Léonard 2014**; **CGP 2021**; **De Bortoli et al. 2021** — §4's
  Schrödinger-bridge = entropic-OT identification and its modern uses.
- **Breeden–Litzenberger 1978** — Theorem 5.1.
- **Dupire 1994**; **Gatheral, TVS** — §5.2's forward equation, formula,
  and practitioner caveats.
- **BHLP 2013** — §5.3's martingale-OT bounds.
- **`ml-geometry/10`** §0 (Fisher information, §2's type-check);
  **`ml-geometry/11`** (couplings, duality, entropic OT);
  **`ml-geometry/13`** §2 (the descent-iteration analogy of §3);
  **`notes/06`** Theorem 4.3 (every differentiation under an integral
  above).

Full bibliographic data: `stochastic/NOTATION_STOCH.md` §3.

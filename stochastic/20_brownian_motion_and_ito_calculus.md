# 20 — Brownian Motion, Itô Calculus, and the Black–Scholes Bridge

**Cross-references.** `stochastic/NOTATION_STOCH.md` (typing §1.1–§1.3,
§1.7, citation key §3 — read first); `notes/06_measure_theory_bridge.md`
(measure, DCT, used throughout); `01_prerequisite_systematic_review_for_GMT_Topology.md`
§9.3, §13.9 ($L^2$/Hilbert geometry — the Itô isometry below *is* that
geometry), §10 (Picard–Lindelöf, upgraded here to SDEs); `notes/07_rectifiability_and_hausdorff_measure.md`
§1 (Rademacher — the contrast object for §1); `00_differential_geometry_foundations_for_GMT_Topology.md`
§21 (heat equation — §5 reduces Black–Scholes to it); feeds
`stochastic/21`–`22` (generators, forward equations, OU computations) and
`ml-geometry/16` §0.3 (whose GBM soft label this document rigorizes).

**Register (maturity ladder, `NOTATION_STOCH.md` §4).** Settled classical
mathematics — Itô 1944; canonical texts Karatzas–Shreve, Le Gall. Phase-2
register: settled canon, cite the monographs. The *finance* material in
§5–§6 is likewise settled (Black–Scholes 1973 is half a century old);
only its practitioner folklore is flagged where it appears.

---

## 0. Standing Definitions

Everything from `stochastic/NOTATION_STOCH.md` §1.1–§1.3 and §1.7 —
$(\Omega,\mathcal{F},(\mathcal{F}_t),\mathbb{P})$, Brownian motion $B$,
quadratic variation, the SDE notation, GBM, and the finance objects
$r, \mathbb{Q}, C(K,T), \tau_b$. Additionally, for this document:

$$
\Pi = \{0=t_0<t_1<\cdots<t_m=t\} \quad \text{a partition of } [0,t], \qquad \lVert\Pi\rVert := \max_i(t_{i+1}-t_i), \qquad \Delta B_i := B_{t_{i+1}}-B_{t_i}, \ \Delta t_i := t_{i+1}-t_i.
$$

$$
\mathcal{S} := \Big\{ H_s(\omega) = \sum_{i=0}^{m-1}\xi_i(\omega)\,\mathbf{1}_{(t_i,t_{i+1}]}(s) \ : \ \xi_i \ \mathcal{F}_{t_i}\text{-measurable and bounded} \Big\} \quad \text{simple adapted processes on } [0,t].
$$

$$
\lVert H\rVert_{L^2(dt\times d\mathbb{P})}^2 := \mathbb{E}\int_0^t H_s^2\,ds, \qquad \Phi(t,x) := \int_{-\infty}^x \tfrac{1}{\sqrt{2\pi t}}e^{-y^2/2t}\,dy, \quad \Phi := \Phi(1,\cdot) \ \text{(standard normal CDF)}.
$$

Brownian motion is used throughout in dimension $d=1$ unless stated;
existence of a process with §1.2's defining properties is taken as given
(Wiener's theorem — construction via Kolmogorov extension plus continuity
modification, Karatzas–Shreve Ch. 2, black-boxed per `01` §17: it is
construction machinery, not the object of study here).

---

## 1. Why a New Calculus: Paths Rougher Than Rademacher Allows

`notes/07` §1 established the GMT resolution of non-smoothness: Lipschitz
maps are differentiable *almost everywhere* (Rademacher), so a.e.-defined
Jacobians survive. Brownian motion sits strictly below that floor:

**Theorem 1.1 (stated; Paley–Wiener–Zygmund).** Almost surely, $t\mapsto
B_t$ is continuous but **nowhere differentiable** — the a.e.-derivative
route of `notes/07` is closed, on every interval, with probability one.
*(Karatzas–Shreve, Ch. 2, Thm 9.18; recognition depth.)*

What survives is *quadratic* structure:

**Proposition 1.2 (quadratic variation of $B$, in $L^2$).** For partitions
$\Pi$ of $[0,t]$,

$$
\sum_{i}(\Delta B_i)^2 \ \longrightarrow\ t \quad \text{in } L^2(\Omega) \text{ as } \lVert\Pi\rVert\to0.
$$

*Proof.* Write $Z_i := (\Delta B_i)^2 - \Delta t_i$; then $\sum_i(\Delta
B_i)^2 - t = \sum_i Z_i$. The $Z_i$ are independent (independent
increments) with $\mathbb{E}[Z_i]=0$ (since $\Delta B_i\sim N(0,\Delta
t_i)$), so all cross terms vanish in

$$
\mathbb{E}\Big[\Big(\sum_i Z_i\Big)^2\Big] = \sum_i \mathbb{E}[Z_i^2] = \sum_i \operatorname{Var}\big((\Delta B_i)^2\big) = \sum_i 2\,\Delta t_i^2 \ \le\ 2\lVert\Pi\rVert\sum_i\Delta t_i = 2\lVert\Pi\rVert\,t \ \to\ 0,
$$

using $\operatorname{Var}(Z^2) = \mathbb{E}[Z^4]-(\mathbb{E}[Z^2])^2 =
3\sigma^4-\sigma^4 = 2\sigma^4$ for $Z\sim N(0,\sigma^2)$. $\blacksquare$

> **Type-check — what $[B]_t = t$ is saying.** For a $C^1$ path $x$,
> $\sum(\Delta x_i)^2 \le \lVert x'\rVert_\infty^2\sum\Delta t_i^2\to0$:
> smooth paths have *zero* quadratic variation, so first-order calculus
> never sees a $(dx)^2$ term. Brownian paths accumulate quadratic variation
> at unit rate — $(dB)^2 = dt$ in the mnemonic — and every deviation of Itô
> calculus from classical calculus (the $\tfrac12 f''$ term in Theorem 3.1,
> the $-\sigma^2/2$ in GBM's exponent, the $\tfrac12\sigma^2S^2\partial_S^2$
> term in Black–Scholes) is this one fact propagating through Taylor
> expansions.

---

## 2. The Itô Integral and the Anchor Proof: the Itô Isometry

For $H\in\mathcal{S}$ (simple adapted, §0), define the **elementary
stochastic integral**

$$
I(H) := \int_0^t H_s\,dB_s := \sum_{i=0}^{m-1} \xi_i\,\Delta B_i \ \in L^2(\Omega).
$$

> **Type-check — why $\xi_i$ must be $\mathcal{F}_{t_i}$-measurable.** The
> integrand is frozen at the *left* endpoint: $\xi_i$ may use information
> up to $t_i$ but not peek at the increment $\Delta B_i$ it multiplies.
> This is not a technicality — it is the finance-native convention (a
> trading position must be chosen *before* the price move it profits from,
> §5), and it is exactly what makes the cross terms vanish in the proof
> below. The Stratonovich integral makes the other choice (midpoint) and
> loses both the martingale property and the isometry.

### Anchor Theorem: Itô Isometry

**Theorem 2.1.** For every $H\in\mathcal{S}$,

$$
\mathbb{E}\Big[\Big(\int_0^t H_s\,dB_s\Big)^2\Big] \ =\ \mathbb{E}\int_0^t H_s^2\,ds,
$$

i.e. $I : \mathcal{S}\subseteq L^2(dt\times d\mathbb{P}) \to L^2(\Omega)$
is a linear isometry. Moreover $\mathbb{E}[I(H)]=0$.

*Proof.* Expand the square:

$$
\mathbb{E}\big[I(H)^2\big] = \sum_{i,j}\mathbb{E}\big[\xi_i\xi_j\,\Delta B_i\,\Delta B_j\big].
$$

*Cross terms ($i<j$).* Condition on $\mathcal{F}_{t_j}$: the factors
$\xi_i$, $\xi_j$, $\Delta B_i$ are all $\mathcal{F}_{t_j}$-measurable
($t_{i+1}\le t_j$, and $\xi_j$ is $\mathcal{F}_{t_j}$-measurable by
adaptedness — the type-check above), while $\Delta B_j$ is independent of
$\mathcal{F}_{t_j}$ with mean zero. By the tower property of conditional
expectation (the $L^2$-projection of `01` §9.3, applied as: project onto
$L^2(\mathcal{F}_{t_j})$ first),

$$
\mathbb{E}\big[\xi_i\xi_j\Delta B_i\,\Delta B_j\big] = \mathbb{E}\big[\xi_i\xi_j\Delta B_i\ \mathbb{E}[\Delta B_j\mid\mathcal{F}_{t_j}]\big] = \mathbb{E}\big[\xi_i\xi_j\Delta B_i\cdot 0\big] = 0.
$$

The same argument with a single factor gives $\mathbb{E}[\xi_i\Delta B_i]
= 0$, hence $\mathbb{E}[I(H)]=0$.

*Diagonal terms ($i=j$).* Condition on $\mathcal{F}_{t_i}$: $\xi_i^2$ is
$\mathcal{F}_{t_i}$-measurable, $\Delta B_i$ independent of it with
$\mathbb{E}[(\Delta B_i)^2] = \Delta t_i$, so

$$
\mathbb{E}\big[\xi_i^2(\Delta B_i)^2\big] = \mathbb{E}\big[\xi_i^2\ \mathbb{E}[(\Delta B_i)^2\mid\mathcal{F}_{t_i}]\big] = \mathbb{E}[\xi_i^2]\,\Delta t_i.
$$

Summing,

$$
\mathbb{E}\big[I(H)^2\big] = \sum_i \mathbb{E}[\xi_i^2]\,\Delta t_i = \mathbb{E}\int_0^t H_s^2\,ds. \qquad\blacksquare
$$

**Corollary 2.2 (the extension, at operational depth).** $\mathcal{S}$ is
dense in the space of adapted processes with $\mathbb{E}\int_0^tH^2\,ds<
\infty$ (Karatzas–Shreve, Ch. 3, Prop. 2.8 — an approximation argument,
black-boxed per `01` §17), and an isometry defined on a dense subspace of
a Hilbert space extends uniquely to a linear isometry on its closure,
because completeness (`01` §9.2) turns Cauchy-in-norm into convergent.
The extension is *defined* to be the Itô integral. It inherits
$\mathbb{E}[I(H)]=0$, the isometry, and the martingale property of
$t\mapsto\int_0^tH\,dB$ (Karatzas–Shreve, Ch. 3).

```text
simple adapted H ∈ 𝒮 ──(freeze-left sums)──► I(H) = Σ ξᵢ ΔBᵢ ∈ L²(Ω)
        │                                          │
        │  (Theorem 2.1: ‖I(H)‖_{L²(Ω)} = ‖H‖_{L²(dt×dℙ)})
        ▼                                          ▼
dense subspace of L²(adapted) ──(isometry + completeness)──► ∫₀ᵗ H dB, an L²-martingale
        │
        ▼
Itô processes dX = b dt + σ dB ──(Taylor + Prop 1.2)──► Itô's formula (§3)
```

> **What this buys you.** The Itô integral is not a pathwise
> Riemann–Stieltjes integral — Theorem 1.1 forecloses that (a nowhere
> differentiable path has infinite variation on every interval). It is an
> $L^2$-*geometric* object: Theorem 2.1 says the map $H\mapsto\int H\,dB$
> is a linear isometry between Hilbert spaces, and Corollary 2.2 completes
> it by exactly the completeness argument this repo has used since `01`
> §9.2. The entire integral is `01` §13.9's inner-product geometry, run at
> full power.

---

## 3. Itô's Formula

**Theorem 3.1 (Itô's formula, stated).** Let $X_t = X_0 + \int_0^t b_s\,ds
+ \int_0^t\sigma_s\,dB_s$ be an Itô process and $f\in C^{1,2}([0,\infty)
\times\mathbb{R})$. Then

$$
f(t,X_t) = f(0,X_0) + \int_0^t\Big(\partial_tf + b_s\,\partial_xf + \tfrac12\sigma_s^2\,\partial_x^2f\Big)(s,X_s)\,ds + \int_0^t \sigma_s\,\partial_xf(s,X_s)\,dB_s.
$$

*(Karatzas–Shreve, Ch. 3, Thm 3.3; Le Gall, Ch. 5. Proof idea, depth B: a
second-order Taylor expansion of $f$ over a partition; first-order terms
assemble the $ds$ and $dB$ integrals; the second-order term
$\tfrac12\partial_x^2f\cdot(\Delta X_i)^2$ does **not** vanish, because
Proposition 1.2 makes $\sum(\Delta X_i)^2$ converge to
$\int\sigma_s^2\,ds$ rather than to $0$; all genuinely higher-order terms
vanish. The full proof is bookkeeping around exactly these three facts and
is black-boxed at operational depth per `01` §17.)*

| Classical chain rule | Itô's formula |
|---|---|
| $df(x_t) = f'(x_t)\,dx_t$ | $df(X_t) = f'(X_t)\,dX_t + \tfrac12 f''(X_t)\,d[X]_t$ |
| valid: $x$ of bounded variation | valid: $X$ an Itô process |
| second-order Taylor term $\to0$ | second-order term $\to \tfrac12f''\,\sigma^2dt$ (Prop 1.2) |

---

## 4. Geometric Brownian Motion, Solved

**Theorem 4.1.** Let $\mu\in\mathbb{R}$, $\sigma>0$, $S_0>0$. The process

$$
S_t := S_0\exp\Big(\big(\mu-\tfrac{\sigma^2}{2}\big)t + \sigma B_t\Big)
$$

solves $dS_t = \mu S_t\,dt + \sigma S_t\,dB_t$, and it is the unique
(up to indistinguishability) solution with initial value $S_0$.

*Proof of the solution property (depth C).* Let $Y_t := (\mu-\tfrac{\sigma^2}{2})t
+ \sigma B_t$ (an Itô process with $b\equiv\mu-\tfrac{\sigma^2}{2}$,
$\sigma_s\equiv\sigma$) and $f(y):=S_0e^y$, so $S_t = f(Y_t)$ and $f'=f''=f$.
By Itô's formula (Theorem 3.1, time-independent case),

$$
dS_t = f'(Y_t)\,dY_t + \tfrac12 f''(Y_t)\,d[Y]_t = S_t\Big(\big(\mu-\tfrac{\sigma^2}{2}\big)dt + \sigma\,dB_t\Big) + \tfrac12 S_t\,\sigma^2\,dt = \mu S_t\,dt + \sigma S_t\,dB_t. \ \blacksquare
$$

*Uniqueness:* $b(s)=\mu s$ and $\sigma(s)=\sigma s$ are globally Lipschitz,
so pathwise uniqueness holds by the standard Picard-iteration theorem for
Lipschitz SDEs — the same fixed-point mechanism as `01` §10's proof anchor,
now on a space of processes (Karatzas–Shreve, Ch. 5, Thm 2.5; cited at
operational depth).

> **Type-check — where the $-\sigma^2/2$ comes from, and why it matters
> downstream.** Naively exponentiating the SDE would suggest $S_t = S_0
> e^{\mu t+\sigma B_t}$; the correction $-\sigma^2t/2$ is precisely Itô's
> second-order term, i.e. Proposition 1.2 again. Consequences used later:
> $\mathbb{E}[S_t] = S_0e^{\mu t}$ (the correction exactly offsets
> $\mathbb{E}[e^{\sigma B_t}] = e^{\sigma^2t/2}$), $\log S_t$ is Gaussian
> (so $S_t$ is lognormal — the distribution under every Black–Scholes
> computation in §5), and the *median* path grows at rate
> $\mu-\sigma^2/2<\mu$: volatility drags typical outcomes below the mean.
> This is the process, typed and solved, that generates DIML's
> $\tilde y_{GBM}$ soft labels (`ml-geometry/16` §0.3); §6 finishes that
> story.

---

## 5. The Options Desk, Part I: Black–Scholes as a Heat Equation

The standing market model of this section: one riskless asset growing at
rate $r$, one risky asset following GBM (§4), continuous frictionless
trading. $V(t,S)$ denotes the price of a European claim as a function of
time and the current asset price, $V\in C^{1,2}$.

**Proposition 5.1 (the hedging argument, depth B).** Suppose a
self-financing portfolio holds the claim and is short $\Delta_t$ units of
the asset. By Itô's formula (Theorem 3.1) applied to $V(t,S_t)$,

$$
dV = \Big(\partial_tV + \mu S\,\partial_SV + \tfrac12\sigma^2S^2\,\partial_S^2V\Big)dt + \sigma S\,\partial_SV\,dB_t,
$$

so the choice $\Delta_t := \partial_SV(t,S_t)$ (**delta hedging**) cancels
the $dB$ term: the hedged portfolio $\Pi_t = V - \Delta_t S_t$ has
increment $d\Pi = (\partial_tV + \tfrac12\sigma^2S^2\partial_S^2V)\,dt$ —
*riskless*. Absence of arbitrage forces every riskless position to earn
exactly $r$: $d\Pi = r\Pi\,dt$. Equating the two expressions for $d\Pi$
yields the **Black–Scholes PDE**

$$
\boxed{\ \partial_tV + \tfrac12\sigma^2S^2\,\partial_S^2V + rS\,\partial_SV - rV = 0,\ } \qquad V(T,S) = (S-K)^+ \ \text{(European call)}.
$$

*(Black–Scholes 1973; Merton 1973; Shreve, SCF II, Ch. 4 for the
self-financing bookkeeping suppressed above.)*

> **Type-check — the practitioner's central fact: $\mu$ is gone.** The
> drift $\mu$ — the parameter every market participant argues about —
> cancels the moment $\Delta = \partial_SV$ is substituted, and the PDE
> contains only $\sigma$ and $r$. Pricing does not require an opinion on
> direction; it requires an opinion on *volatility*. Equivalently
> (Girsanov's theorem plus the martingale representation theorem, cited at
> recognition depth: Harrison–Pliska 1981; Shreve, SCF II, Ch. 5), there is
> an equivalent measure $\mathbb{Q}$ under which $e^{-rt}S_t$ is a
> martingale and $V(t,S) = e^{-r(T-t)}\mathbb{E}_{\mathbb{Q}}[\,\text{payoff}
> \mid S_t=S\,]$ — the SDE under $\mathbb{Q}$ is GBM with $\mu$ replaced by
> $r$. This is the change-of-measure counterpart of the hedging argument.

**Proposition 5.2 (reduction to the heat equation, operational depth).**
Set $\tau := T-t$, $x := \log S$, and $u(\tau,x) := e^{r\tau}V(T-\tau,e^x)$.
A direct chain-rule computation turns the Black–Scholes PDE into the
constant-coefficient parabolic equation $\partial_\tau u = \tfrac12\sigma^2
\partial_x^2u + (r-\tfrac12\sigma^2)\partial_xu$, and the further shift
$y := x+(r-\tfrac12\sigma^2)\tau$ removes the first-order term:

$$
\partial_\tau w = \tfrac12\sigma^2\,\partial_y^2 w
$$

— the **heat equation** of `00` §21, with thermal diffusivity
$\tfrac12\sigma^2$. Solving it with the call payoff as initial data and
undoing the substitutions gives the **Black–Scholes formula**

$$
C(t,S) = S\,\Phi(d_1) - Ke^{-r\tau}\,\Phi(d_2), \qquad d_{1,2} = \frac{\log(S/K) + (r\pm\tfrac12\sigma^2)\tau}{\sigma\sqrt\tau}.
$$

*(Standard; Shreve, SCF II, Ch. 4; Gatheral, TVS, Ch. 1.)* Practitioner
note, at recognition depth: desks quote prices *through this formula run
backwards* — given a market price, the **implied volatility**
$\sigma_{\mathrm{impl}}(K,T)$ is the unique $\sigma$ reproducing it
(unique since $\partial C/\partial\sigma>0$), and the resulting surface
$(K,T)\mapsto\sigma_{\mathrm{impl}}$ is the market's native coordinate
system. What the *shape* of that surface says about the risk-neutral
density is `21` §5's subject (Breeden–Litzenberger, Dupire).

```text
GBM under ℙ (§4) ──(Itô, Thm 3.1)──► dV expansion
      │                                   │  choose Δ = ∂_S V  (kills dB)
      ▼                                   ▼
no-arbitrage (riskless ⇒ rate r) ──► Black–Scholes PDE ──(τ = T−t, x = log S)──► heat equation (00 §21)
                                                              │
                                                              ▼
                                              BS formula ──(inverted)──► implied-vol surface ──► 21 §5
```

---

## 6. The Options Desk, Part II: First Passage, Barriers, and the DIML Soft Label

### 6.1 Anchor Proof: the Reflection Principle

**Theorem 6.1.** For standard Brownian motion and $a>0$, $t>0$,

$$
\mathbb{P}\Big(\max_{0\le s\le t}B_s \ge a\Big) = 2\,\mathbb{P}(B_t > a).
$$

*Proof.* Let $\tau_a := \inf\{s\ge0 : B_s = a\}$, a stopping time; by path
continuity, $\{\max_{s\le t}B_s\ge a\} = \{\tau_a\le t\}$ (the maximum
reaches $a$ iff the path, started below, touches it — the intermediate
value theorem). Decompose by the endpoint's position ($\mathbb{P}(B_t=a)=0$,
a continuous distribution):

$$
\mathbb{P}(\tau_a\le t) = \mathbb{P}(\tau_a\le t,\ B_t>a) + \mathbb{P}(\tau_a\le t,\ B_t<a).
$$

The first event is just $\{B_t>a\}$: an endpoint above $a$ forces a
crossing before $t$, again by continuity. For the second, apply the
**strong Markov property** (the one input taken as a black box, per `01`
§17: at the stopping time $\tau_a$, the post-$\tau_a$ path
$s\mapsto B_{\tau_a+s}-a$ is a fresh Brownian motion independent of the
past; Karatzas–Shreve, Ch. 2, and the reflection argument is their §2.6):
a fresh Brownian motion is symmetric, so conditionally on $\{\tau_a\le t\}$
the events "end below $a$" and "end above $a$" have equal probability.
Hence

$$
\mathbb{P}(\tau_a\le t,\ B_t<a) = \mathbb{P}(\tau_a\le t,\ B_t>a) = \mathbb{P}(B_t>a),
$$

and adding the two pieces gives $\mathbb{P}(\tau_a\le t) = 2\,\mathbb{P}
(B_t>a)$. $\blacksquare$

**Corollary 6.2 (first-passage law).** $\mathbb{P}(\tau_a\le t) =
2(1-\Phi(a/\sqrt t))$, and differentiating in $t$ gives the density

$$
f_{\tau_a}(t) = \frac{a}{\sqrt{2\pi t^3}}\,e^{-a^2/2t}, \qquad t>0
$$

— heavy-tailed: $\mathbb{E}[\tau_a]=\infty$ even though $\tau_a<\infty$
almost surely. Passage is certain but its expected wait is infinite; a
useful calibration of how misleading "eventually" can be.

### 6.2 Drift, GBM barriers, and the closed form the oracle almost has

For Brownian motion with drift, symmetry fails and the reflection argument
needs a Girsanov change of measure; the result (operational depth,
Karatzas–Shreve §3.5; Shreve, SCF II, Ch. 7) for $X_t = \nu t+\sigma B_t$
and barrier $m<0$ is the **inverse-Gaussian barrier law**

$$
\mathbb{P}\Big(\min_{s\le T}X_s\le m\Big) = \Phi\Big(\frac{m-\nu T}{\sigma\sqrt T}\Big) + e^{2\nu m/\sigma^2}\,\Phi\Big(\frac{m+\nu T}{\sigma\sqrt T}\Big).
$$

Since $\log(S_t/S_0)$ is exactly such a process with $\nu=\mu-\tfrac{\sigma^2}
{2}$ (§4), GBM barrier probabilities are this formula with $m = \log(b/S_0)$
— the engine inside **barrier-option** pricing (down-and-out/down-and-in
contracts; Shreve, SCF II, Ch. 7), where the reflection principle is not a
textbook curiosity but the desk's closed form.

**The DIML soft label, finished.** `ml-geometry/16` §0.3's
$\tilde y_{GBM}$ is a Monte Carlo estimate of a first-passage probability
for simulated GBM. This section pins down what is and is not analytically
available:

- **One gate, closed form.** The oracle's loss gate $L\le-\theta_1$ alone
  is a lower barrier $b = (1-\theta_1)\cdot(\text{cost basis})$ on the
  price path, and its 30-day firing probability is *exactly* the displayed
  inverse-Gaussian law with $T=30$ days. A free sanity check on the
  simulator: for lots where the other three gates are slack, the Monte
  Carlo frequency should match this formula to sampling error.
- **Four gates, no closed form.** The full oracle conjunction
  (`ml-geometry/16` §0.2) couples the price barrier to a tracking-error
  cap, a realized-gains sign, and a *path-dependent* wash-sale clock; the
  first-passage region is no longer a half-line barrier for a
  one-dimensional diffusion, and no closed form should be expected. The
  200-path Monte Carlo of `16` §0.3 is therefore *justified*, not merely
  convenient — with the one-gate formula available as its unit test.

---

## References Used

- **Karatzas–Shreve** — Ch. 2 (Wiener's construction, nowhere
  differentiability Thm 9.18, strong Markov property, §2.6 reflection
  principle), Ch. 3 (Itô integral, Prop. 2.8 density of simple processes,
  Thm 3.3 Itô's formula), §3.5 (Girsanov, drifted first passage), Ch. 5
  (Thm 2.5, Lipschitz SDE uniqueness).
- **Le Gall** — Ch. 5 (Itô's formula; a cleaner modern proof of the same
  results, cross-checked against Karatzas–Shreve above).
- **Itô 1944** — the original stochastic integral.
- **Black–Scholes 1973**; **Merton 1973** — §5's PDE and formula.
- **Harrison–Pliska 1981** — martingale/risk-neutral formulation (§5,
  recognition depth).
- **Shreve, SCF II** — Ch. 4 (delta hedging, BS PDE and formula), Ch. 5
  (risk-neutral measure), Ch. 7 (barrier options, first-passage laws).
- **Gatheral, TVS** — Ch. 1 (implied volatility as the market's
  coordinate system; hands off to `21` §5).
- **`ml-geometry/16`** §0.2–0.3 — the DIML oracle and $\tilde y_{GBM}$,
  rigorized in §6.2.

Full bibliographic data: `stochastic/NOTATION_STOCH.md` §3.

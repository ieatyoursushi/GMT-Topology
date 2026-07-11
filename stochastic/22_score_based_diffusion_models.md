# 22 — Score-Based Diffusion Models: Noising, Score Matching, Time Reversal

**Cross-references.** `stochastic/NOTATION_STOCH.md` (typing §1.3, §1.5,
citation key §3); `stochastic/20` (Itô calculus, the isometry — used
verbatim in §2), `stochastic/21` (forward equations, §1; Schrödinger
bridges, §5); `ml-geometry/10` (the Fisher score $s_\theta$ — §0's
type-check distinguishes it from this document's Stein score);
`ml-geometry/11` (OT — the flow-matching remark in §5); `notes/06`
Theorem 4.3 (DCT, justifying §3's differentiation under the integral).

**Register (maturity ladder, `NOTATION_STOCH.md` §4).** Genuinely young:
the founding practical papers are 2020–2021, the field's preferred
formulation has already shifted once (score SDEs $\to$ flow matching,
§5), and this document should be read in Phase-3 register — PhD-direction
reconnaissance anchored on the results likely to survive reformulation:
the two closed-form computations (§2) and the two identities (§3, §4).

---

## 0. Standing Definitions

$$
p_0 =: p_{\mathrm{data}} \quad \text{the data density on } \mathbb{R}^n, \qquad p_t \quad \text{the density of the forward process at time } t \text{ (standing hypothesis below)}.
$$

$$
\text{Forward (noising) SDE — the OU/VP choice used throughout:} \qquad dX_t = -X_t\,dt + \sqrt2\,dB_t, \qquad X_0\sim p_{\mathrm{data}}.
$$

$$
q_\sigma(\tilde x\mid x) := N(\tilde x;\ x,\ \sigma^2 I_n) \quad \text{a Gaussian noising kernel}, \qquad q_\sigma(\tilde x) := \int q_\sigma(\tilde x\mid x)\,p_{\mathrm{data}}(x)\,dx \quad \text{the noised marginal}.
$$

$$
s_\theta : \mathbb{R}^n\times[0,T]\to\mathbb{R}^n \quad \text{a parametrized (neural) score model}, \qquad \nabla_x\log p_t \quad \text{the (Stein) score it targets (`NOTATION\_STOCH.md' §1.5)}.
$$

**Standing regularity hypothesis** (same pattern as `ml-geometry/10` §0
and `stochastic/21` §0): all densities are smooth and positive with
integrable-envelope decay, so differentiation under the integral sign is
justified by `notes/06` Theorem 4.3, and all expectations written below
are finite.

```text
p_data ──(forward OU SDE: analytic, no learning)──► p_t ──► N(0, I) as t → ∞   (§2)
                                                     │
                              (§3: denoising score matching — the trainable objective)
                                                     ▼
                                        s_θ(x,t) ≈ ∇_x log p_t(x)
                                                     │
              (§4: Anderson's reverse SDE / probability-flow ODE, run t: T → 0)
                                                     ▼
                                     samples ≈ p_data   (generation = time reversal)
```

---

## 1. The One-Sentence Construction

Diffusion models generate by *reversing a diffusion*: push data through a
fixed noising SDE until it is indistinguishable from a Gaussian (§2),
learn the score $\nabla_x\log p_t$ of every intermediate marginal — the
only unknown the reversal needs (§4) — via an objective that never
requires evaluating that score (§3, the anchor), then integrate the
reverse-time SDE from noise back to data. Each ingredient is a Phase-4
object already built: the forward SDE is `20` §3–§4 machinery, its
marginals obey `21`'s forward equation, and the score is
`ml-geometry/10`'s formula shape with the differentiation slot moved.

---

## 2. The Forward Process, Solved Exactly

**Proposition 2.1 (OU transition law).** The solution of §0's forward SDE
started at $X_0 = x_0$ is

$$
X_t = x_0e^{-t} + \sqrt2\int_0^t e^{-(t-s)}\,dB_s, \qquad X_t \ \sim\ N\big(x_0e^{-t},\ (1-e^{-2t})\,I_n\big).
$$

*Proof (depth C, per coordinate).* Apply Itô's formula (`20` Theorem 3.1)
to $f(t,x) = e^tx$: $d(e^tX_t) = e^tX_t\,dt + e^t\,dX_t = \sqrt2\,e^t\,dB_t$
(the drift cancels — that is what the integrating factor $e^t$ is for), so
$e^tX_t = x_0 + \sqrt2\int_0^te^s\,dB_s$, which is the displayed formula.
The stochastic integral of a *deterministic* integrand is Gaussian (an
$L^2$-limit of Gaussian sums, `20` Corollary 2.2), with mean $0$ (`20`
Theorem 2.1) and variance given by **the Itô isometry**:

$$
\operatorname{Var}\Big(\sqrt2\,e^{-t}\int_0^te^s\,dB_s\Big) = 2e^{-2t}\int_0^te^{2s}\,ds = 2e^{-2t}\cdot\frac{e^{2t}-1}{2} = 1-e^{-2t}. \qquad\blacksquare
$$

**Consequences.** (i) $p_t \to N(0,I_n)$ as $t\to\infty$ at rate $e^{-t}$
in the mean and $e^{-2t}$ in the variance gap — "the forward process
forgets its initial condition," which is why pure noise is a usable
starting point for generation. (ii) The conditional marginal is exactly a
Gaussian kernel: $p_t(\cdot\mid x_0) = q_\sigma(\cdot\mid x_0e^{-t})$ with
$\sigma^2 = 1-e^{-2t}$ — so §3's Gaussian-kernel score matching *is* the
training objective for this SDE, with no approximation. (iii) The score
of a Gaussian is affine: $\nabla_{\tilde x}\log q_\sigma(\tilde x\mid x)
= (x-\tilde x)/\sigma^2$ — the "predict the noise" form below.

---

## 3. Anchor Proof: Denoising Score Matching

The object the reversal needs is $\nabla\log q_\sigma$ (the *marginal*
score) — unavailable, since $q_\sigma$ integrates over the unknown
$p_{\mathrm{data}}$. The identity that makes the field trainable:

**Theorem 3.1 (Vincent 2011).** Define, for a candidate score field
$s:\mathbb{R}^n\to\mathbb{R}^n$,

$$
J_{\mathrm{expl}}(s) := \tfrac12\,\mathbb{E}_{\tilde x\sim q_\sigma}\big\|s(\tilde x) - \nabla_{\tilde x}\log q_\sigma(\tilde x)\big\|^2, \qquad J_{\mathrm{DSM}}(s) := \tfrac12\,\mathbb{E}_{\substack{x\sim p_{\mathrm{data}} \\ \tilde x\sim q_\sigma(\cdot\mid x)}}\big\|s(\tilde x) - \nabla_{\tilde x}\log q_\sigma(\tilde x\mid x)\big\|^2.
$$

Then $J_{\mathrm{expl}}(s) = J_{\mathrm{DSM}}(s) + C$, where $C$ does not
depend on $s$. In particular the two objectives have the same minimizers.

*Proof.* Expand both squares; the $\tfrac12\mathbb{E}\|s(\tilde x)\|^2$
terms agree exactly (in both objectives $\tilde x$ has marginal
$q_\sigma$), and the terms not involving $s$ are constants. It remains to
match the cross terms. For $J_{\mathrm{expl}}$:

$$
\mathbb{E}_{\tilde x\sim q_\sigma}\big[s(\tilde x)\cdot\nabla\log q_\sigma(\tilde x)\big] = \int s(\tilde x)\cdot\nabla q_\sigma(\tilde x)\,d\tilde x,
$$

using $q_\sigma\nabla\log q_\sigma = \nabla q_\sigma$ (positivity, §0).
Differentiate the definition of $q_\sigma$ under the integral sign
(standing hypothesis; `notes/06` Theorem 4.3 applied to the difference
quotients, exactly as in `ml-geometry/10` Theorem 2.1's proof):

$$
\nabla_{\tilde x}\,q_\sigma(\tilde x) = \int \nabla_{\tilde x}\,q_\sigma(\tilde x\mid x)\,p_{\mathrm{data}}(x)\,dx.
$$

Substituting and unwinding with $\nabla_{\tilde x}q_\sigma(\tilde x\mid x)
= q_\sigma(\tilde x\mid x)\,\nabla_{\tilde x}\log q_\sigma(\tilde x\mid x)$:

$$
\int s(\tilde x)\cdot\nabla q_\sigma(\tilde x)\,d\tilde x = \iint s(\tilde x)\cdot\nabla_{\tilde x}\log q_\sigma(\tilde x\mid x)\ q_\sigma(\tilde x\mid x)\,p_{\mathrm{data}}(x)\,dx\,d\tilde x = \mathbb{E}_{x,\tilde x}\big[s(\tilde x)\cdot\nabla_{\tilde x}\log q_\sigma(\tilde x\mid x)\big],
$$

which is precisely $J_{\mathrm{DSM}}$'s cross term. So the two objectives
differ only in their $s$-free constants:
$C = \tfrac12\mathbb{E}\|\nabla\log q_\sigma(\tilde x)\|^2 -
\tfrac12\mathbb{E}\|\nabla\log q_\sigma(\tilde x\mid x)\|^2$. $\blacksquare$

> **Type-check — why this is the load-bearing identity.**
> $J_{\mathrm{expl}}$ mentions $\nabla\log q_\sigma$, which nobody can
> evaluate; $J_{\mathrm{DSM}}$ mentions only $\nabla\log q_\sigma(\tilde
> x\mid x)$ — for the Gaussian kernel, $(x-\tilde x)/\sigma^2$, i.e.
> *the noise that was added, rescaled* (write $\tilde x = x+\sigma\varepsilon$:
> the regression target is $-\varepsilon/\sigma$). Theorem 3.1 says
> minimizing the tractable objective minimizes the intractable one. Every
> diffusion model's training loop — including DDPM's
> "$\varepsilon$-prediction" (Ho–Jain–Abbeel 2020) and the continuous-time
> weighting of Song et al. 2021 — is this theorem applied at each noise
> scale $\sigma(t)$ of Proposition 2.1(ii), plus a choice of weighting.
> Hyvärinen 2005's original score matching removes the unknown score by
> integration by parts instead; Vincent's form is the one that scales.

> **Type-check — Fisher score vs. Stein score, resolved in one line.**
> `ml-geometry/10` §2's zero-mean-score identity
> $\mathbb{E}_{p_\theta}[\nabla_\theta\log p_\theta]=0$ has an exact Stein
> twin: $\mathbb{E}_{p}[\nabla_x\log p(X)] = \int\nabla p = 0$, by the
> same differentiate-the-normalization move run in $x$ instead of
> $\theta$. Same mechanism, different slot — the §0 distinction is a
> renaming of *which variable the integral identity is differentiated in*.

---

## 4. Time Reversal

**Theorem 4.1 (Anderson 1982; stated, recognition depth).** Under
regularity hypotheses on the coefficients and marginals, if $X$ solves
$dX_t = b(X_t,t)\,dt + \sigma\,dB_t$ on $[0,T]$ with marginals $p_t$, then
the time-reversed process $\bar X_s := X_{T-s}$ solves

$$
d\bar X_s = \big[-b(\bar X_s,T-s) + \sigma\sigma^\top\nabla_x\log p_{T-s}(\bar X_s)\big]\,ds + \sigma\,d\bar B_s
$$

for a Brownian motion $\bar B$ adapted to the reversed filtration. The
**only** unknown in the reversal is the score — hence §3. Song et al.
2021 additionally exhibit the **probability-flow ODE** $\dot x =
b - \tfrac12\sigma\sigma^\top\nabla_x\log p_t(x)$, a deterministic flow
with the *same marginals* $p_t$ — `21` Corollary 1.2 applied in reverse:
both dynamics solve the same forward equation.

**Proposition 4.2 (sanity check at depth C: the stationary OU case).**
Take §0's forward SDE with $X_0\sim N(0,I_n)$. By Proposition 2.1 the
process is stationary: $p_t = N(0,I_n)$ for all $t$, so $\nabla\log p_t(x)
= -x$. Anderson's formula then gives, with $b(x) = -x$ and
$\sigma\sigma^\top = 2I$:

$$
d\bar X_s = \big[x - 2x\big]\big|_{x=\bar X_s}\,ds + \sqrt2\,d\bar B_s = -\bar X_s\,ds + \sqrt2\,d\bar B_s
$$

— the reversed process solves the *same* OU equation. That is exactly
right: a stationary reversible diffusion (OU is the $V(x)=\tfrac12|x|^2$
case of `21` §0, whose Gibbs density $e^{-V}/Z$ is this Gaussian) is
statistically indistinguishable run forwards or backwards, and Anderson's
correction term is precisely what makes it so. $\blacksquare$

---

## 5. The Moving Frontier (all flagged current)

Read with `NOTATION_STOCH.md` §3.2's currency-check box in hand; verified
as the field's organizing references as of 2026-07.

- **Flow matching** (Lipman et al. 2023; the 2024 *FM Guide* is the de
  facto reference): learn a velocity field transporting noise to data
  along prescribed interpolation paths, regressing on conditional
  velocities — structurally §3's trick (replace an intractable marginal
  target by a tractable conditional one) applied to `ml-geometry/11`-style
  transport dynamics instead of score fields. Much of current practice
  has moved to this formulation; the probability-flow ODE of §4 is the
  bridge between the two.
- **Stochastic interpolants** (Albergo et al. 2023): the unifying frame
  containing score SDEs and flow matching as special cases.
- **Consistency models** (Song et al. 2023): distill the reverse dynamics
  into a one-step map — the sampling-cost frontier.
- **Schrödinger-bridge generative models** (De Bortoli et al. 2021):
  replace the fixed forward SDE by the entropic-OT bridge of `21` §4
  between data and noise; Sinkhorn (`ml-geometry/11` §4) reappears as the
  training loop's outer iteration.

---

## 6. Finance Weave (brief, and flagged as leads)

Two practitioner-adjacent hooks, both honest about maturity:

- **Deep hedging** (Buehler et al. 2019) is the settled end: replace the
  model-then-hedge pipeline of `20` §5 (whose $\Delta = \partial_SV$
  presumes the model) by directly learning the hedge from simulated
  paths under frictions. It is the conceptual licence for the next item:
  once the *hedge* is learned from a simulator, the simulator's realism
  is the binding constraint.
- **Generative market simulators** — diffusion/flow models trained on
  historical paths as replacements for parametric simulators — are an
  active, unsettled literature (Phase-3 register applies with full
  force). The DIML lead, extending `ml-geometry/16` §4's open list: the
  $\tilde y_{GBM}$ soft label (`20` §6.2) inherits GBM's known
  misspecifications (constant $\sigma$, no jumps — and `23` §6 adds:
  volatility is empirically *rough*); a learned simulator would trade
  that bias for a distribution-shift risk that is harder to state. The
  honest framing is `16`'s own: a direction made precisely askable, not
  a claim.

---

## References Used

- **Anderson 1982** — Theorem 4.1.
- **Hyvärinen 2005**; **Vincent 2011** — score matching; Theorem 3.1
  follows Vincent's argument.
- **Ho–Jain–Abbeel 2020**; **Song et al. 2021** — the founding practical
  papers (DDPM; continuous-time score SDE + probability-flow ODE).
- **Lipman et al. 2023**; **Lipman et al. 2024, FM Guide**; **Albergo et
  al. 2023**; **Song et al. 2023**; **De Bortoli et al. 2021** — §5's
  current landscape, all flagged per §3.2's register.
- **Buehler et al. 2019** — §6's deep-hedging anchor.
- **`stochastic/20`** (Itô formula, isometry — Proposition 2.1);
  **`stochastic/21`** (forward equation, Gibbs/OU stationarity —
  Proposition 4.2; Schrödinger bridge — §5); **`ml-geometry/10`** (score
  identities, §3's type-checks); **`notes/06`** Theorem 4.3 (the
  differentiation-under-the-integral step of Theorem 3.1).

Full bibliographic data: `stochastic/NOTATION_STOCH.md` §3.

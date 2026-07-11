# Notation and Standing Conventions — Phase 4 (Stochastic Analysis and Geometry)

**Role of this file.** This is the Phase-4 analogue of `notes/NOTATION.md`
and `ml-geometry/NOTATION_ML.md`. It does **not** restate the house style,
the pure-math typing context, the depth scale, or either existing citation
key — all remain in force exactly as written. This file adds only what
Phase 4 (stochastic analysis and its geometry) needs on top: the typed
objects of probability-in-continuous-time, a citation key split by maturity,
and a register note, since Phase 4 — unlike Phase 3 — spans settled
classical mathematics and genuinely young material in the same phase.

**Status.** Phase 4 is **complete**: this file, the hub
(`stochastic/00_stochastic_analysis_hub.md`), and the four deep dives
`20`–`23`. The phase was built map-first (`01` §1) — this file and the hub
came first; the deep dives carry the anchor proofs and the finance weave.

---

## 0. Inheritance Declaration

Every document in `stochastic/` is bound by, without restating:

- **`notes/NOTATION.md` §1, §1.1, §2, §4** — house style, depth scale,
  global typing context, pure-math citation key. In particular the
  measure-theoretic bindings of §2.4 are used constantly below: a
  probability space is the special case $(\Omega,\mathcal{F},\mathbb{P})$
  of `notes/06`'s $(\Omega,\mathcal{F},\mu)$ with $\mathbb{P}(\Omega)=1$.
- **`ml-geometry/NOTATION_ML.md` §1–§4** — Phase 4 reuses Phase-3 objects
  directly: the score $s_\theta$ (§1.1), $W_p$ and entropic OT/Sinkhorn
  (§1.2), retractions (§1.4), and the `DIML` cross-repo citation convention
  (§4) — `20` closes a typing gap in `16` §0.3 (see §5 below).

---

## 1. New Typing Context for Phase 4

### 1.1 Filtered probability spaces and processes

$$
(\Omega,\mathcal{F},\mathbb{P}) \quad \text{a probability space}, \qquad (\mathcal{F}_t)_{t\ge 0} \quad \text{a filtration}: \ \mathcal{F}_s\subseteq\mathcal{F}_t\subseteq\mathcal{F} \ \text{ for } s\le t.
$$

$$
X : [0,\infty)\times\Omega\to\mathbb{R}^n \quad \text{a stochastic process}, \qquad X_t := X(t,\cdot), \qquad X \text{ adapted} \iff X_t \text{ is } \mathcal{F}_t\text{-measurable } \forall t.
$$

$$
\tau : \Omega\to[0,\infty] \quad \text{a stopping time}: \ \{\tau\le t\}\in\mathcal{F}_t \ \forall t.
$$

$$
\mathbb{E}[\,\cdot\mid\mathcal{F}_t\big] \quad \text{conditional expectation — the } L^2\text{-projection of `01' §9.3/§13.9, reused, not redefined}.
$$

$$
X \text{ a martingale} \iff X \text{ adapted}, \ \mathbb{E}|X_t|<\infty \ \forall t, \ \text{ and } \mathbb{E}[X_t\mid\mathcal{F}_s]=X_s \ \forall s\le t.
$$

### 1.2 Brownian motion and quadratic variation

$$
B : [0,\infty)\times\Omega\to\mathbb{R}^d \quad \text{standard Brownian motion}: \quad B_0=0; \ \ B_t-B_s \sim N(0,(t-s)I_d) \text{ independent of } \mathcal{F}_s; \ \ t\mapsto B_t(\omega) \text{ continuous a.s.}
$$

$$
[X]_t := \lim_{\|\Pi\|\to0}\sum_{t_i\in\Pi} (X_{t_{i+1}}-X_{t_i})^2 \quad \text{(limit in probability over partitions } \Pi \text{ of } [0,t]\text{)}, \qquad [B^i]_t = t.
$$

$$
\int_0^t H_s\,dB_s \quad \text{the Itô integral}: \quad H \text{ adapted}, \ \mathbb{E}\!\int_0^t H_s^2\,ds<\infty \ \implies\ \int_0^t H\,dB \in L^2(\Omega), \ \text{ an } L^2\text{-martingale in } t,
$$

$$
\mathbb{E}\Big[\Big(\int_0^t H_s\,dB_s\Big)^2\Big] = \mathbb{E}\int_0^t H_s^2\,ds \quad \text{(Itô isometry — anchor of `20' Theorem 2.1; it is `01' §13.9's $L^2$ inner-product geometry doing the real work)}.
$$

### 1.3 SDEs, generators, and GBM

$$
dX_t = b(X_t)\,dt + \sigma(X_t)\,dB_t \quad \text{is notation for} \quad X_t = X_0 + \int_0^t b(X_s)\,ds + \int_0^t \sigma(X_s)\,dB_s,
$$

$$
b:\mathbb{R}^n\to\mathbb{R}^n, \qquad \sigma:\mathbb{R}^n\to\mathbb{R}^{n\times d} \quad \text{(Lipschitz — existence/uniqueness by the same Picard iteration as `01' §10's proof anchor, upgraded to process space)}.
$$

$$
\mathcal{L}f(x) := b(x)\cdot\nabla f(x) + \tfrac12\operatorname{tr}\big(\sigma(x)\sigma(x)^\top \nabla^2 f(x)\big), \qquad \mathcal{L} : C^2(\mathbb{R}^n)\to C^0(\mathbb{R}^n) \quad \text{the generator}.
$$

For $b=0$, $\sigma=I$: $\mathcal{L} = \tfrac12\Delta$ — Brownian motion's
generator is half the Laplacian, the Euclidean shadow of $\tfrac12\Delta_g$
(`00` §21).

$$
\text{GBM}: \quad dS_t = \mu S_t\,dt + \sigma S_t\,dB_t, \qquad S_t = S_0\exp\big((\mu-\tfrac{\sigma^2}{2})t + \sigma B_t\big), \qquad \mu\in\mathbb{R},\ \sigma>0
$$

— **geometric Brownian motion**, the process generating DIML's
$\tilde y_{GBM}$ soft labels (`ml-geometry/16` §0.3), typed here for the
first time in this repository (see §5).

### 1.4 Densities, Fokker–Planck, and Wasserstein gradient flow

$$
\rho_t \quad \text{the density of } \operatorname{Law}(X_t) \text{ w.r.t. } \mathcal{L}^n \text{ (when it exists)}, \qquad \partial_t\rho_t = \mathcal{L}^*\rho_t \quad \text{(Kolmogorov forward / Fokker–Planck; } \mathcal{L}^* \text{ the formal adjoint)}.
$$

$$
\mathcal{F}(\rho) := \int V\rho\,d\mathcal{L}^n + \int \rho\log\rho\,d\mathcal{L}^n \quad \text{free energy (potential + entropy)}, \qquad W_2 \quad \text{from `ml-geometry/NOTATION\_ML.md' §1.2}.
$$

$$
\rho_{k+1} := \operatorname*{arg\,min}_{\rho} \Big\{ \mathcal{F}(\rho) + \frac{1}{2\tau}W_2^2(\rho,\rho_k) \Big\} \quad \text{the JKO (minimizing-movement) scheme, step } \tau>0.
$$

### 1.5 Scores and time reversal

$$
p_t \quad \text{the density of a forward diffusion at time } t, \qquad \nabla_x \log p_t(x) \quad \text{the (Stein) score}.
$$

> **Type-check — two scores, one formula shape.** `ml-geometry/NOTATION_ML.md`
> §1.1's score is $s_\theta(x) = \nabla_\theta \log p_\theta(x)$ —
> differentiation in the *parameter*. The score above is $\nabla_x\log p_t(x)$
> — differentiation in the *state*. Same log-density, different slot;
> informal usage calls both "the score," and the distinction (Fisher score
> vs. Stein score) matters exactly when both appear in one argument, as in
> score-matching objectives (`22` §3).

$$
d\bar X_{\bar t} = \big[-b(\bar X_{\bar t}) + \sigma\sigma^\top\nabla_x\log p_{\bar t}(\bar X_{\bar t})\big]\,d\bar t + \sigma\,d\bar B_{\bar t} \quad \text{(reverse-time SDE, Anderson 1982 — the identity generative diffusion models run on)}.
$$

### 1.6 Path signatures

$$
X : [0,T]\to\mathbb{R}^d \quad \text{of bounded variation (the one-variable shadow of `notes/09''s } BV\text{)},
$$

$$
S(X) := \big(1,\ S^1(X),\ S^2(X),\ \dots\big), \qquad S^k(X) := \int_{0<t_1<\cdots<t_k<T} dX_{t_1}\otimes\cdots\otimes dX_{t_k} \ \in (\mathbb{R}^d)^{\otimes k}
$$

— the **signature**: iterated integrals, i.e. repeated `notes/03`-style line
integration of the coordinate 1-forms $dx^i$ along the path (Chen 1958).

$$
T\big((\mathbb{R}^d)\big) := \prod_{k\ge0}(\mathbb{R}^d)^{\otimes k} \quad \text{the (extended) tensor algebra, with product } \otimes, \qquad S(X) \in T\big((\mathbb{R}^d)\big).
$$

$$
X * Y \quad \text{concatenation of paths}: \ X:[0,s]\to\mathbb{R}^d,\ Y:[s,s+u]\to\mathbb{R}^d \text{ with } Y_s = X_s, \text{ run in sequence}.
$$

### 1.7 Mathematical finance (options-practitioner objects)

$$
r \ge 0 \quad \text{riskless rate}, \qquad \mathbb{Q}\sim\mathbb{P} \quad \text{an equivalent martingale (risk-neutral) measure}: \ e^{-rt}S_t \text{ is a } \mathbb{Q}\text{-martingale}.
$$

$$
C(K,T) := e^{-rT}\,\mathbb{E}_{\mathbb{Q}}\big[(S_T-K)^+\big] \quad \text{European call price}, \qquad (s-K)^+ := \max(s-K,0), \qquad K>0 \text{ strike}, \ T>0 \text{ expiry}.
$$

$$
q_T \quad \text{the risk-neutral density of } S_T \text{ under } \mathbb{Q} \text{ (when it exists)}, \qquad \sigma_{\mathrm{loc}}(K,T) \quad \text{local volatility (`21' §5)}, \qquad \sigma_{\mathrm{impl}}(K,T) \quad \text{Black–Scholes implied volatility}.
$$

$$
\tau_b := \inf\{t\ge0 : S_t \le b\} \quad \text{first-passage (barrier-hitting) time to level } b>0.
$$

---

## 2. Master Dictionary Supplement: Geometry/GMT Object $\to$ Stochastic Instantiation

Same table shape as `notes/NOTATION.md` §3 and `ml-geometry/NOTATION_ML.md`
§2; here the correspondence runs Phases 1–3 $\to$ Phase 4.

| Phases 1–3 object | Where built | Stochastic instantiation | Planned file |
|---|---|---|---|
| Chain rule / FTC ($d$, Stokes) | `notes/03`, `00` §16 | Itô's formula: $f(X_t)=f(X_0)+\int f'\,dX + \tfrac12\int f''\,d[X]$ — the chain rule acquires a second-order term | `20` |
| Laplace–Beltrami $\Delta_g$, heat equation | `00` §21 | generator $\tfrac12\Delta$ of Brownian motion; Kolmogorov forward/backward equations | `20` |
| Lipschitz/Rademacher (a.e. differentiability) | `notes/07` | Brownian paths: continuous, *nowhere* differentiable — the failure mode motivating Itô integration | `20` |
| $L^2$ projection, Hilbert geometry | `01` §13.9, §9.3 | conditional expectation, martingales, the Itô isometry | `20` |
| Gradient flow on a manifold | `notes/04`, `ml-geometry/13` | Fokker–Planck as the $W_2$-gradient flow of free energy (JKO/Otto) | `21` |
| Entropic OT / Sinkhorn | `ml-geometry/11` §4 | the Schrödinger bridge (static bridge $=$ entropic OT) | `21` |
| Fisher score $s_\theta$ | `ml-geometry/10` | Stein score $\nabla_x\log p_t$; score matching; reverse-time SDE | `22` |
| Line integrals of 1-forms | `notes/03`, `00` §5 | path signature (Chen iterated integrals) as a feature map for time series | `23` |

---

## 3. Citation Key — Phase 4

Split by maturity, as `ml-geometry/NOTATION_ML.md` §3 does.

### 3.1 Foundational / classic

- **Karatzas–Shreve** — I. Karatzas, S. Shreve, *Brownian Motion and
  Stochastic Calculus*, 2nd ed., Graduate Texts in Mathematics 113,
  Springer, 1991.
- **Le Gall** — J.-F. Le Gall, *Brownian Motion, Martingales, and
  Stochastic Calculus*, Graduate Texts in Mathematics 274, Springer, 2016.
- **Øksendal** — B. Øksendal, *Stochastic Differential Equations*, 6th ed.,
  Universitext, Springer, 2003.
- **Revuz–Yor** — D. Revuz, M. Yor, *Continuous Martingales and Brownian
  Motion*, 3rd ed., Grundlehren der mathematischen Wissenschaften 293,
  Springer, 1999.
- **Anderson 1982** — B. D. O. Anderson, "Reverse-time diffusion equation
  models," *Stochastic Processes and their Applications* 12(3), 1982.
- **JKO 1998** — R. Jordan, D. Kinderlehrer, F. Otto, "The Variational
  Formulation of the Fokker–Planck Equation," *SIAM Journal on Mathematical
  Analysis* 29(1), 1998.
- **Otto 2001** — F. Otto, "The Geometry of Dissipative Evolution
  Equations: the Porous Medium Equation," *Communications in Partial
  Differential Equations* 26(1–2), 2001.
- **AGS** — L. Ambrosio, N. Gigli, G. Savaré, *Gradient Flows in Metric
  Spaces and in the Space of Probability Measures*, 2nd ed., Lectures in
  Mathematics ETH Zürich, Birkhäuser, 2008.
- **Chen 1958** — K.-T. Chen, "Integration of Paths — A Faithful
  Representation of Paths by Noncommutative Formal Power Series,"
  *Transactions of the AMS* 89(2), 1958.
- **Lyons 1998** — T. Lyons, "Differential equations driven by rough
  signals," *Revista Matemática Iberoamericana* 14(2), 1998.
- **Hsu** — E. P. Hsu, *Stochastic Analysis on Manifolds*, Graduate Studies
  in Mathematics 38, AMS, 2002.
- **Black–Scholes 1973** — F. Black, M. Scholes, "The Pricing of Options
  and Corporate Liabilities," *Journal of Political Economy* 81(3), 1973.
- **Shreve, SCF II** — S. Shreve, *Stochastic Calculus for Finance II:
  Continuous-Time Models*, Springer Finance, Springer, 2004.
- **Fernholz** — E. R. Fernholz, *Stochastic Portfolio Theory*,
  Applications of Mathematics 48, Springer, 2002.
- **Léonard 2014** — C. Léonard, "A survey of the Schrödinger problem and
  some of its connections with optimal transport," *Discrete and Continuous
  Dynamical Systems A* 34(4), 2014.
- **Itô 1944** — K. Itô, "Stochastic Integral," *Proceedings of the
  Imperial Academy (Tokyo)* 20(8), 1944.
- **Merton 1973** — R. C. Merton, "Theory of Rational Option Pricing,"
  *Bell Journal of Economics and Management Science* 4(1), 1973.
- **Harrison–Pliska 1981** — J. M. Harrison, S. R. Pliska, "Martingales and
  Stochastic Integrals in the Theory of Continuous Trading," *Stochastic
  Processes and their Applications* 11(3), 1981.
- **Breeden–Litzenberger 1978** — D. T. Breeden, R. H. Litzenberger,
  "Prices of State-Contingent Claims Implicit in Option Prices," *Journal
  of Business* 51(4), 1978.
- **Dupire 1994** — B. Dupire, "Pricing with a Smile," *Risk* 7(1), 1994.
- **Gatheral, TVS** — J. Gatheral, *The Volatility Surface: A
  Practitioner's Guide*, Wiley Finance, Wiley, 2006.
- **BHLP 2013** — M. Beiglböck, P. Henry-Labordère, F. Penkner,
  "Model-independent bounds for option prices — a mass transport approach,"
  *Finance and Stochastics* 17(3), 2013.
- **Hyvärinen 2005** — A. Hyvärinen, "Estimation of Non-Normalized
  Statistical Models by Score Matching," *Journal of Machine Learning
  Research* 6, 2005.
- **Vincent 2011** — P. Vincent, "A Connection Between Score Matching and
  Denoising Autoencoders," *Neural Computation* 23(7), 2011.
- **Hambly–Lyons 2010** — B. Hambly, T. Lyons, "Uniqueness for the
  signature of a path of bounded variation and the reduced path group,"
  *Annals of Mathematics* 171(1), 2010.

### 3.2 Current (2020s) — anchor on the monograph/survey, treat individual results as supporting examples that may be superseded

- **Friz–Hairer** — P. Friz, M. Hairer, *A Course on Rough Paths: With an
  Introduction to Regularity Structures*, 2nd ed., Universitext, Springer,
  2020.
- **Song et al. 2021** — Y. Song, J. Sohl-Dickstein, D. P. Kingma, A.
  Kumar, S. Ermon, B. Poole, "Score-Based Generative Modeling through
  Stochastic Differential Equations," *ICLR*, 2021.
- **Ho–Jain–Abbeel 2020** — J. Ho, A. Jain, P. Abbeel, "Denoising Diffusion
  Probabilistic Models," *NeurIPS*, 2020.
- **De Bortoli et al. 2021** — V. De Bortoli, J. Thornton, J. Heng, A.
  Doucet, "Diffusion Schrödinger Bridge with Applications to Score-Based
  Generative Modeling," *NeurIPS*, 2021.
- **Chevyrev–Kormilitzin 2016** — I. Chevyrev, A. Kormilitzin, "A Primer on
  the Signature Method in Machine Learning," arXiv:1603.03788. *(Flag:
  arXiv-only survey; the standard entry point, likely to be superseded by a
  textbook treatment as the sub-field matures.)*
- **Lipman et al. 2023** — Y. Lipman, R. T. Q. Chen, H. Ben-Hamu, M.
  Nickel, M. Le, "Flow Matching for Generative Modeling," *ICLR*, 2023.
- **Lipman et al. 2024, FM Guide** — Y. Lipman, M. Havasi, P. Holderrieth,
  N. Shaul, M. Le, B. Karrer, R. T. Q. Chen, D. Lopez-Paz, H. Ben-Hamu, I.
  Gat, "Flow Matching Guide and Code," arXiv:2412.06264, 2024. *(Flag:
  arXiv guide; the field's current de facto reference for the
  flow-matching reformulation of diffusion models.)*
- **Albergo et al. 2023** — M. S. Albergo, N. M. Boffi, E. Vanden-Eijnden,
  "Stochastic Interpolants: A Unifying Framework for Flows and
  Diffusions," arXiv:2303.08797, 2023.
- **Song et al. 2023** — Y. Song, P. Dhariwal, M. Chen, I. Sutskever,
  "Consistency Models," *ICML*, 2023.
- **CGP 2021** — Y. Chen, T. T. Georgiou, M. Pavon, "Stochastic Control
  Liaisons: Richard Sinkhorn Meets Gaspard Monge on a Schrödinger Bridge,"
  *SIAM Review* 63(2), 2021.
- **Salvi et al. 2021** — C. Salvi, T. Cass, J. Foster, T. Lyons, W. Yang,
  "The Signature Kernel Is the Solution of a Goursat PDE," *SIAM Journal
  on Mathematics of Data Science* 3(3), 2021.
- **Kidger et al. 2020** — P. Kidger, J. Morrill, J. Foster, T. Lyons,
  "Neural Controlled Differential Equations for Irregular Time Series,"
  *NeurIPS*, 2020.
- **GJR 2018** — J. Gatheral, T. Jaisson, M. Rosenbaum, "Volatility Is
  Rough," *Quantitative Finance* 18(6), 2018. *(The founding paper of the
  rough-volatility literature.)*
- **BFG 2016** — C. Bayer, P. Friz, J. Gatheral, "Pricing under Rough
  Volatility," *Quantitative Finance* 16(6), 2016.
- **LNP 2020** — T. Lyons, S. Nejad, I. Perez Arribas, "Non-parametric
  Pricing and Hedging of Exotic Derivatives," *Applied Mathematical
  Finance* 27(6), 2020, pp. 457–494.
- **KLP 2020** — J. Kalsi, T. Lyons, I. Perez Arribas, "Optimal Execution
  with Rough Path Signatures," *SIAM Journal on Financial Mathematics*
  11(2), 2020.
- **Cuchiero et al. 2023** — C. Cuchiero, G. Gazzani, S. Svaluto-Ferro,
  "Signature-Based Models: Theory and Calibration," *SIAM Journal on
  Financial Mathematics* 14(3), 2023.
- **Cuchiero et al. 2025** — C. Cuchiero, G. Gazzani, J. Möller, S.
  Svaluto-Ferro, "Joint Calibration to SPX and VIX Options with
  Signature-Based Models," *Mathematical Finance* 35, 2025.
- **Buehler et al. 2019** — H. Buehler, L. Gonon, J. Teichmann, B. Wood,
  "Deep Hedging," *Quantitative Finance* 19(8), 2019.

> **Currency check.** The fast-moving entries above (flow matching and its
> guide, stochastic interpolants, consistency models, signature-based
> volatility models) were re-verified as the field's prominent references
> on **2026-07-11** by web search; the signature-volatility line remains
> actively moving (joint SPX/VIX calibration reached *Mathematical Finance*
> in 2025, with 2025–26 preprints extending it). Per the register rules,
> treat them as the current organizing references, not settled canon.

---

## 4. Register Note for Phase 4 — a Maturity Ladder, Not One Register

Phase 3 could use a single register ("young field, honest framing").
Phase 4 cannot — its four topics sit at different rungs, and each
document will say which rung it is on:

| File | Topic | Maturity | Register |
|---|---|---|---|
| `20` | Brownian motion, Itô calculus | settled classical (Itô 1944; canonical texts) | Phase-2 register: settled canon, cite the monographs |
| `21` | Fokker–Planck as $W_2$-gradient flow | settled-with-a-monograph (JKO 1998 $\to$ AGS 2008) | mostly settled; frontier at the edges |
| `22` | score-based diffusion models | genuinely young (2020s) | Phase-3 register: PhD-direction reconnaissance |
| `23` | signatures and rough paths | theory settled (Lyons 1998, Friz–Hairer); ML applications young | split register, stated per-section |

---

## 5. DIML Tie-In

`ml-geometry/16` §0.3 defines the soft label $\tilde y_{GBM}$ as a 200-path
first-passage frequency of simulated GBM paths — and GBM was, until this
phase, never typed anywhere in this repository, a gap by the house rules
this repo holds itself to. §1.3 above supplies the typed definition; `20`
Theorem 4.1 solves the process, and `20` §6.2 carries the first-passage
machinery (including the one-gate closed form usable as a unit test on
DIML's simulator, and the reason the four-gate oracle has none). `16`
§0.3 cross-references these directly — GBM is no longer an undefined
primitive anywhere in this repository. All `DIML` citations in Phase 4
follow the pinning convention of `ml-geometry/NOTATION_ML.md` §4
unchanged.

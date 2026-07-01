# 10 — Information Geometry: The Fisher–Rao Metric and Natural Gradient (PSTAT 126)

### Inspired by Karen Grigorian's PSTAT 126 course

**Cross-references.** `ml-geometry/NOTATION_ML.md` (typing conventions §1.1,
citation key §3.1); `notes/NOTATION.md` §2.3 (Riemannian metric $g$) and
`notes/04_riemannian_metrics_and_curvature.md` §1.1 (the musical isomorphism
$\sharp$, reused directly below — not redefined); `01_prerequisite_systematic_review_for_GMT_Topology.md`
§13.7 (the KL-divergence/Fisher-information connection already flagged there
without proof — proved here in full).

---

## 0. Standing Definitions

$$
\Theta\subseteq\mathbb{R}^p \text{ open}, \qquad \theta\in\Theta, \qquad p_\theta:\mathcal{X}\to[0,\infty), \qquad \int_{\mathcal{X}} p_\theta(x)\,d\mu(x) = 1 \ \ \forall \theta\in\Theta.
$$

$$
\mathcal{P}=\{p_\theta:\theta\in\Theta\} \quad \text{a statistical manifold}, \qquad \theta \text{ a global chart}, \qquad T_\theta\Theta\cong\mathbb{R}^p.
$$

$$
\ell(\theta;x) := \log p_\theta(x), \qquad s_\theta(x):=\nabla_\theta \ell(\theta;x)\in\mathbb{R}^p \quad \text{(the score)}.
$$

$$
g^F_\theta:T_\theta\Theta\times T_\theta\Theta\to\mathbb{R}, \qquad g^F_{ij}(\theta) := \mathbb{E}_{p_\theta}\big[\partial_i\ell(\theta;X)\,\partial_j\ell(\theta;X)\big] \quad \text{(`ml-geometry/NOTATION_ML.md' §1.1)}.
$$

**Standing regularity hypothesis** (used throughout this document): the
support of $p_\theta$ does not depend on $\theta$, and differentiation under
the integral sign with respect to $\theta$ is justified (e.g. by a
dominated-convergence argument, `notes/06` Theorem 4.3, applied to a
$\theta$-independent integrable envelope of $\partial_\theta p_\theta$ and
$\partial^2_\theta p_\theta$). This is stated once here because it is used
twice below without restatement.

---

## 1. The Statistical Manifold as a Riemannian Manifold

$\mathcal{P}$ is treated as a smooth $p$-manifold with $\theta$ as a global
chart (`notes/02` §2: a single chart trivially satisfies the smooth-atlas
condition). The question this document answers: what Riemannian metric
$g^F$ on $\mathcal{P}$ is the *canonical* one — not chosen arbitrarily, but
determined by the statistical structure itself?

> **Type-check.** $g^F_\theta$ as defined in §0 is manifestly a symmetric
> bilinear form on $T_\theta\Theta$ (symmetric because the product
> $\partial_i\ell\cdot\partial_j\ell$ is); positive-semidefiniteness follows
> because it is an expectation of an outer product $s_\theta s_\theta^\top$,
> hence PSD as a matrix; it is positive-definite exactly when the score
> components $\{\partial_i\ell(\theta;X)\}_{i=1}^p$ are linearly independent
> as random variables (the usual "identifiability" condition), which is
> assumed throughout. Under this assumption, $g^F$ is a genuine Riemannian
> metric in the sense of `notes/04` Definition 1.1.

---

## 2. Anchor Proof: Fisher Information Equals the Negative Expected Hessian of the Log-Likelihood

**Theorem 2.1.** Under the standing regularity hypothesis (§0),

$$
g^F_{ij}(\theta) = \mathbb{E}_{p_\theta}\big[\partial_i\ell(\theta;X)\,\partial_j\ell(\theta;X)\big] = -\mathbb{E}_{p_\theta}\big[\partial_i\partial_j\ell(\theta;X)\big].
$$

*Proof.* Start from the normalization identity, valid for every $\theta\in\Theta$:

$$
\int_{\mathcal{X}} p_\theta(x)\,d\mu(x) = 1.
$$

Differentiate both sides with respect to $\theta^i$. By the regularity
hypothesis, differentiation passes inside the integral:

$$
\int_{\mathcal{X}} \partial_i p_\theta(x)\,d\mu(x) = 0. \tag{$\ast$}
$$

Using $\partial_i p_\theta = p_\theta\,\partial_i\ell(\theta;\cdot)$ (chain
rule for $\ell=\log p_\theta$, valid where $p_\theta>0$), $(\ast)$ reads

$$
\int_{\mathcal{X}} p_\theta(x)\,\partial_i\ell(\theta;x)\,d\mu(x) = \mathbb{E}_{p_\theta}[s_\theta^i] = 0. \tag{1}
$$

This is the **zero-mean-score identity**, recorded for use in §4 below. Now
differentiate $(\ast)$ again with respect to $\theta^j$, again passing the
derivative inside the integral:

$$
\int_{\mathcal{X}} \partial_j\big(\partial_i p_\theta(x)\big)\,d\mu(x) = 0.
$$

Using $\partial_i p_\theta = p_\theta\,\partial_i\ell$ and the product rule,

$$
\partial_j(\partial_i p_\theta) = (\partial_j p_\theta)(\partial_i\ell) + p_\theta\,\partial_j\partial_i\ell = p_\theta\big[(\partial_j\ell)(\partial_i\ell) + \partial_i\partial_j\ell\big],
$$

using $\partial_j p_\theta = p_\theta\,\partial_j\ell$ again in the first
term. Substituting back,

$$
0 = \int_{\mathcal{X}} p_\theta(x)\Big[\partial_i\ell(\theta;x)\,\partial_j\ell(\theta;x) + \partial_i\partial_j\ell(\theta;x)\Big]\,d\mu(x) = \mathbb{E}_{p_\theta}[\partial_i\ell\,\partial_j\ell] + \mathbb{E}_{p_\theta}[\partial_i\partial_j\ell].
$$

Rearranging,

$$
\mathbb{E}_{p_\theta}[\partial_i\ell\,\partial_j\ell] = -\mathbb{E}_{p_\theta}[\partial_i\partial_j\ell],
$$

which is exactly $g^F_{ij}(\theta) = -\mathbb{E}_{p_\theta}[\partial_i\partial_j\ell(\theta;X)]$. $\blacksquare$

> **Type-check — what breaks without the regularity hypothesis.** If the
> support of $p_\theta$ depends on $\theta$ (e.g. $p_\theta=\mathrm{Uniform}(0,\theta)$),
> differentiating $\int p_\theta\,d\mu=1$ under the integral sign picks up an
> extra boundary term from the moving domain of integration (a
> Leibniz-integral-rule correction), and identity $(\ast)$ — hence Theorem
> 2.1 — fails. This is exactly the same "differentiate under the integral,
> watch what the hypothesis is buying you" discipline as `notes/06` Theorem
> 4.3's dominated-convergence hypothesis, instantiated here for a parametric
> family instead of a sequence.

**Corollary 2.2 (the second-order KL expansion, completing `01` §13.7).**
For $h\in\mathbb{R}^p$ small,

$$
D_{\mathrm{KL}}(p_\theta \,\|\, p_{\theta+h}) = \tfrac12\,h^\top g^F(\theta)\,h + o(\lVert h\rVert^2).
$$

*Proof sketch (depth B, building on Theorem 2.1).* Taylor-expand $\ell(\theta+h;x)$
to second order in $h$ inside $D_{\mathrm{KL}}(p_\theta\|p_{\theta+h}) =
\mathbb{E}_{p_\theta}[\ell(\theta;X)-\ell(\theta+h;X)]$: the zeroth-order term
cancels, the first-order term vanishes by the zero-mean-score identity (1)
applied at $\theta$, and the second-order term is $-\tfrac12 h^\top
\mathbb{E}_{p_\theta}[\nabla^2_\theta\ell(\theta;X)]h = \tfrac12h^\top
g^F(\theta)h$ by Theorem 2.1. $\blacksquare$

So $g^F$ is, up to the factor of $\tfrac12$, exactly the local quadratic
approximation to KL divergence — this is the precise sense in which "KL
divergence behaves like squared Riemannian distance for nearby parameters"
(`01` §13.7), and it is why $g^F$, rather than some other candidate metric,
is the canonical choice on $\mathcal{P}$: it is the metric whose induced
squared-distance function locally agrees with the statistically meaningful
notion of divergence between models.

---

## 3. Natural Gradient as the Musical Isomorphism $\sharp$

Given a loss $L:\Theta\to\mathbb{R}$ (e.g. negative log-likelihood or an
empirical risk), the ordinary gradient $\nabla_\theta L(\theta)$ treats
$\Theta\subseteq\mathbb{R}^p$ as flat Euclidean space. But $\mathcal{P}$
carries the metric $g^F$ constructed above, and $dL_\theta\in T_\theta^*\Theta$
is the primitive, chart-independent object (`notes/02` §5.2) — exactly as
`00` §17.1 and `notes/04` §1.1 distinguish $df$ from $\nabla f$.

**Definition 3.1 (natural gradient).** The **natural gradient** of $L$ at
$\theta$ is

$$
\tilde\nabla L(\theta) := \sharp_{g^F}\big(dL_\theta\big) = \big(g^F(\theta)\big)^{-1}\nabla_\theta L(\theta),
$$

i.e. $\sharp$ of `notes/04` §1.1's Proposition 1.2, instantiated with
$g=g^F$. This is not a new construction — it is the Riemannian gradient
(`ml-geometry/NOTATION_ML.md` §1.4) of $L$ on $(\mathcal{P},g^F)$, applying a
definition already fully proved once, here specialized to a particular
metric.

**Proposition 3.2 (reparametrization sensitivity).** Under a smooth
reparametrization $\phi:\Theta'\to\Theta$, the ordinary gradient transforms
by the Jacobian of $\phi^{-1}$ acting contravariantly, while the natural
gradient transforms exactly as a coordinate representation of the single
chart-independent object $\sharp_{g^F}(dL)$ should — i.e. the *direction* it
represents in $T_\theta\mathcal{P}$ is the same regardless of which
parametrization was used to compute it, whereas the Euclidean gradient's
direction (as a vector to be added to $\theta$) is not.

*Proof (depth B, direct from `notes/02` §4 Step 4's Jacobian
transformation law applied twice — once to $dL$, once to $g^F$, which cancel).*
Write $\theta=\phi(\theta')$. By the chain rule, $\nabla_{\theta'}L =
J_\phi^\top \nabla_\theta L$ where $J_\phi=\partial\theta/\partial\theta'$.
The Fisher metric transforms as a $(0,2)$-tensor under the same
reparametrization (direct from its definition as an expectation of a
product of score components, each of which transforms via $J_\phi$ by the
chain rule): $g^F(\theta') = J_\phi^\top g^F(\theta) J_\phi$. Hence

$$
\big(g^F(\theta')\big)^{-1}\nabla_{\theta'}L = \big(J_\phi^\top g^F(\theta)J_\phi\big)^{-1}J_\phi^\top\nabla_\theta L = J_\phi^{-1}\big(g^F(\theta)\big)^{-1}\nabla_\theta L,
$$

which is exactly the tangent-vector transformation law of `notes/02` §4 Step
4 applied to $\tilde\nabla L(\theta)\in T_\theta\Theta$ — i.e. the natural
gradient computed in either parametrization represents the *same* tangent
vector in $T_\theta\mathcal{P}$, transported correctly by $J_\phi^{-1}$; the
ordinary gradient does not enjoy this property because it is a covector
($dL\in T^*_\theta\Theta$) being silently treated as a vector by identifying
$\mathbb{R}^p$ with its own dual, which only respects the *Euclidean*
metric, not $g^F$. $\blacksquare$

> **Primitive-vs-derived, restated for this setting.** $dL_\theta$ is
> parametrization-independent by construction (`notes/02` §5.2). $\nabla_\theta L$
> is the Euclidean-metric-dependent shadow of $dL_\theta$; $\tilde\nabla L(\theta)$
> is the $g^F$-dependent shadow. Since $g^F$ is itself canonically determined
> by the statistical model (§2), $\tilde\nabla L$ is the more *statistically*
> meaningful of the two derived objects, even though both are equally valid
> "gradients" in the bare linear-algebra sense.

---

## 4. Consequence: Natural Gradient Descent

The natural gradient descent update is

$$
\theta_{k+1} = \theta_k - \eta\,\tilde\nabla L(\theta_k) = \theta_k - \eta\,\big(g^F(\theta_k)\big)^{-1}\nabla_\theta L(\theta_k),
$$

which is (at recognition depth here; full convergence theory for retraction-based
Riemannian descent is proved in `13` §3) steepest descent with respect to
$g^F$ rather than the Euclidean metric on $\Theta$. Amari 1998 originally
motivated this for training neural networks, arguing that the Euclidean
metric on weight space has no statistical meaning, while $g^F$ does; this
observation is what launched information geometry's role in the ML
literature (though the *field* of information geometry, per Amari–Nagaoka
and Amari IG, considerably predates its ML application and is a mature area
of differential-geometric statistics in its own right — not itself a young
field, unlike some of the other Phase-3 topics).

---

## References Used

- **Amari 1998** — "Natural Gradient Works Efficiently in Learning,"
  *Neural Computation* 10(2). The natural-gradient construction (§3–4).
- **Amari, IG**; **Amari–Nagaoka** — the Fisher–Rao metric and its
  canonical role on statistical manifolds (§1–2), and the mature,
  pre-ML-era theory of information geometry this document draws on.
- **Rao 1945** — the original identification of Fisher information as a
  Riemannian metric on a parameter space.
- **`notes/04_riemannian_metrics_and_curvature.md`** §1.1 — the musical
  isomorphism $\sharp$, reused without modification in §3.
- **`01_prerequisite_systematic_review_for_GMT_Topology.md`** §13.7 — the
  KL/Fisher connection stated there without proof, proved here as Corollary
  2.2.

Full bibliographic data: `ml-geometry/NOTATION_ML.md` §3.

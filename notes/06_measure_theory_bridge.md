# 06 — The Measure Theory Bridge

**Cross-references.** `notes/NOTATION.md` (typing conventions, §2.4 for the
measure-theoretic global context used throughout); `00_differential_geometry_foundations_for_GMT_Topology.md`
§24–25 (survey-level motivation for measure theory and Hausdorff measure);
`01_prerequisite_systematic_review_for_GMT_Topology.md` §8 (measure theory
map, DCT stated as a proof anchor there and proved here in full); feeds
`notes/07_rectifiability_and_hausdorff_measure.md` (rectifiable sets are
defined using $\mathcal{H}^k$, constructed here) and `notes/09_varifolds_and_finite_perimeter.md`
(Radon measures, BV).

---

## 0. Standing Definitions

$$
\Omega \quad \text{a nonempty set}, \qquad \mathcal{F} \subseteq 2^\Omega \quad \sigma\text{-algebra}: \ \varnothing\in\mathcal{F},\ A\in\mathcal{F}\Rightarrow A^c\in\mathcal{F},\ \{A_i\}_{i=1}^\infty\subseteq\mathcal{F}\Rightarrow\bigcup_i A_i\in\mathcal{F}.
$$

$$
\mu : \mathcal{F} \to [0,\infty] \quad \text{a measure}: \quad \mu(\varnothing)=0, \qquad \mu\Big(\bigsqcup_{i=1}^\infty A_i\Big) = \sum_{i=1}^\infty \mu(A_i) \ \text{(countable additivity, disjoint } A_i\text{)}.
$$

$$
f : \Omega \to [-\infty,\infty] \quad \mathcal{F}\text{-measurable}: \qquad f^{-1}((a,\infty]) \in \mathcal{F} \ \ \forall a\in\mathbb{R}.
$$

$$
\mu^* : 2^X \to [0,\infty] \quad \text{an outer measure}: \quad \mu^*(\varnothing)=0, \quad A\subseteq B\Rightarrow\mu^*(A)\le\mu^*(B), \quad \mu^*\Big(\bigcup_{i=1}^\infty A_i\Big)\le\sum_{i=1}^\infty\mu^*(A_i).
$$

---

## 1. Why Measurability Is a Wider Frame Than Smoothness

Smooth objects (§`00` Ch. 2–23) are highly structured: locally
diffeomorphic to $\mathbb{R}^n$, differentiable to all orders. Measurable
objects need only enough structure to define size and integration. The key
philosophical inclusion:

$$
\{\text{smooth geometry}\} \ \subsetneq\ \{\text{measurable geometry}\}.
$$

This section builds the measure-theoretic machinery ($\sigma$-algebras,
measures, the Lebesgue integral, its convergence theorems, and finally
Hausdorff measure) that makes this inclusion precise and usable. Just as the
Lebesgue integral extends the reach of the Riemann integral by working with
measurable sets rather than requiring near-continuity, GMT extends the reach
of smooth differential geometry by working with measurable/rectifiable
structure rather than requiring smoothness.

---

## 2. Outer Measures and the Carathéodory Construction

**Definition 2.1 (Carathéodory measurability).** Given an outer measure
$\mu^*$ on $X$, a set $A\subseteq X$ is **$\mu^*$-measurable** if

$$
\mu^*(S) = \mu^*(S\cap A) + \mu^*(S\setminus A) \qquad \text{for every } S\subseteq X.
$$

**Theorem 2.2 (Carathéodory's theorem).** The collection $\mathcal{F}$ of
$\mu^*$-measurable sets is a $\sigma$-algebra, and $\mu^*|_{\mathcal{F}}$ is a
(countably additive) measure.

*(Standard; e.g. Evans–Gariepy, MTFP, Thm 1.9, or Rudin, RCA, Thm 2.20-adjacent
constructions. Proof is a careful but mechanical verification of closure
under complement and countable union directly from Definition 2.1, together
with additivity on disjoint measurable sets via a telescoping argument; not
reproduced here at depth C per `01` §17 — it is exactly the kind of
"construction machinery" that should be used at depth B until it becomes the
object of study itself, which it does not here.)*

This construction is used twice in this repository: to build Lebesgue
measure $\mathcal{L}^n$ from the outer measure $\mathcal{L}^{n,*}(A) :=
\inf\{\sum_i |Q_i| : A\subseteq\bigcup_iQ_i,\ Q_i \text{ cubes}\}$, and (§5
below) to build Hausdorff measure $\mathcal{H}^k$ from a diameter-based outer
measure — the *same* theorem, applied to two different outer measures.

---

## 3. Measurable Functions and the Lebesgue Integral

**Definition 3.1 (simple function).** $s = \sum_{i=1}^m c_i\,\mathbf{1}_{A_i}$,
$c_i\in[0,\infty)$, $A_i\in\mathcal{F}$ disjoint. Define $\int_\Omega s\,d\mu
:= \sum_i c_i\,\mu(A_i)$.

**Definition 3.2 (Lebesgue integral of a nonnegative measurable function).**
For $f:\Omega\to[0,\infty]$ measurable,

$$
\int_\Omega f\,d\mu := \sup\left\{\int_\Omega s\,d\mu \ : \ s \text{ simple}, \ 0\le s\le f\right\}.
$$

For general measurable $f$, write $f = f^+-f^-$ ($f^\pm := \max(\pm f,0)$)
and define $\int f\,d\mu := \int f^+\,d\mu - \int f^-\,d\mu$ when at least one
term is finite; $f\in L^1(\mu)$ if both are finite.

> **Type-check.** The Riemann integral partitions the *domain* into small
> intervals and asks the function to be nearly constant there (forcing
> near-continuity). The Lebesgue integral partitions the *range* into small
> intervals and asks only that the preimage of each range-interval be
> measurable (Definition 3.2 via simple functions is exactly this, dualized).
> This is the single structural change that lets the Lebesgue integral handle
> far more irregular functions and, crucially, interact well with limits
> (§4) — a Riemann-integrable analogue of the theorems below simply is not
> true in comparable generality.

---

## 4. Convergence Theorems and the Anchor Proof: Dominated Convergence

**Theorem 4.1 (Monotone Convergence Theorem, MCT).** If $f_n:\Omega\to[0,\infty]$
measurable, $f_n \le f_{n+1}$ for all $n$, and $f_n \to f$ pointwise, then
$\int_\Omega f_n\,d\mu \to \int_\Omega f\,d\mu$.

*(Evans–Gariepy, MTFP, Thm 1.14; Rudin, RCA, Thm 1.26. Standard; used below
without reproof, as it is the base case the other convergence theorems
reduce to.)*

**Lemma 4.2 (Fatou's Lemma).** If $f_n : \Omega\to[0,\infty]$ measurable,
then $\int_\Omega \liminf_n f_n\,d\mu \le \liminf_n \int_\Omega f_n\,d\mu$.

*Proof.* Let $g_n := \inf_{k\ge n} f_k$, so $g_n \le f_n$, $g_n$ increases to
$\liminf_n f_n$. By MCT, $\int g_n \,d\mu\to \int \liminf_n f_n\,d\mu$. Since
$g_n\le f_k$ for all $k\ge n$, $\int g_n\,d\mu \le \inf_{k\ge n}\int
f_k\,d\mu \le \int f_n \, d\mu$; taking $\liminf_n$ of both sides,
$\int \liminf_n f_n \,d\mu = \lim_n\int g_n\,d\mu \le \liminf_n \int
f_n\,d\mu$. $\blacksquare$

### Anchor Theorem: Dominated Convergence

**Theorem 4.3 (Dominated Convergence Theorem, DCT).** Let $f_n:\Omega\to
[-\infty,\infty]$ be measurable with $f_n\to f$ $\mu$-a.e., and suppose there
is $g\in L^1(\mu)$, $g\ge 0$, with $|f_n|\le g$ $\mu$-a.e. for every $n$.
Then $f\in L^1(\mu)$ and

$$
\int_\Omega f_n\,d\mu \ \longrightarrow\ \int_\Omega f\,d\mu, \qquad \text{equivalently} \qquad \int_\Omega |f_n-f|\,d\mu \to 0.
$$

*Proof.* Modifying on a $\mu$-null set does not change any integral, so
assume the convergence and domination hold everywhere (not just a.e.). Since
$|f_n|\le g$ for all $n$ and $f_n\to f$ pointwise, $|f|\le g$ too, so $f\in
L^1(\mu)$ (as $\int|f|\,d\mu \le \int g\,d\mu <\infty$).

Apply Fatou's Lemma (Lemma 4.2) to the nonnegative sequence $g+f_n \ge 0$
(nonnegative since $|f_n|\le g$):

$$
\int_\Omega (g+f)\,d\mu = \int_\Omega \liminf_n (g+f_n)\,d\mu \le \liminf_n \int_\Omega (g+f_n)\,d\mu = \int_\Omega g\,d\mu + \liminf_n \int_\Omega f_n\,d\mu.
$$

Since $\int g\,d\mu <\infty$, subtract it from both sides:

$$
\int_\Omega f\,d\mu \ \le\ \liminf_n \int_\Omega f_n\,d\mu. \tag{i}
$$

Apply Fatou's Lemma again to the nonnegative sequence $g - f_n \ge 0$:

$$
\int_\Omega (g-f)\,d\mu \le \liminf_n \int_\Omega (g-f_n)\,d\mu = \int_\Omega g\,d\mu - \limsup_n \int_\Omega f_n\,d\mu,
$$

using $\liminf_n(-a_n) = -\limsup_n a_n$. Subtracting $\int g\,d\mu$ from both
sides (again finite):

$$
-\int_\Omega f\,d\mu \ \le\ -\limsup_n \int_\Omega f_n\,d\mu
\quad\Longleftrightarrow\quad
\limsup_n \int_\Omega f_n\,d\mu \ \le\ \int_\Omega f\,d\mu. \tag{ii}
$$

Combining (i) and (ii): $\limsup_n \int f_n\,d\mu \le \int f\,d\mu \le
\liminf_n \int f_n\,d\mu$. Since always $\liminf \le \limsup$, all three are
equal, so $\lim_n \int_\Omega f_n\,d\mu$ exists and equals $\int_\Omega
f\,d\mu$. The $L^1$-norm statement follows by the same argument applied to
$|f_n - f| \le 2g$, which $\to 0$ pointwise. $\blacksquare$

*(This Fatou-based proof is the standard one; cf. Evans–Gariepy, MTFP, Thm
1.19; Rudin, RCA, Thm 1.34.)*

> **Why this is "the engine."** MCT and DCT are theorems about when
> $\lim_n\int f_n = \int\lim_n f_n$ — i.e. when a limit and an integral
> commute. This is not automatic (a classic counterexample: tall thin spikes
> of height $n$ and width $1/n$ marching off to infinity converge pointwise
> to $0$ but each has integral $1$ — no dominating $g\in L^1$ exists). Every
> subsequent construction in this repository that says "pass to a limit
> under the integral sign" — weak convergence of measures/currents/varifolds
> (`notes/08`, `notes/09`), the area formula (`notes/07` §3) — is,
> underneath, an appeal to a theorem in this family.

---

## 5. Hausdorff Measure

### 5.1 Construction

**Definition 5.1.** For $A\subseteq\mathbb{R}^n$, $k\ge 0$, $\delta>0$, define

$$
\mathcal{H}_\delta^k(A) := \inf\left\{ \sum_{i=1}^\infty \alpha_k\Big(\frac{\operatorname{diam}(C_i)}{2}\Big)^k \ : \ A\subseteq\bigcup_{i=1}^\infty C_i,\ \operatorname{diam}(C_i)\le\delta \right\},
$$

where $\alpha_k := \pi^{k/2}/\Gamma(k/2+1)$ is a normalizing constant (chosen
so that $\mathcal{H}^n = \mathcal{L}^n$ on $\mathbb{R}^n$ for integer $k=n$;
see Theorem 5.4). Since shrinking $\delta$ only restricts the allowed
covers, $\mathcal{H}^k_\delta(A)$ is non-decreasing as $\delta\downarrow 0$;
define

$$
\mathcal{H}^k(A) := \lim_{\delta\to 0^+} \mathcal{H}^k_\delta(A) = \sup_{\delta>0}\mathcal{H}^k_\delta(A) \ \in [0,\infty].
$$

**Proposition 5.2.** $\mathcal{H}^k$ is an outer measure on $\mathbb{R}^n$
(countable subadditivity is inherited from the infimum-over-covers
definition, monotonicity is immediate), and (via Carathéodory's theorem,
Theorem 2.2) restricts to a genuine countably additive measure on the
$\mathcal{H}^k$-measurable sets, which include all Borel sets (a further
standard fact: $\mathcal{H}^k$ is a *metric* outer measure, i.e.
$\mathcal{H}^k(A\cup B) = \mathcal{H}^k(A)+\mathcal{H}^k(B)$ whenever
$\operatorname{dist}(A,B)>0$, and metric outer measures always make Borel
sets measurable — Carathéodory's criterion; Evans–Gariepy, MTFP, Thm 1.9 and
the surrounding discussion).

> **Why take $\delta\to 0$ first?** Using arbitrarily large covering pieces
> (no $\delta$ restriction) can badly *underestimate* $k$-dimensional size —
> e.g. a single huge ball covers a short curve "for free" using a
> low-dimensional-looking large diameter. Forcing $\operatorname{diam}(C_i)
> \le \delta$ and then sending $\delta \to 0$ forces the covering pieces to
> become small and to actually resolve the local $k$-dimensional structure
> of $A$, which is exactly what makes $\mathcal{H}^k$ behave like a genuine
> $k$-dimensional size rather than a crude outer bound.

### 5.2 Anchor Proof: $\mathcal{H}^1([0,L]) = L$

**Theorem 5.3.** For an interval $[0,L]\subseteq\mathbb{R}$, $\mathcal{H}^1([0,L]) = L$.

*Proof.* Here $k=1$, $\alpha_1 = \pi^{1/2}/\Gamma(3/2) = \pi^{1/2}/(\tfrac12
\sqrt\pi) = 2$, so $\alpha_1(\operatorname{diam}(C_i)/2)^1 =
\operatorname{diam}(C_i)$: for $k=1$, the normalizing constant makes the
"cost" of covering piece $C_i$ exactly its diameter, matching ordinary length.

*Upper bound ($\mathcal{H}^1([0,L])\le L$).* For any $\delta>0$, partition
$[0,L]$ into $m = \lceil L/\delta\rceil$ consecutive closed subintervals
$C_1,\dots,C_m$ each of length $\le\delta$ (so $\operatorname{diam}(C_i)\le
\delta$) with $\sum_i \operatorname{diam}(C_i) = L$ exactly. This is an
admissible $\delta$-cover, so $\mathcal{H}^1_\delta([0,L]) \le \sum_i
\operatorname{diam}(C_i) = L$. Since this holds for every $\delta>0$,
$\mathcal{H}^1([0,L]) = \lim_{\delta\to 0}\mathcal{H}^1_\delta([0,L]) \le L$.

*Lower bound ($\mathcal{H}^1([0,L])\ge L$).* Let $\{C_i\}_{i=1}^\infty$ be
any countable cover of $[0,L]$ with $\operatorname{diam}(C_i)\le\delta$. Each
$C_i$ is contained in a closed interval $\overline{C_i}$ of length exactly
$\operatorname{diam}(C_i)$ (the smallest interval containing $C_i$). By
countable subadditivity of ordinary Lebesgue outer measure applied directly
to the countable cover $\{\overline{C_i}\}$ of $[0,L]$ — no compactness
argument is needed (and none is available as stated: the $\overline{C_i}$
are closed, and finite subcovers are only guaranteed for *open* covers) —

$$
L = \mathcal{L}^1([0,L]) \le \sum_{i=1}^\infty \mathcal{L}^1(\overline{C_i}) = \sum_{i=1}^\infty \operatorname{diam}(C_i).
$$

(This uses the standard fact $\mathcal{L}^1([0,L]) = L$ for Lebesgue outer
measure and its countable subadditivity — the $n=1$ case of the Lebesgue
measure construction via Theorem 2.2, taken as known background per the
"safe to black-box" list in `01` §17.) Taking the infimum over all such
covers, $\mathcal{H}^1_\delta([0,L]) \ge L$ for every $\delta>0$, hence
$\mathcal{H}^1([0,L]) \ge L$.

Combining both bounds, $\mathcal{H}^1([0,L]) = L$. $\blacksquare$

*(Evans–Gariepy, MTFP, §2.1, works this case and the general
$\mathcal{H}^n=\mathcal{L}^n$ identification; Falconer, Ch. 2–3, gives the
$\alpha_k$-normalization motivation and further worked examples, e.g. fractal
sets with non-integer Hausdorff dimension.)*

**Theorem 5.4 (stated, not reproved).** $\mathcal{H}^n = \mathcal{L}^n$ on
$\mathbb{R}^n$ (same normalization argument, generalized — Evans–Gariepy,
MTFP, Thm 2.2). More generally, for a $C^1$ curve $\gamma:[a,b]\to\mathbb{R}^n$,
$\mathcal{H}^1(\gamma([a,b])) = L(\gamma) = \int_a^b\lVert\gamma'(t)\rVert\,dt$
(`00` §4) — Hausdorff measure recovers the classical arc length, justifying
the claim in `00` §25 that $\mathcal{H}^k$ is "the measure-theoretic
replacement for length, area, and volume."

### 5.3 Radon Measures

**Definition 5.5.** A Radon measure on $\mathbb{R}^n$ is a Borel measure that
is finite on compact sets, outer regular ($\mu(A) = \inf\{\mu(U):U\supseteq
A \text{ open}\}$), and inner regular on open sets. Both $\mathcal{L}^n$ and
$\mathcal{H}^k$ restricted to sets where it is $\sigma$-finite are Radon.
Radon measures are the natural measure objects for weak-$*$ convergence
and are exactly the objects a current's mass measure (`notes/08`)
or a varifold (`notes/09`) is built from.

> **Type-check — weak vs. weak-$*$ convergence.** `01` §9.4 defines *weak*
> convergence in a normed space $X$: $x_n \rightharpoonup x$ iff
> $\ell(x_n)\to\ell(x)$ for every $\ell\in X^*$. *Weak-$*$* convergence lives
> one dual up: for elements $\mu_n$ of a dual space $X^*$ — and Radon
> measures *are* a dual space, $\mathcal{M}(\mathbb{R}^n) \cong
> C_c(\mathbb{R}^n)^*$ by the Riesz representation theorem (black-boxed per
> `01` §17) — one tests against the *predual*, not the double dual:
> $\mu_n \overset{*}{\rightharpoonup} \mu$ iff $\int f\,d\mu_n \to \int
> f\,d\mu$ for every $f\in C_c(\mathbb{R}^n)$. Every convergence of measures,
> currents, or varifolds in this repository (`notes/08`–`09`, and the
> coupling compactness of `ml-geometry/11` §1) is weak-$*$: pairing against
> test functions/forms/fields. The payoff is compactness (Banach–Alaoglu,
> black-boxed): a mass-bounded sequence in a dual space has a weak-$*$
> convergent subsequence — the engine behind the Federer–Fleming compactness
> theorem cited in `notes/08` §3.

---

## References Used

- **Evans–Gariepy, MTFP** — Ch. 1 (Thm 1.9 Carathéodory's theorem and metric
  outer measures, Thm 1.14 MCT, Thm 1.19 DCT — the anchor proof above
  follows this source's Fatou-based argument), §2.1 (Hausdorff measure
  construction, the $\mathcal{H}^1([0,L])=L$ computation, Thm 2.2 for
  $\mathcal{H}^n=\mathcal{L}^n$).
- **Rudin, RCA** — Ch. 1–2 (Thm 1.26 MCT, Thm 1.34 DCT, alternative
  presentation cross-checked against Evans–Gariepy above).
- **Falconer, Geometry of Fractal Sets** — Ch. 2–3 (Hausdorff measure and
  dimension, motivation for the $\alpha_k$ normalization and non-integer-$k$
  examples).

Full bibliographic data: `notes/NOTATION.md` §4.

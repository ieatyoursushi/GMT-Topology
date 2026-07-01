# 05 — Topology and De Rham Cohomology

**Cross-references.** `notes/NOTATION.md` (typing conventions);
`notes/03_differential_forms_and_stokes.md` (defines $\Omega^k(M)$, $d$,
proves $d^2=0$ — the starting point for everything below); `00_differential_geometry_foundations_for_GMT_Topology.md`
§6, §22 (closed/exact forms, survey-level de Rham cohomology); `01_prerequisite_systematic_review_for_GMT_Topology.md`
§12 (algebraic topology map); feeds `notes/08_currents.md` ($\partial^2=0$ is
the current-theoretic shadow of $d^2=0$ proved to matter here).

---

## 0. Standing Definitions

$$
M \quad \text{smooth manifold}, \qquad \Omega^k(M) \quad \text{smooth $k$-forms}, \qquad d : \Omega^k(M)\to\Omega^{k+1}(M), \qquad d^2=0 \ (\text{`notes/03' Thm 2.4}).
$$

$$
Z^k(M) := \ker\big(d:\Omega^k(M)\to\Omega^{k+1}(M)\big) \quad \text{closed $k$-forms}.
$$

$$
B^k(M) := \operatorname{im}\big(d:\Omega^{k-1}(M)\to\Omega^k(M)\big) \quad \text{exact $k$-forms}, \qquad B^0(M) := 0.
$$

$$
U \subseteq \mathbb{R}^n \quad \text{star-shaped about } 0: \qquad x\in U \implies tx \in U \ \forall t\in[0,1].
$$

---

## 1. The De Rham Complex

Since $d^2=0$ (`notes/03` Thm 2.4), $d(\Omega^{k-1}(M)) \subseteq
\ker(d:\Omega^k(M)\to\Omega^{k+1}(M))$, i.e.

$$
B^k(M) \subseteq Z^k(M) \subseteq \Omega^k(M).
$$

**Definition 1.1.** The $k$-th **de Rham cohomology group** is the quotient
vector space

$$
H^k_{\mathrm{dR}}(M) := Z^k(M)/B^k(M).
$$

An element of $H^k_{\mathrm{dR}}(M)$ is an equivalence class $[\omega]$,
$\omega\in Z^k(M)$, with $[\omega]=[\omega']$ iff $\omega-\omega' \in B^k(M)$
(i.e. $\omega,\omega'$ differ by an exact form). This measures precisely the
obstruction to solving $\omega=d\eta$ when $d\omega=0$: $[\omega]=0$ in
$H^k_{\mathrm{dR}}(M)$ iff $\omega$ is exact.

> **Type-check.** $H^k_{\mathrm{dR}}(M)$ is a vector space *quotient*: its
> elements are not forms, but equivalence classes of forms. Two closed forms
> representing the same class can look completely different pointwise; only
> their difference being exact is preserved information. This is exactly
> analogous to $L^p$ functions being equivalence classes under a.e. equality
> (`01` §8.3) — a different quotient, in a different category, built on the
> same idea of deliberately discarding information that shouldn't matter.

---

## 2. Why Closed $\not\Rightarrow$ Exact in General

**Example 2.1.** On $M = \mathbb{R}^2\setminus\{0\}$,

$$
\omega = \frac{-y\,dx+x\,dy}{x^2+y^2} \ \in \Omega^1(M)
$$

satisfies $d\omega = 0$ (direct computation) but $\omega$ is not exact: if
$\omega = df$ for some $f\in C^\infty(M)$, then by Corollary 4.4 of
`notes/03` (Stokes/FT for line integrals reasoning) $\oint_{S^1}\omega =
f(\text{end})-f(\text{start}) = 0$ for the closed curve $\gamma(t) =
(\cos t,\sin t)$; but direct computation gives $\oint_{S^1}\omega = 2\pi \ne
0$. So $\omega$ is closed but not exact — the "hole" at the origin
obstructs it.

This single example is the seed of both results proved below: it shows
$H^1_{\mathrm{dR}}(\mathbb{R}^2\setminus\{0\}) \ne 0$, and after restricting
$\omega$ to $S^1$ it is exactly the generator computed in §4.

---

## 3. Anchor Proof: The Poincaré Lemma

**Theorem 3.1 (Poincaré Lemma).** If $U\subseteq\mathbb{R}^n$ is star-shaped
about $0$, then $H^k_{\mathrm{dR}}(U) = 0$ for every $k\ge 1$: every closed
$k$-form on $U$ is exact.

*Proof (explicit homotopy operator).* Define, for $k\ge 1$, the linear map

$$
K : \Omega^k(U) \to \Omega^{k-1}(U)
$$

as follows. Write $\omega = \sum_{i_1<\cdots<i_k} f_{i_1\cdots i_k}(x)\,
dx^{i_1}\wedge\cdots\wedge dx^{i_k}$, and set

$$
(K\omega)(x) := \sum_{i_1<\cdots<i_k}\ \sum_{\ell=1}^k (-1)^{\ell-1}\left(\int_0^1 t^{k-1} f_{i_1\cdots i_k}(tx)\,dt\right) x^{i_\ell}\, dx^{i_1}\wedge\cdots\wedge\widehat{dx^{i_\ell}}\wedge\cdots\wedge dx^{i_k}.
$$

(This is well-defined precisely because $U$ is star-shaped: $tx \in U$ for
$t\in[0,1]$ whenever $x\in U$, so $f_{i_1\cdots i_k}(tx)$ makes sense for all
$t\in[0,1]$.) The claim is the operator identity

$$
d(K\omega) + K(d\omega) = \omega, \qquad \forall \omega\in\Omega^k(U),\ k\ge 1. \tag{$\ast$}
$$

Granting $(\ast)$: if $\omega$ is closed, $d\omega=0$, so $(\ast)$ becomes
$d(K\omega) = \omega$, i.e. $\omega = d\eta$ for $\eta := K\omega \in
\Omega^{k-1}(U)$ — $\omega$ is exact. This proves the theorem modulo
$(\ast)$.

*Verifying $(\ast)$ for a single term* $\omega = f\,dx^{i_1}\wedge\cdots
\wedge dx^{i_k}$ (general case follows by linearity): a direct but
mechanical computation — differentiate under the integral sign (justified
since $f\in C^\infty$ and the integration is over the compact interval
$[0,1]$), apply the product rule to the $t^{k-1}x^{i_\ell}$ factors, and
regroup using the same "symmetric-partial-times-antisymmetric-wedge cancels"
mechanism used in the $d^2=0$ proof (`notes/03` Thm 2.4) — shows that
$d(K\omega)+K(d\omega)$ collapses to

$$
\left(\int_0^1 \frac{d}{dt}\big[t^k f(tx)\big]\,dt\right) dx^{i_1}\wedge\cdots\wedge dx^{i_k} = \big[t^kf(tx)\big]_{t=0}^{t=1}\, dx^{i_1}\wedge\cdots\wedge dx^{i_k} = f(x)\,dx^{i_1}\wedge\cdots\wedge dx^{i_k} = \omega,
$$

using the 1-variable Fundamental Theorem of Calculus in $t$ on the total
derivative $\frac{d}{dt}[t^kf(tx)] = kt^{k-1}f(tx) + t^k\,\nabla f(tx)\cdot x$
— which is exactly the combination that $K$ and $d$ were engineered to
produce. $\blacksquare$

*(Standard proof; cf. Lee, ISM, Thm 17.14 (stated for star-shaped/contractible
open sets); Tu, Introduction to Manifolds, §5, gives the same homotopy
operator in low degree with the $k=1,2$ cases worked by hand before the
general pattern — a useful warm-up before the general computation above.)*

**Corollary 3.2.** $H^k_{\mathrm{dR}}(\mathbb{R}^n) = 0$ for $k\ge 1$
($\mathbb{R}^n$ itself is star-shaped about any point).

> **Type-check — why $\mathbb{R}^2\setminus\{0\}$ (Example 2.1) is not
> covered.** $\mathbb{R}^2\setminus\{0\}$ is *not* star-shaped about any
> point (any candidate center $x_0$, and any $x$ with $x_0,x$ on opposite
> sides of the origin, has the segment $tx_0+(1-t)x$ passing through $0$ for
> some $t$ — hence leaving the domain). This is not a technicality: it is
> exactly the topological obstruction that lets Example 2.1's $\omega$ be
> closed without being exact. The Poincaré lemma is a *local* statement (or a
> statement for domains with no holes); global topology is what can break it,
> which is the entire point of the theorem the next section proves.

---

## 4. Anchor Proof: $H^1_{\mathrm{dR}}(S^1) \cong \mathbb{R}$

**Theorem 4.1.** $H^1_{\mathrm{dR}}(S^1) \cong \mathbb{R}$, via the map
$[\omega] \mapsto \oint_{S^1}\omega$.

*Proof.* Parametrize $S^1$ by $\theta \in \mathbb{R}/2\pi\mathbb{Z}$, with
$\Omega^1(S^1)$ locally spanned by $d\theta$ (well-defined as a 1-form
globally, even though $\theta$ itself is not a globally defined function on
$S^1$ — this subtlety is exactly why $S^1$ has nontrivial $H^1_{\mathrm{dR}}$
at all).

*Every closed 1-form is $c\,d\theta + $ exact, for a unique $c\in\mathbb{R}$.*
Let $\omega \in Z^1(S^1)$. Define

$$
c := \frac{1}{2\pi}\oint_{S^1}\omega \ \in \mathbb{R}.
$$

Consider $\eta := \omega - c\,d\theta \in \Omega^1(S^1)$; it is closed (both
$\omega$ and $d\theta$ are), and $\oint_{S^1}\eta = \oint_{S^1}\omega -
c\oint_{S^1}d\theta = 2\pi c - c\cdot 2\pi = 0$.

*Claim:* a closed 1-form $\eta$ on $S^1$ with $\oint_{S^1}\eta = 0$ is exact.
Lift $\eta$ to the universal cover $\mathbb{R} \xrightarrow{\pi} S^1$
($\pi(\theta) = e^{i\theta}$, identifying $S^1\subset\mathbb{C}$): $\pi^*\eta$
is a closed 1-form on $\mathbb{R}$, hence exact by Corollary 3.2
($\mathbb{R}$ is star-shaped), say $\pi^*\eta = dF$, $F\in C^\infty(\mathbb{R})$.
Since $\oint_{S^1}\eta=0$, i.e. $\int_0^{2\pi}\pi^*\eta = 0$, we get $F(2\pi)
= F(0) + \int_0^{2\pi}dF = F(0)$, so $F$ is $2\pi$-periodic and descends to a
well-defined smooth function $f : S^1\to\mathbb{R}$ with $\pi^*(df) = d(f
\circ \pi) = dF = \pi^*\eta$; since $\pi^*$ is injective on forms ($\pi$ is a
local diffeomorphism), $df = \eta$. So $\eta$ is exact, proving the claim,
and hence $\omega = c\,d\theta + d f$ for this $f$.

*Uniqueness of $c$.* If $\omega = c\,d\theta+d f = c'\,d\theta + df'$ then
$(c-c')d\theta = d(f'-f)$ is exact; but $\oint_{S^1}d\theta = 2\pi \ne 0$
while $\oint_{S^1}$ of any exact form is $0$ (Corollary 4.4-style reasoning:
$\oint_{S^1}dg = \int_0^{2\pi}\frac{d}{d\theta}(g\circ\pi)\,d\theta =
(g\circ\pi)(2\pi)-(g\circ\pi)(0) = 0$ since $g\circ\pi$ is $2\pi$-periodic).
So $c=c'$.

*Conclusion.* Every $[\omega]\in H^1_{\mathrm{dR}}(S^1)$ has a unique
representative-determined constant $c = \frac{1}{2\pi}\oint_{S^1}\omega$,
with $\omega - c\,d\theta$ exact — i.e. $[\omega] = c\,[d\theta]$, so
$[d\theta]$ is a basis of $H^1_{\mathrm{dR}}(S^1)$ as a real vector space, and
$H^1_{\mathrm{dR}}(S^1) \cong \mathbb{R}\cdot[d\theta] \cong \mathbb{R}$, via
$[\omega]\mapsto\oint_{S^1}\omega$ (which sends $c\,[d\theta] \mapsto
2\pi c$, a linear isomorphism $\mathbb{R}\to\mathbb{R}$, hence
$H^1_{\mathrm{dR}}(S^1)\cong\mathbb{R}$ as claimed — the normalization
constant $2\pi$ is immaterial to the isomorphism type). $\blacksquare$

*(Standard proof via lifting to the universal cover; cf. Tu, Introduction to
Manifolds, §17, Example computing $H^1_{\mathrm{dR}}(S^1)$; Bott–Tu, Ch. 1,
§4–5, for the Mayer–Vietoris route to the same computation as an alternative
proof strategy, not reproduced here.)*

> **What this buys you.** $H^1_{\mathrm{dR}}(S^1)\ne 0$ makes precise the
> informal claim in `00` §6.1 and §22 that "the circle has a 1-dimensional
> hole": the hole is not a metaphor, it is a nonzero cohomology class,
> exhibited by an explicit representative $d\theta$ (equivalently, by
> Example 2.1's $\omega$ restricted to $S^1$, which one checks is exactly
> $d\theta$). Contrast Corollary 3.2: $\mathbb{R}^n$, having no such hole,
> has $H^1_{\mathrm{dR}}(\mathbb{R}^n)=0$.

---

## 5. Consequence for Conservative Vector Fields

Combining `00` §6 with this document: a vector field $F$ on a domain
$U\subseteq\mathbb{R}^n$ corresponds to a closed 1-form $\omega_F$ iff
$\nabla\times F = 0$ (in $\mathbb{R}^3$; more generally $d\omega_F=0$).

- If $U$ is star-shaped (or more generally simply connected — a fact that
  generalizes Theorem 3.1 beyond star-shaped domains, not reproved here),
  $H^1_{\mathrm{dR}}(U)=0$, so closed $\implies$ exact: **every** curl-free
  field on $U$ is conservative.
- If $U$ has a "hole" (e.g. $U=\mathbb{R}^2\setminus\{0\}$), $H^1_{\mathrm{dR}}(U)
  \ne 0$ (Example 2.1), so curl-free does **not** imply conservative — this
  is precisely why the field $F=(-y,x)/(x^2+y^2)$, despite $\nabla\times F=0$
  away from the origin, has nonzero circulation around the origin.

This is the precise version of the informal claim, made already in `00` §6.1,
that "this is where topology enters the calculus."

---

## References Used

- **Tu, Introduction to Manifolds** — §5 (Poincaré lemma via explicit
  homotopy operator, low-degree cases worked by hand — cross-checked against
  the general-degree operator $K$ constructed in Theorem 3.1 above), §17
  (computation of $H^1_{\mathrm{dR}}(S^1)$ via the universal cover, the same
  strategy used in Theorem 4.1).
- **Lee, ISM** — Ch. 17–18 (de Rham cohomology, Thm 17.14 Poincaré lemma for
  star-shaped/contractible domains).
- **Bott–Tu** — Ch. 1, §4–5 (Mayer–Vietoris computation of
  $H^\bullet_{\mathrm{dR}}(S^1)$, an alternative route to Theorem 4.1 not
  reproduced here but useful as a cross-check).

Full bibliographic data: `notes/NOTATION.md` §4.

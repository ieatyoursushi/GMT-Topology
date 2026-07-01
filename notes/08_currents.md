# 08 — Currents

**Cross-references.** `notes/NOTATION.md` (typing conventions, §2.4 for
$D_k(\mathbb{R}^n)$); `notes/03_differential_forms_and_stokes.md` (defines
$\Omega_c^k(\mathbb{R}^n)$, $d$, and proves generalized Stokes — the theorem
this chapter reinterprets as definitional); `notes/06_measure_theory_bridge.md`
(Radon measures, weak-$*$ convergence background); `notes/07_rectifiability_and_hausdorff_measure.md`
(rectifiable sets, from which integral currents are built); `00_differential_geometry_foundations_for_GMT_Topology.md`
§28 (survey-level currents); feeds `notes/09_varifolds_and_finite_perimeter.md`
(contrasts oriented currents against unoriented varifolds).

---

## 0. Standing Definitions

$$
\Omega_c^k(\mathbb{R}^n) \quad \text{compactly supported smooth $k$-forms on } \mathbb{R}^n \quad \text{(`notes/03' §0)}.
$$

$$
T : \Omega_c^k(\mathbb{R}^n) \to \mathbb{R} \quad \text{linear and continuous (w.r.t. the standard test-form topology)}, \qquad T \in D_k(\mathbb{R}^n).
$$

$$
M \subseteq \mathbb{R}^n \quad \text{a smooth, compact, oriented $k$-submanifold with boundary } \partial M.
$$

---

## 1. From Integration Functionals to Currents

Recall from `01` §9.5 the conceptual move: object $\leadsto$ linear
functional on test objects. This is the single idea underlying currents.

**Motivating example.** A smooth, compact, oriented $k$-submanifold $M
\subseteq\mathbb{R}^n$ defines

$$
T_M(\omega) := \int_M \omega, \qquad T_M : \Omega_c^k(\mathbb{R}^n)\to\mathbb{R}, \qquad \omega \in \Omega_c^k(\mathbb{R}^n).
$$

$T_M$ is linear in $\omega$ (linearity of the integral) and continuous in an
appropriate sense (bounded in terms of the sup-norms of $\omega$'s
coefficients and their derivatives, uniformly for $\omega$ supported in a
fixed compact set — the precise topology on $\Omega_c^k$ is the standard one
from distribution theory, taken as background here).

**Definition 1.1 ($k$-current).** A **$k$-current** on $\mathbb{R}^n$ is a
continuous linear functional $T:\Omega_c^k(\mathbb{R}^n)\to\mathbb{R}$. Write
$D_k(\mathbb{R}^n)$ for the space of $k$-currents.

> **Type-check.** $T_M$ is *primarily* the functional $\omega\mapsto\int_M
> \omega$, not the point-set $M$. Two different-looking constructions that
> integrate against test forms identically define the *same* current. This
> mirrors exactly the distribution-vs-function move in `01` §9.5: a
> distribution is defined by how it pairs with test functions, not by a
> pointwise formula; a current is defined by how it pairs with test forms,
> not by a point-set. The type of a current is
> "$\Omega_c^k(\mathbb{R}^n)\to\mathbb{R}$, linear + continuous" — nothing in
> that type mentions a point set at all.

**Definition 1.2 (mass).** The **mass** of $T\in D_k(\mathbb{R}^n)$ is

$$
\mathbf{M}(T) := \sup\{T(\omega) : \omega\in\Omega_c^k(\mathbb{R}^n),\ \lVert\omega\rVert_\infty \le 1\} \ \in [0,\infty].
$$

For $T_M$, $\mathbf{M}(T_M) = \mathcal{H}^k(M)$ (the $k$-dimensional volume of
$M$) — mass generalizes "area/volume of the surface" to a current.

**Definition 1.3 (integral current, informally).** $T\in D_k(\mathbb{R}^n)$
is an **integer-multiplicity rectifiable current** if it has the form

$$
T(\omega) = \int_E \langle \omega(x), \vec{\tau}(x)\rangle\, \theta(x)\, d\mathcal{H}^k(x),
$$

where $E\subseteq\mathbb{R}^n$ is $\mathcal{H}^k$-rectifiable (`notes/07`
§2), $\vec\tau(x)$ is a choice of orientation of the approximate tangent
plane $T_xE$ (existing $\mathcal{H}^k$-a.e. by `notes/07` Proposition 2.3),
and $\theta:E\to\mathbb{Z}_{>0}$ is an integer multiplicity function,
$\mathcal{H}^k$-integrable. It is an **integral current** if additionally its
boundary (Definition 2.1 below) is also integer-multiplicity rectifiable. The
smooth-manifold current $T_M$ is the special case $E=M$, $\theta\equiv 1$,
$\vec\tau$ the given orientation.

---

## 2. Boundary by Duality

**Definition 2.1 (boundary of a current).** For $T\in D_k(\mathbb{R}^n)$,
$k\ge 1$, define $\partial T \in D_{k-1}(\mathbb{R}^n)$ by

$$
\partial T(\eta) := T(d\eta), \qquad \eta \in \Omega_c^{k-1}(\mathbb{R}^n).
$$

For $k=0$, set $\partial T := 0$.

> **Type-check.** $d : \Omega_c^{k-1}(\mathbb{R}^n) \to \Omega_c^k(\mathbb{R}^n)$
> has exactly the right type to be pre-composed with $T:\Omega_c^k
> (\mathbb{R}^n)\to\mathbb{R}$, giving $T\circ d : \Omega_c^{k-1}(\mathbb{R}^n)
> \to\mathbb{R}$ — an element of $D_{k-1}(\mathbb{R}^n)$, exactly the type
> required of $\partial T$. This is the *only* type-correct way to define a
> boundary operator on currents purely from $d$ and duality, without
> reference to any point-set structure — and it is the general pattern
> "adjoint of $d$" that also defines the distributional/weak derivative in
> `01` §9.6.

### 2.1 Anchor Proof 1: $\partial\partial = 0$

**Theorem 2.2.** For every $T\in D_k(\mathbb{R}^n)$, $\partial(\partial T) = 0$.

*Proof.* For $\zeta\in\Omega_c^{k-2}(\mathbb{R}^n)$,

$$
(\partial(\partial T))(\zeta) = (\partial T)(d\zeta) = T(d(d\zeta)) = T(0) = 0,
$$

using Definition 2.1 twice and then `notes/03` Theorem 2.4 ($d^2=0$) applied
to $\zeta$, and linearity of $T$ ($T(0)=0$). Since $\zeta$ was arbitrary,
$\partial(\partial T) = 0$ as a functional. $\blacksquare$

> **Why this is not a coincidence.** $\partial^2=0$ for currents is the exact
> dual statement of $d^2=0$ for forms (`notes/03` Theorem 2.4): both say "the
> boundary of a boundary/the derivative of a derivative vanishes," and the
> proof of one is transported to the other purely by the duality
> $\partial T(\omega) := T(d\omega)$. This is the precise sense in which `01`
> §12.1's slogan "boundaries have no boundary" and "$d^2=0$" are two shadows
> of a single algebraic fact.

### 2.2 Anchor Proof 2: Stokes Becomes (Almost) Definitional

**Theorem 2.3.** Let $M\subseteq\mathbb{R}^n$ be a smooth, compact, oriented
$k$-manifold with boundary $\partial M$ (induced orientation, `notes/03`
Definition 3.2), and let $T_M, T_{\partial M}$ be the associated currents.
Then

$$
\partial T_M = T_{\partial M}.
$$

*Proof.* For $\eta \in \Omega_c^{k-1}(\mathbb{R}^n)$,

$$
(\partial T_M)(\eta) = T_M(d\eta) = \int_M d\eta \ \overset{(\ast)}{=}\ \int_{\partial M}\eta = T_{\partial M}(\eta),
$$

where $(\ast)$ is exactly the generalized Stokes theorem, `notes/03` Theorem
4.2, applied to $\omega=\eta$ restricted to $M$. Since $\eta$ was arbitrary,
$\partial T_M = T_{\partial M}$ as functionals. $\blacksquare$

> **"Almost definitional," precisely.** Theorem 2.3 shows that once
> $\partial T := T(d\,\cdot\,)$ is *defined* by duality (Definition 2.1), the
> classical Stokes theorem for smooth manifolds is recovered as a single
> application of the already-proved generalized Stokes theorem (`notes/03`
> Thm 4.2) — no new analysis is required. What *is* new, and is the actual
> payoff of the current-theoretic framework, is that $\partial T$ makes
> sense and satisfies $\partial\partial=0$ (Theorem 2.2) for **every**
> current $T$, including ones with no underlying smooth manifold at all
> (e.g. integral currents built from merely rectifiable sets, Definition
> 1.3, or currents that are weak limits of a sequence of smooth manifold
> currents whose limit is not itself a manifold). The generalized Stokes
> theorem is a genuine theorem about smooth $M$; the current-boundary
> identity $\partial T(\omega)=T(d\omega)$ is instead a *definition* that
> happens to reduce to Stokes exactly when $T=T_M$ for smooth $M$ — this is
> the sense in which `00` §16.2 and §28 call the passage to currents "nearly
> definitional."

---

## 3. Why Currents Are the Right GMT Object

Currents retain, simultaneously and without requiring $M$ to be a smooth
manifold:

- **orientation** (built into $\vec\tau$ in Definition 1.3, or into the sign
  convention of $T(\omega)=\int_M\omega$ for smooth $M$),
- **multiplicity** ($\theta$ in Definition 1.3 — e.g. a current can represent
  "the same surface, counted twice"),
- **boundary** (Definition 2.1, satisfying $\partial^2=0$ by Theorem 2.2
  regardless of smoothness),
- **integration against test forms** (the defining datum, Definition 1.1).

This is exactly the list given informally in `00` §28, now each item traced
to the specific definition or theorem that supplies it. The compactness
theorem for integral currents (Federer–Fleming; stated, not proved, per the
"safe to black-box" list in `01` §17) is what makes currents useful for
existence proofs in the calculus of variations: a mass-bounded,
boundary-mass-bounded sequence of integral currents has a weakly convergent
subsequence, and the limit is again an integral current — the
current-theoretic analogue of the compactness arguments flagged as central
in `01` §6.2.

---

## References Used

- **Federer, GMT** — Ch. 4 (currents: definitions, mass, boundary by
  duality, the compactness theorem for integral currents mentioned in §3
  above but not proved).
- **Simon, LGMT** — §26–27 (an accessible from-scratch treatment of currents
  and their boundary, cross-checked against Federer's definitions above;
  the $\partial T_M=T_{\partial M}$ computation in Theorem 2.3 follows this
  source's presentation of the same fact).

Full bibliographic data: `notes/NOTATION.md` §4.

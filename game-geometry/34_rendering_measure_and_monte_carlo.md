# 34 — Rendering: Solid Angle, the Area Formula, and Monte Carlo Light

**Cross-references.** `game-geometry/NOTATION_GAME.md` (typing §1.5, `ZT`
convention §4); `notes/06` §5 (Hausdorff measure — solid angle *is*
$\mathcal{H}^2$ on the sphere), `notes/07` §3 (the area formula — §2's
anchor is a direct application of its Theorem 3.1/3.2); `01` §13.3
(empirical measures) and `stochastic/20` §2 ($L^2$ estimators) — §3's
Monte Carlo; `ml-geometry/14` §3 (mesh currents — §4's LOD remark);
`game-geometry/31` (the camera map whose fibers §1 integrates over);
`game-geometry/32` (raycasts as visibility).

**Register.** The rendering equation is settled (Kajiya 1986; the
measure-theoretic formulation is textbook — PBR 4e). The GPU-pipeline
material in §4 is versioned engineering, and the one research-flavored
item (GPU-driven LOD, Nanite) is flagged current.

**The Unity mechanics this document owns.** Light attenuation and spot
cones, shadow rays, sampling/spawning randomness — and the reason all of
it is Lebesgue integration wearing a shader.

---

## 0. Standing Definitions

Everything from `NOTATION_GAME.md` §1.5 ($\sigma = \mathcal{H}^2\llcorner
S^2$, hemisphere $H^2(n)$, radiance $L$, BRDF $f_r$, projection $\pi_x$,
MC estimator $\hat I_N$). Additionally, for a surface patch
$S\subseteq\mathbb{R}^3$ (a compact $C^1$ hypersurface piece, `notes/07`)
and a viewpoint $x\notin S$:

$$
r(p) := |p-x|, \qquad u(p) := \pi_x(p) = \frac{p-x}{r(p)}, \qquad \cos\theta(p) := |\langle n(p),\,u(p)\rangle| \quad (n(p) \text{ the unit normal of } S \text{ at } p).
$$

**Standing regularity hypothesis:** patches are $C^1$, viewpoints are off
the surfaces involved, and visibility functions are measurable — enough
for `notes/06` Theorem 4.3-style exchanges without re-flagging.

---

## 1. The Rendering Equation Is a Lebesgue Integral

**The equation (Kajiya 1986), typed.** At a surface point $p$ with
normal $n$, outgoing direction $\omega_o$:

$$
L_o(p,\omega_o) \ =\ L_e(p,\omega_o)\ +\ \int_{H^2(n)} f_r(p,\omega_i,\omega_o)\,L_i(p,\omega_i)\,\langle\omega_i,n\rangle\ d\sigma(\omega_i),
$$

an integral over the hemisphere **with respect to solid-angle measure**
$\sigma = \mathcal{H}^2\llcorner S^2$ — the Hausdorff measure this
repository constructed in `notes/06` §5, restricted to the unit sphere.
Nothing Riemannian is invoked: the domain is a fixed sphere, the measure
is Hausdorff, the integrand is a measurable radiance field, and every
question about it (convergence of estimators, changes of variables,
interchange of limits) is `notes/06` machinery. Real-time pipelines
evaluate crude quadratures of this integral (a sum over a few analytic
lights); path tracers evaluate it by Monte Carlo (§3); both are
approximating the same Lebesgue integral (PBR 4e, Ch. 4–5, 13).

> **Type-check — the degree-matching rule, measure-theoretically.**
> Radiance $L$ has units of power per area per solid angle, i.e. it is a
> *density with respect to the product of two measures* the repo has
> already typed: $\mathcal{H}^2$ on surfaces and $\sigma$ on directions.
> The $\langle\omega_i,n\rangle$ factor is not decoration — it is
> exactly the Jacobian relating the two, as §2 now proves.

---

## 2. Anchor Proof: the $\cos\theta/r^2$ Geometry Factor Is the Area Formula

Light transport constantly rewrites direction-integrals as
surface-integrals ("integrate over the light source's area instead of
over directions"). The exchange rate is:

**Theorem 2.1.** Let $S$ be a patch visible injectively from $x$ (each
direction meets it at most once). Then for every nonnegative measurable
$g$ on $S^2$,

$$
\int_{\pi_x(S)} g\ d\sigma \ =\ \int_S g(u(p))\ \frac{\cos\theta(p)}{r(p)^2}\ d\mathcal{H}^2(p);
$$

in particular the solid angle subtended by $S$ is $\sigma(\pi_x(S)) =
\int_S \cos\theta/r^2\,d\mathcal{H}^2$.

*Proof.* The map $\pi_x: S\to S^2$ is $C^1$ (quotient of $C^1$
functions, $r>0$), injective by hypothesis, so the Lipschitz-map area
formula (`notes/07` Theorem 3.2, injective case, applied to a Lipschitz
extension of $\pi_x$ off the compact patch) reduces the claim to
computing the **2-Jacobian of the tangent map** $D\pi_x|_{T_pS}$ and
showing $J_2 = \cos\theta/r^2$.

*Step 1: the ambient differential.* Differentiating $u = (p-x)/r$ with
$\nabla r = u$:

$$
Du_p \ =\ \frac{1}{r}\big(\mathrm{Id} - u\,u^\top\big) \ =\ \frac{1}{r}\,P_{u^\perp},
$$

the orthogonal projection onto $u^\perp$, scaled by $1/r$ — a direct
quotient-rule computation.

*Step 2: singular values on $T_pS$.* Write the viewing direction against
the surface frame: $u = \cos\theta\,n + \sin\theta\,\tilde u$ with
$\tilde u\in T_pS$ unit (if $\sin\theta=0$ the map is conformal on
$T_pS$ with factor $1/r$ and $\cos\theta=1$; the generic case follows).
Choose the orthonormal basis of $T_pS$: $e_1\perp\tilde u$ (so
$e_1\perp u$ and $e_1\perp n$), $e_2 = \tilde u$. Then:

$$
P_{u^\perp}e_1 = e_1 \quad (\text{already }\perp u), \qquad
P_{u^\perp}e_2 = \tilde u - \sin\theta\,u = \cos\theta\,\big(\cos\theta\,\tilde u - \sin\theta\,n\big),
$$

(substitute $u$ and simplify), where $\cos\theta\,\tilde u -
\sin\theta\,n$ is a *unit* vector orthogonal to $e_1$. So in orthonormal
bases the tangent map $\tfrac1r P_{u^\perp}|_{T_pS}$ has singular values
$\tfrac1r$ and $\tfrac{\cos\theta}{r}$, and by the linear-map area
formula — `notes/07` **Theorem 3.1**, the anchor proved there from the
SVD — its 2-Jacobian is the product:

$$
J_2\big(D\pi_x|_{T_pS}\big) \ =\ \frac1r\cdot\frac{\cos\theta}{r} \ =\ \frac{\cos\theta}{r^2}.
$$

*Step 3: conclude.* The area formula turns $\int_{\pi_x(S)}g\,d\mathcal{H}^2
= \int_S (g\circ u)\,J_2\,d\mathcal{H}^2$, and $\sigma =
\mathcal{H}^2\llcorner S^2$. $\blacksquare$

> **What this buys you.** Every "light falls off as $1/r^2$" in a shader,
> every spotlight cone term, every conversion between sampling a light's
> *area* and sampling *directions* in a path tracer — with the sampling
> densities related by exactly $\cos\theta/r^2$ (PBR 4e, Ch. 4, 12) — is
> Theorem 2.1. That the workhorse identity of rendering is literally the
> repository's `notes/07` anchor, applied to one explicit linear map, is
> the cleanest single datum for the phase's wager: **illumination is
> measure theory.** (The smooth-Riemannian reader may note $\sigma$ is
> also the round metric's volume form — true, but nothing in the proof
> used it; the Hausdorff formulation is the one that survives when $S$
> is merely rectifiable, per the `notes/07` worldview.)

---

## 3. Monte Carlo Rendering: Estimators on the Repo's Probability Stack

Path tracers evaluate §1's integral by sampling. With target $I = \int
f\,d\mu$ ($\mu$ a probability measure after normalization) and iid
samples $X_i\sim p$ with density $p>0$ on $\{f\ne0\}$:

**Proposition 3.1.** The importance-sampled estimator $\hat I_N =
\frac1N\sum_i f(X_i)/p(X_i)$ satisfies:

1. **(Unbiased.)** $\mathbb{E}[\hat I_N] = \int \frac{f}{p}\,p\,d\mathcal{L} = I$.
2. **(Variance.)** $\operatorname{Var}(\hat I_N) = \frac1N\Big(\int
   \frac{f^2}{p}\,d\mathcal{L} - I^2\Big)$ — the $L^2$ geometry of
   `stochastic/20` §2 on the sample space: independence kills the cross
   terms exactly as in the Itô-isometry computation.
3. **(Optimal proposal.)** Among densities, $\operatorname{Var}$ is
   minimized by $p^*\propto|f|$ (for $f\ge0$: variance zero — each
   sample returns $I$ exactly). *Proof of 3:* by Cauchy–Schwarz in
   $L^2(p)$ (`01` §9), $\big(\int|f|\big)^2 = \big(\int
   \tfrac{|f|}{p}\,p\big)^2 \le \int\tfrac{f^2}{p}\cdot\int p =
   \int\tfrac{f^2}{p}$, with equality iff $|f|/p$ is a.e. constant.
   $\blacksquare$

*(All three are elementary and proved in full — depth C by triviality;
their consequences are the practitioner playbook.)* **Cosine-weighted
hemisphere sampling** is item 3 aimed at §1's integrand: sampling
$\omega_i$ with density $\propto\langle\omega_i,n\rangle$ cancels the
Jacobian factor of Theorem 2.1's type-check; **next-event estimation**
(sampling the light's area, converting by Theorem 2.1) is a change of
proposal measure through the area formula; **multiple importance
sampling** blends proposals with weights keeping item 1 intact (PBR 4e,
Ch. 13–14, at recognition depth). The samples themselves are `01`
§13.3's empirical measure $\hat\mu_N$ pushed through the integrand — the
same object that opened the repo's probability line.

---

## 4. Meshes, LOD, and the Return of Currents

The scene geometry these integrals run over is triangulated — and
`ml-geometry/14` Theorem 3.1 is exactly the statement that mesh
refinement converges *as currents*: $T_h(\omega)\to T(\omega)$, with
mesh surface *area* converging to the smooth $\mathcal{H}^2$-measure.
Level-of-detail systems traverse the same limit in reverse — simplify
the mesh while controlling the error a viewer (a family of §2 projection
maps!) can see; GPU-driven pipelines like Nanite (Karis 2021, flagged
current) make the cut view-dependent, choosing per-cluster resolution by
projected — i.e. Theorem 2.1-weighted — error. The `14`-style question
"in which topology does the simplified mesh approximate the original"
is, on this reading, a production scheduling problem. (Recognition
depth; the research literature on perceptual/geometric LOD metrics is
active.)

---

## 5. The `ZT` Weave: Flashlights, Falloff, and Spawn Randomness

- **The flashlight is a solid-angle wedge.** `(ZT,
  Assets/Scripts/flashlight.cs, pinned 2026-07-19 @ ce956b0)` toggles a
  Unity spot light: a cone $C_\alpha = \{\omega : \langle\omega,
  f\rangle\ge\cos\alpha\}$ of directions — a $\sigma$-measurable cap
  with $\sigma(C_\alpha) = 2\pi(1-\cos\alpha)$ (integrate
  `notes/06`-style in spherical coordinates) — carrying an intensity
  density attenuated by exactly Theorem 2.1's $1/r^2$ (engines
  additionally clamp with a smooth window for range; Unity Docs, Light).
  Reading a game flashlight as "a finite measure on $S^2$, pushed to
  surfaces by the area formula" is this document in one object.
- **Shadow = visibility = `32`.** Whether the cone's light reaches $p$
  is a raycast ($\tau^*$ against occluders) — the visibility factor
  multiplying §1's integrand is `32` §2's query, and shadow maps are its
  precomputation on the projection charts of `31`.
- **Spawning is sampling.** `(ZT, Assets/Scripts/SpawnClown.cs and
  Managers, pinned 2026-07-19 @ ce956b0)` places enemies by drawing from
  finitely many spawn points/ranges — sampling a discrete (mixture)
  measure, `01` §13.3's $\hat\mu$ from the generative side. The same
  uniform draws that integrate light in §3 populate the level here; a
  designer tuning spawn weights is editing a probability measure, and
  "fair-feeling randomness" complaints are variance statements
  (Proposition 3.1(2)) about small $N$.

```text
lights (emitters: measures on S²) ── flashlight cone C_α, σ(C_α) = 2π(1−cos α)
      │
      │  Theorem 2.1:  dσ  =  (cos θ / r²) dℋ²      ← notes/07's area formula, verbatim
      ▼
surfaces (ℋ²-measure, meshes ≈ currents — §4) ──visibility: 32's raycast──► shading point p
      │
      │  §1: L_o = L_e + ∫_{H²(n)} f_r L_i ⟨ω,n⟩ dσ      (Lebesgue integral, notes/06 machinery)
      ▼
camera (31's projection charts) ──§3: Monte Carlo on 01 §13.3's empirical measure──► pixels
```

---

## References Used

- **Kajiya 1986** — the rendering equation (§1).
- **PBR 4e** — Ch. 4–5 (radiometry, the measure-theoretic formulation),
  Ch. 12–14 (light sampling, MC, MIS) — the practitioner companion to
  §§1–3.
- **RTR 4e** — real-time approximations of the same integral
  (recognition, §1).
- **Karis 2021** — Nanite, §4's flagged-current LOD remark.
- **Unity Docs** — Light (spot cones, attenuation), QualitySettings/LOD
  (accessed 2026-07-19).
- **ZT** (pinned 2026-07-19 @ `ce956b0`) —
  `Assets/Scripts/flashlight.cs`; `Assets/Scripts/SpawnClown.cs` and
  spawn managers.
- **`notes/06`** §5 ($\mathcal{H}^2$, spherical integration);
  **`notes/07`** Theorems 3.1–3.2 (the anchor's engine); **`01`** §9,
  §13.3 (Cauchy–Schwarz; empirical measures); **`stochastic/20`** §2
  (the $L^2$ estimator geometry); **`ml-geometry/14`** §3 (mesh
  currents).

Full bibliographic data: `game-geometry/NOTATION_GAME.md` §3–§4.

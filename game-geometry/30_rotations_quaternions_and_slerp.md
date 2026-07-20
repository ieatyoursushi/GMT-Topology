# 30 — Rotations: Quaternions, Slerp, and the Gimbal-Lock Chart

**Cross-references.** `game-geometry/NOTATION_GAME.md` (typing §1.1,
citation key, `ZT` convention §4 — read first); `notes/02` (charts and
their singularities — §4 is a worked engine instance), `notes/04` §3
(geodesics, $\exp_p$ — §3 realizes the "sphere geodesics are great
circles" row of its table at depth C); `00` §9 and `01` §10 (flows/ODEs —
§5's smoothing analysis); `stochastic/22` §2 (the same integrating-factor
ODE, there for OU — compare §5). Feeds `31` (rotations sit inside
transforms) and `32` (rigid-body attitude).

**Register.** Settled classical (Hamilton 1843; Shoemake 1985 for the
graphics transplant). Engine citations per `NOTATION_GAME.md` §5.

**The Unity mechanics this document owns.** `Quaternion.LookRotation`,
`Quaternion.Slerp`/`Lerp`, `Transform.localEulerAngles`, mouse-look pitch
clamping, and exponential `Lerp` smoothing — each tied to its `ZT` use.

---

## 0. Standing Definitions

Everything from `NOTATION_GAME.md` §1.1 ($\mathbb{H}$, $S^3$, $R_q$,
$SO(3)$, slerp). Additionally: for pure imaginary $v,w\in\Im(\mathbb{H})
\cong\mathbb{R}^3$, the quaternion product decomposes as

$$
v\,w = -\langle v,w\rangle + v\times w \ \in \mathbb{R}\oplus\Im(\mathbb{H})
$$

(direct computation from $\mathbf{i}\mathbf{j}=\mathbf{k}$ etc. — the dot
and cross product are the real and imaginary shadows of one product), and
norm multiplicativity holds:

$$
|pq|^2 = pq\,\overline{pq} = p\,q\bar q\,\bar p = |q|^2\,p\bar p = |p|^2|q|^2,
$$

using $\overline{pq} = \bar q\bar p$ and that $q\bar q=|q|^2$ is real
(hence central).

---

## 1. Anchor Proof: Unit Quaternions Rotate, Two-to-One

**Theorem 1.1.** For $q\in S^3$, the map $R_q(v) = qv\bar q$ is a
rotation: $R_q\in SO(3)$. The assignment $q\mapsto R_q$ is a group
homomorphism $S^3\to SO(3)$ that is **surjective** with **kernel
$\{\pm1\}$** — a 2:1 covering. Explicitly, writing

$$
q = \cos\tfrac\theta2 + \sin\tfrac\theta2\,u, \qquad u\in\Im(\mathbb{H}),\ |u|=1,\ \theta\in\mathbb{R},
$$

$R_q$ is the rotation about the axis $u$ by the angle $\theta$.

*Proof.* **(i) $R_q$ preserves $\Im(\mathbb{H})$ and is an isometry.**
For $v$ pure imaginary, $\overline{qv\bar q} = q\bar v\bar q = -qv\bar q$,
so $R_q(v)$ is pure imaginary. Linearity is clear;
$|R_q(v)| = |q||v||\bar q| = |v|$ by norm multiplicativity (§0) and
$|q|=1$. So $R_q\in O(3)$.

**(ii) $\det R_q = +1$.** $q\mapsto R_q$ is continuous, $S^3$ is
connected (`01` §7), $\det\circ R$ is a continuous map $S^3\to\{\pm1\}$,
and $R_1 = \mathrm{id}$ has determinant $+1$ — so $\det R_q = +1$ for
every $q$. (This is `01` §7.2's "connectedness rules out jumps" used as a
working tool.)

**(iii) Homomorphism.** $R_{pq}(v) = pq\,v\,\overline{pq} =
p(qv\bar q)\bar p = R_p(R_q(v))$.

**(iv) Axis–angle form.** Split $v = v_\parallel + v_\perp$ along/against
$u$. For $v_\parallel = \lambda u$: $q$ is a real combination of $1$ and
$u$, so $q$ commutes with $u$, giving $R_q(v_\parallel) = v_\parallel
q\bar q = v_\parallel$ — the axis is fixed. For $v_\perp\perp u$, abbreviate
$c = \cos\tfrac\theta2$, $s=\sin\tfrac\theta2$:

$$
R_q(v_\perp) = (c+su)\,v_\perp\,(c-su) = c^2 v_\perp + cs\,(u v_\perp - v_\perp u) - s^2\,u v_\perp u.
$$

By §0's product rule and $u\perp v_\perp$: $uv_\perp = u\times v_\perp$
and $v_\perp u = -\,u\times v_\perp$, so $uv_\perp - v_\perp u =
2\,u\times v_\perp$; and

$$
u\,v_\perp\,u = (u\times v_\perp)\,u = -\langle u\times v_\perp, u\rangle + (u\times v_\perp)\times u = v_\perp,
$$

since $u\times v_\perp\perp u$ and $(u\times v_\perp)\times u = v_\perp$
for unit $u\perp v_\perp$. Hence

$$
R_q(v_\perp) = (c^2-s^2)\,v_\perp + 2cs\ u\times v_\perp = \cos\theta\ v_\perp + \sin\theta\ u\times v_\perp
$$

— exactly the rotation of $v_\perp$ by $\theta$ in the plane $u^\perp$
(Rodrigues' formula). Combining with the fixed axis proves the axis–angle
claim, and **surjectivity** follows since every element of $SO(3)$ is an
axis–angle rotation (Euler's rotation theorem: a special orthogonal map
of $\mathbb{R}^3$ has $+1$ as an eigenvalue — its eigenvector is the
axis; standard, cited at operational depth, e.g. Hall, Lie, Ch. 1).

**(v) Kernel.** $R_q=\mathrm{id}$ means $qv = vq$ for all pure imaginary
$v$, hence for all of $\mathbb{H}$. Write $q = a+b\mathbf{i}+c\mathbf{j}
+d\mathbf{k}$ and impose $q\mathbf{i}=\mathbf{i}q$: using
$\mathbf{ji}=-\mathbf{k}$, $\mathbf{ki}=\mathbf{j}$,

$$
q\mathbf{i} = -b + a\mathbf{i} + d\mathbf{j} - c\mathbf{k}, \qquad \mathbf{i}q = -b + a\mathbf{i} - d\mathbf{j} + c\mathbf{k},
$$

forcing $c=d=0$; imposing $q\mathbf{j}=\mathbf{j}q$ similarly forces
$b=0$. So $q$ is real and unit: $q=\pm1$; conversely both act trivially.
$\blacksquare$

> **Type-check — the half-angle is the double cover.** The formula rotates
> by $\theta$ while $q$ carries $\theta/2$: as $\theta$ runs $0\to2\pi$,
> the rotation returns to $\mathrm{id}$ but $q$ arrives at $-1$. That is
> the kernel $\{\pm1\}$ made kinematic — $q$ and $-q$ encode the same
> rotation, which is why engine code takes $\operatorname{sign}$-corrected
> dot products before interpolating (§3), and why "the space of rotations"
> is $SO(3)\cong S^3/\{\pm1\} = \mathbb{RP}^3$ — a quotient manifold in
> exactly `notes/02` §1's sense (and `01` §7.4's "quotients are powerful
> but dangerous," here a benign free quotient).

**The engine hook.** `(ZT, Assets/Scripts/Player/PlayerMovement.cs,
Turning(), pinned 2026-07-19 @ ce956b0)` builds its aim rotation as
`Quaternion.LookRotation(playerToMouse)` after flattening
`playerToMouse.y = 0f`: LookRotation is precisely "the unique $R\in SO(3)$
with $R\,\hat z = $ given forward, $R\,\hat y$ closest to world-up" —
well-posed exactly because the flattening keeps forward off the up-axis
(otherwise the "closest up" clause degenerates: the same singularity §4
meets as gimbal lock). The result is applied through
`Rigidbody.MoveRotation`, i.e. handed to the physics integrator as an
element of $SO(3)$, not as three Euler numbers — the engine's own API is
quaternion-native for exactly the reasons of this section (Unity Docs,
Quaternion.LookRotation, Rigidbody.MoveRotation).

---

## 2. Interpolating Rotations: the Problem Slerp Solves

Given attitudes $q_0, q_1\in S^3$ (say, a turret's current and desired
facing), the naive blend $(1-t)q_0 + tq_1$ leaves $S^3$ — it is a chord
of the sphere, and after renormalization it traverses angle at a
*non-uniform rate* (fast in the middle, slow at the ends). The correct
notion of "straight line between rotations" is `notes/04` §3's:
**the geodesic**.

---

## 3. Anchor Proof: Slerp Is the Great-Circle Geodesic

**Theorem 3.1.** Let $q_0,q_1\in S^3$ with $\Omega =
\arccos\langle q_0,q_1\rangle\in(0,\pi)$. Then

$$
\operatorname{slerp}(q_0,q_1;t) = \cos(t\Omega)\,q_0 + \sin(t\Omega)\,w, \qquad w := \frac{q_1 - \langle q_0,q_1\rangle\,q_0}{\sin\Omega},
$$

i.e. slerp is the arc $\gamma(t) = \cos(t\Omega)q_0 + \sin(t\Omega)w$ of
the great circle through $q_0$ in the direction of the unit vector
$w\perp q_0$; it lies on $S^3$, runs from $q_0$ to $q_1$ at **constant
speed $\Omega$**, and is a geodesic of the round $S^3$ — in the language
of `notes/04` §3, $\operatorname{slerp}(q_0,q_1;t) =
\exp_{q_0}\!\big(t\,\Omega\,w\big)$.

*Proof.* **(i) The two formulas agree.** $w$ is the Gram–Schmidt
orthonormalization of $q_1$ against $q_0$: $\langle w,q_0\rangle = 0$ and
$|w|=1$ (numerator has squared norm $1 - \cos^2\Omega = \sin^2\Omega$).
Substituting $q_1 = \cos\Omega\,q_0 + \sin\Omega\,w$ into
`NOTATION_GAME.md` §1.1's slerp formula:

$$
\frac{\sin((1-t)\Omega)}{\sin\Omega}q_0 + \frac{\sin(t\Omega)}{\sin\Omega}\big(\cos\Omega\,q_0+\sin\Omega\,w\big)
= \frac{\sin((1{-}t)\Omega) + \sin(t\Omega)\cos\Omega}{\sin\Omega}\,q_0 + \sin(t\Omega)\,w,
$$

and $\sin((1-t)\Omega) = \sin\Omega\cos(t\Omega) -
\cos\Omega\sin(t\Omega)$ collapses the $q_0$-coefficient to
$\cos(t\Omega)$. **(ii) On the sphere, endpoints, constant speed.**
$|\gamma(t)|^2 = \cos^2(t\Omega)+\sin^2(t\Omega) = 1$ (orthonormal
$q_0,w$); $\gamma(0)=q_0$, $\gamma(1) = \cos\Omega\,q_0+\sin\Omega\,w =
q_1$; $\gamma'(t) = \Omega(-\sin(t\Omega)q_0 + \cos(t\Omega)w)$ has
$|\gamma'| \equiv \Omega$. **(iii) Geodesic.** $\gamma''(t) =
-\Omega^2\gamma(t)$ is a scalar multiple of the position vector — i.e.
*normal* to $S^3$ at $\gamma(t)$ (the tangent space of the unit sphere at
$x$ is $x^\perp$). For an embedded submanifold with the induced metric,
the Levi-Civita covariant derivative is the tangential projection of the
ambient derivative (the Gauss formula — Lee, IRM, Ch. 8, operational
depth; it is the embedded-submanifold face of `notes/04` Theorem 2.3's
uniqueness), so

$$
\nabla_{\dot\gamma}\dot\gamma = \big(\gamma''\big)^{\mathrm{tangential}} = \big(-\Omega^2\gamma\big)^{\mathrm{tangential}} = 0
$$

— `notes/04` Definition 3.1's geodesic equation holds. Since
$\gamma(t)=\exp_{q_0}(t\Omega w)$ is then immediate from `notes/04`
Definition 3.2 ($\gamma$ is *the* geodesic with initial velocity
$\Omega w$), the proof is complete. $\blacksquare$

> **What this buys you, at the controller.** (a) *Constant angular rate*:
> a turret slerping at fixed $t$-rate turns through equal angles in equal
> frames — the property lerp-then-normalize lacks. (b) *The sign flip*:
> because of Theorem 1.1's double cover, $q_1$ and $-q_1$ are the same
> attitude but lie antipodally on $S^3$; engines take
> $\langle q_0,q_1\rangle<0 \Rightarrow q_1\mapsto-q_1$ first, choosing
> the *shorter* of the two geodesics — otherwise the turret swings the
> long way around. (c) *Cost*: slerp needs a $\arccos$ and two $\sin$s;
> `Quaternion.Lerp` (normalize the chord) is cheaper and, for small
> $\Omega$, agrees to second order — which is why engines offer both
> (Shoemake 1985; Unity Docs, Quaternion.Slerp). This is
> `ml-geometry/13` §2's retraction-vs-exponential trade, verbatim:
> normalized lerp is a retraction on $S^3$ whose first-order agreement
> with $\exp$ is exactly the retraction axiom.

---

## 4. Euler Angles Are a Chart, and Gimbal Lock Is Its Singularity

Yaw–pitch–roll assigns to $(\alpha,\beta,\gamma)$ the rotation
$E(\alpha,\beta,\gamma) := R_{\hat y}(\alpha)\,R_{\hat x}(\beta)\,
R_{\hat z}(\gamma)$. On the open set $\beta\in(-\tfrac\pi2,\tfrac\pi2)$
(mod the other angles' periodicity), $E$ is a diffeomorphism onto its
image — a **chart** on the 3-manifold $SO(3)$ in exactly `notes/02` §2's
sense. At $\beta=\pm\tfrac\pi2$ the differential $dE$ **drops rank**: the
yaw and roll one-parameter subgroups become parallel (rotating about
world-up and about body-forward-now-pointing-up move the frame the same
way), so $dE$ has rank 2 — one degree of freedom vanishes. That is
**gimbal lock**: not a failure of rotations (Theorem 1.1's $S^3$ has no
singular points) but of a *coordinate chart*, precisely `notes/02`'s
lesson that charts are local and their boundaries are real.

**The engine hook.** `(ZT, Assets/Scripts/CameraMovement.cs, pinned
2026-07-19 @ ce956b0)` implements first-person look as

- yaw on the body: `transform.Rotate(Vector3.up * mouseDelta.x * mouseSensitivity)`,
- pitch on the camera: `camerPitch = Mathf.Clamp(camerPitch, -90f, 90f)`
  then `playerCamera.localEulerAngles = Vector3.right * camerPitch`.

Two chart-theoretic facts hide here. First, the yaw/pitch *split across
two transforms* composes rotations in a fixed order — it is the Euler
chart implemented as a scene-graph hierarchy (`31` §1). Second, the
clamp at exactly $\pm90°$ is the chart boundary: the code confines the
state to the closure of the chart's domain of validity, which is why FP
cameras never gimbal-lock in practice — *the genre's control scheme is a
chart-domain restriction*. (Flight and space games, which need
$\beta$ past vertical, are exactly the genres forced to store attitude
as a quaternion.)

---

## 5. Lerp Smoothing Is a Discretized Flow (and Its Famous Frame-Rate Bug)

`(ZT, Assets/Scripts/CamerFollow.cs, pinned 2026-07-19 @ ce956b0)`
follows the player with

```csharp
transform.position = Vector3.Lerp(transform.position, targetCamPos, smoothing * Time.deltaTime);
```

with `smoothing = 5f`. Typed: the update is $x_{k+1} = x_k +
\lambda\Delta t_k\,(a - x_k)$, i.e. **explicit Euler with step
$\Delta t_k$ on the linear ODE** $\dot x = \lambda(a-x)$ — the
flow-of-a-vector-field picture of `00` §9, whose exact solution is
exponential decay $x(t) = a + e^{-\lambda t}(x_0-a)$ (the same
integrating-factor computation as `stochastic/22` Proposition 2.1 with
the noise deleted).

> **Type-check — why this idiom is frame-rate dependent.** Explicit Euler
> with step $\Delta t$ contracts the error by $(1-\lambda\Delta t)$ per
> frame, so over one second at frame rate $1/\Delta t$ the total
> contraction is $(1-\lambda\Delta t)^{1/\Delta t}$ — which *depends on*
> $\Delta t$ and only approaches the true $e^{-\lambda}$ as
> $\Delta t\to0$. At 30 vs 144 FPS the camera stiffness genuinely
> differs; and for $\Delta t > 1/\lambda$ (a frame hitch with
> `smoothing = 5` means $\Delta t>0.2$s) the factor $1-\lambda\Delta t$
> goes negative — overshoot. The frame-rate-*independent* form is the
> exact flow, $x \mapsto a + e^{-\lambda\Delta t}(x-a)$, one `exp` per
> frame — the discrete-vs-continuous-flow distinction of `01` §10 paying
> a concrete engineering dividend. The same analysis applies verbatim to
> slerping a fraction $\lambda\Delta t$ toward a target attitude each
> frame, with the geodesic (§3) in place of the segment.

```text
mouse position ──ScreenPointToRay──► floor raycast (31 §4) ──► playerToMouse ∈ ℝ³, y := 0
        │                                                            │
        ▼                                                            ▼
FP look: Euler chart, clamped |pitch| ≤ 90° (§4)      LookRotation: forward ↦ R ∈ SO(3)  (§1)
        │                                                            │
        └────────────► attitude state q ∈ S³  ◄──────────────────────┘
                            │  (§3: slerp = exp_{q₀}(tΩw), shorter arc via sign flip)
                            ▼
              per-frame blending / smoothing  (§5: discretized flow, e^{−λΔt} for frame-rate freedom)
```

---

## References Used

- **Shoemake 1985** — slerp for graphics; §2–§3's motivation and the
  lerp-vs-slerp trade.
- **Hall, Lie** — Ch. 1 (Euler's rotation theorem, $SO(3)$ basics; the
  double cover $S^3\cong SU(2)\to SO(3)$ in representation-theoretic
  form, cross-checking Theorem 1.1).
- **Lee, IRM** — Ch. 5 (geodesics, $\exp$ — via `notes/04` §3), Ch. 8
  (the Gauss formula used in Theorem 3.1(iii), at operational depth).
- **Unity Docs** — Quaternion.Slerp, Quaternion.Lerp,
  Quaternion.LookRotation, Rigidbody.MoveRotation,
  Transform.localEulerAngles (accessed 2026-07-19).
- **ZT** (pinned 2026-07-19 @ `ce956b0`) —
  `Assets/Scripts/Player/PlayerMovement.cs` (Turning());
  `Assets/Scripts/CameraMovement.cs` (yaw/pitch split, pitch clamp);
  `Assets/Scripts/CamerFollow.cs` (lerp smoothing).

Full bibliographic data: `game-geometry/NOTATION_GAME.md` §3–§4.

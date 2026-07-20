# 32 — Collision: Distance Functions, Sphere Tracing, Convexity, Rigid Bodies

**Cross-references.** `game-geometry/NOTATION_GAME.md` (typing §1.3, `ZT`
convention §4); `ml-geometry/15` Lemma 4.1 (the distance function is
1-Lipschitz — the single load-bearing input of §2); `notes/07` §1
(Rademacher — the eikonal remark in §2); `01` §4 and §9 (the projection
theorems — §3's separating hyperplane); `01` §4.7 (spectral theorem —
§4's inertia tensor); `game-geometry/31` (colliders live in world space
via its transforms); feeds `33` (a navmesh is baked from collision
geometry) and `34` (raycasting returns as visibility).

**Register.** Settled classical mathematics; the *algorithms* are
1980s–90s classics (GJK 1988; Hart 1996) and the practitioner canon is
living (Quilez, SDF). Per `NOTATION_GAME.md` §5.

**The Unity mechanics this document owns.** `Physics.Raycast` against
implicit surfaces, `Physics.OverlapSphere`, convex `MeshCollider`s /
GJK-style narrow phase, and rigid-body inertia.

**A note on the guess.** This is the document where "more Riemannian than
Lebesgue" starts to fail: nothing in §2 assumes a manifold — the
correctness theorem for sphere tracing needs only a **closed set** and a
**1-Lipschitz function**, which is `notes/06`–`07` country, not
`notes/04` country.

---

## 0. Standing Definitions

Everything from `NOTATION_GAME.md` §1.3 ($d_S$, SDF, $h_A$, $A\ominus B$,
$B_r(c)$). Additionally:

$$
S\subseteq\mathbb{R}^n \ \text{nonempty and closed}, \qquad \rho(t) := o + t\hat v, \ t\ge0, \ |\hat v|=1 \quad \text{a ray}, \qquad \tau^* := \inf\{t\ge0 : \rho(t)\in S\} \quad (\inf\varnothing := \infty).
$$

$$
A, B \subseteq\mathbb{R}^n \ \text{convex}; \qquad \text{a body } \mathcal{B} \text{ with mass density } \varrho:\mathcal{B}\to[0,\infty), \ \text{total mass } m = \int_\mathcal{B}\varrho\,d\mathcal{L}^3.
$$

---

## 1. The Two Queries Every Frame Is Made Of

Engine collision reduces to two primitive questions, both purely
metric-geometric:

- **Ray query** (`Physics.Raycast`): first intersection of $\rho$ with
  scene geometry — compute $\tau^*$ and the hit point $\rho(\tau^*)$.
  `31` Corollary 4.2 solved the single-plane case in closed form;
  `(ZT, Assets/Scripts/Player/PlayerShooting.cs, pinned 2026-07-19 @
  ce956b0)` is the gameplay face: a hitscan shot *is* $\tau^*$ against
  the shootable mask.
- **Region query** (`Physics.OverlapSphere`): all colliders meeting the
  metric ball $B_r(c)$ — a sublevel set $\{d_{S}\le r\}$ enumeration.

§2 proves the general algorithm for the first when geometry is given by a
distance function; §3 gives the structure theory for the second when
geometry is convex; §5 reads `ZT`'s explosives through both.

---

## 2. Anchor Proof: Sphere Tracing Is Correct

Suppose the scene is presented by its distance function $d_S$ (or an SDF
$f$ with $|f|=d_S$, `NOTATION_GAME.md` §1.3) — the representation behind
raymarched/procedural geometry (Hart 1996; Quilez, SDF). **Sphere
tracing** marches the ray by exactly the current distance:

$$
t_0 := 0, \qquad t_{k+1} := t_k + d_S(\rho(t_k)).
$$

**Theorem 2.1.** Assume $o\notin S$ and $\tau^*<\infty$. Then:

1. **(Safety — never overshoot.)** $t_k \le \tau^*$ for every $k$.
2. **(Convergence to the *first* hit.)** $t_k\uparrow\tau^*$, and
   $\rho(\tau^*)\in S$.

*Proof.* **(1)** By induction. $t_0 = 0\le\tau^*$. If $t_k\le\tau^*$,
then since $\rho(\tau^*)\in S$ ($S$ is closed and $\rho$ continuous, so
the infimum defining $\tau^*$ is attained — a minimizing sequence of hit
times has a convergent subsequence with limit in $S$),

$$
d_S(\rho(t_k)) \ \le\ |\rho(t_k)-\rho(\tau^*)| \ =\ \tau^*-t_k,
$$

(the distance to $S$ is at most the distance to the particular point
$\rho(\tau^*)\in S$; the ray is unit-speed), hence $t_{k+1} = t_k +
d_S(\rho(t_k)) \le \tau^*$.

**(2)** $(t_k)$ is nondecreasing ($d_S\ge0$) and bounded above by
$\tau^*$, hence convergent (`01` §6.1 — completeness earning its keep) to
some $t_\infty\le\tau^*$. The steps then vanish: $d_S(\rho(t_k)) =
t_{k+1}-t_k\to0$. Since $d_S$ is continuous — indeed **1-Lipschitz**,
`ml-geometry/15` Lemma 4.1 — and $\rho$ is continuous,
$d_S(\rho(t_\infty)) = \lim_k d_S(\rho(t_k)) = 0$, so
$\rho(t_\infty)\in S$ ($d_S=0$ exactly on the closed set $S$). By
minimality of $\tau^*$, $t_\infty\ge\tau^*$; with (1), $t_\infty =
\tau^*$. $\blacksquare$

> **Type-check — what was *not* assumed.** No smoothness of $S$, no
> manifold structure, no rectifiability even: $S$ may be a fractal
> sublevel set, a union of primitives, anything closed. The entire proof
> runs on three repo staples — closedness, completeness (`01` §6),
> and the 1-Lipschitz bound (`ml-geometry/15` Lemma 4.1). Where
> smoothness *does* re-enter: shading a raymarched surface needs a
> normal, obtained as $\nabla f/|\nabla f|$ — which exists only
> $\mathcal{L}^n$-a.e. (Rademacher, `notes/07` Theorem 1.2, since
> $f$ is 1-Lipschitz), satisfies $|\nabla d_S|=1$ a.e. off $S$ (the
> eikonal equation, at recognition depth), and in practice is taken by
> finite differences — an a.e.-defined gradient sampled and hoped
> smooth, the `notes/07` worldview running on a GPU.

> **Practitioner corollaries.** The proof shows convergence can be slow
> when the ray grazes $S$ (steps $\tau^*-t_k$ shrink only geometrically
> when the ray passes near the surface without hitting) — the reason
> real implementations cap iterations and accept an $\varepsilon$-band
> hit $d_S<\varepsilon$; and it shows why an *underestimating* distance
> bound ($0\le g\le d_S$) preserves safety (1) verbatim — only equality
> in the Lipschitz bound was used — which is why "cheap conservative
> distance bounds" are legitimate SDFs for marching (Hart 1996; Quilez,
> SDF).

---

## 3. Convex Collision: Separation, Support, GJK

**Theorem 3.1 (separating hyperplane, compact case).** Let
$A,B\subseteq\mathbb{R}^n$ be disjoint, convex, closed, with $A$ compact.
Then there exist $u\ne0$ and $c_1<c_2$ with $\langle a,u\rangle\le c_1 <
c_2\le\langle b,u\rangle$ for all $a\in A$, $b\in B$.

*Proof (depth B/C — a corollary of the repo's own projection anchors).*
$A-B := \{a-b\}$ is convex and closed (compact + closed sum), and
$0\notin A-B$ (disjointness). By the projection theorem onto the closed
convex set $A-B$ (`01` §9's Hilbert projection anchor, in
finite dimensions `01` §4), there is a unique nearest point $u^*\in A-B$
to $0$, $u := -u^*\ne0$, and the variational inequality
$\langle 0-u^*,\,z-u^*\rangle\le0\ \forall z\in A-B$ rearranges to
$\langle z,u\rangle \le \langle u^*,u\rangle = -|u^*|^2 < 0$ for all
$z\in A-B$, i.e. $\langle a,u\rangle < \langle b,u\rangle$ uniformly with
gap $|u^*|^2$; take $c_1 = \sup_A\langle\cdot,u\rangle$,
$c_2 = c_1+|u^*|^2$. $\blacksquare$

Three structural consequences, at operational depth:

- **Support functions decide everything.** $A\cap B\ne\varnothing \iff
  0\in A\ominus B$, and the convex set $A\ominus B$ is known to a query
  direction $u$ through $h_{A\ominus B}(u) = h_A(u) + h_{-B}(u)$ — two
  support evaluations. **GJK** (GJK 1988; Ericson, RTCD, Ch. 9) is a
  simplex descent on exactly this: it seeks the point of $A\ominus B$
  nearest the origin (Theorem 3.1's $u^*$!) using only support queries —
  the projection theorem run as an algorithm. This is the narrow-phase
  engine behind convex `MeshCollider` interaction.
- **Metric balls are the universal probe.** `Physics.OverlapSphere`
  returns $\{$colliders $C$ : $d_C(c)\le r\}$ — for each collider a
  single distance comparison, which for convex $C$ is again a projection
  computation. The query region is `notes/06` §5's metric ball, and
  nothing about it is smooth.
- **Contact normals are Theorem 3.1's $u$.** The direction the physics
  solver pushes along is the separating direction at zero gap — the
  variational inequality's gradient, which on smooth boundary pieces
  agrees with `31` Theorem 2.1's surface normal.

---

## 4. Rigid Bodies in One Table

A rigid body's configuration space is $SE(3)$ (`31` §1): position
$\in\mathbb{R}^3$, attitude $\in SO(3)$ (`30`). Its rotational inertia
about the center of mass is the **inertia tensor**

$$
I := \int_{\mathcal{B}} \varrho(x)\,\big(|x|^2\,\mathrm{Id} - x\,x^\top\big)\,d\mathcal{L}^3(x) \ \in \mathbb{R}^{3\times3},
$$

manifestly symmetric and positive semidefinite (for $w\in\mathbb{R}^3$,
$w^\top I w = \int\varrho\,|x\times w|^2 \ge 0$ — direct identity). By
the spectral theorem (`01` §4.7 — the same anchor that ran PCA in
`ml-geometry/13` §4), $I$ diagonalizes in an orthonormal basis: the
**principal axes**, with eigenvalues the principal moments. Angular
momentum is $L = I\omega$ — a *symmetric operator applied to the spin
vector*, so $L\nparallel\omega$ unless $\omega$ is an eigenvector, which
is the one-line explanation of tumbling (torque-free motion is unstable
about the middle eigenvalue — the tennis-racket effect; Arnold, MMCM,
Ch. 6, recognition depth). Engines expose exactly this decomposition:
Unity stores `inertiaTensor` (the eigenvalues) plus
`inertiaTensorRotation` (the eigenbasis, as a `30`-style quaternion) —
the spectral theorem is literally two serialized fields (Unity Docs,
Rigidbody.inertiaTensor).

---

## 5. The `ZT` Weave: Explosives as Measure-Theoretic Queries

`(ZT, Assets/Scripts/Rocket.cs, class Rocket, pinned 2026-07-19 @
ce956b0)` implements the rocket blast as a **two-tier ball query**:

```csharp
Collider[] innerHits = Physics.OverlapSphere(transform.position, explodeRadius, mask);
Collider[] outerHits = Physics.OverlapSphere(transform.position, outerRadius, mask);
```

full damage on $B_{r_1}(c)$, reduced damage on the annulus
$B_{r_2}(c)\setminus B_{r_1}(c)$ (membership in the annulus enforced by
excluding inner hits — with a `HashSet` of instance IDs deduplicating
multi-collider enemies: a quotient by the collider$\to$enemy map, so
damage is a function of the *enemy*, not the collider). Typed: the
damage field is the radially symmetric simple function

$$
\mathrm{dmg}(x) = D_1\,\mathbf{1}_{B_{r_1}(c)}(x) + D_2\,\mathbf{1}_{B_{r_2}(c)\setminus B_{r_1}(c)}(x)
$$

— a two-step approximation (in exactly `notes/06` §3.1's simple-function
sense) of the continuous falloff kernels physics would suggest
($1/r^2$ from `34`'s solid-angle geometry, or a smooth spline as in
`AddExplosionForce`). The `Tornado`/`IceBullet` variants are the same
query pattern with different kernels and statuses.

> **A pinned-revision flag (per `NOTATION_GAME.md` §4).** At the pinned
> commit, `Rocket.FixedUpdate` sets `rocketMovement.Set(0f, 0f, speed)`
> and then translates by `rocketMovement * speed * Time.deltaTime` — the
> inspector value enters **twice**, so the projectile's realized velocity
> scales as $\text{speed}^2$ (a rocket at `speed` $=8$ flies four times
> as fast as one at $4$, not twice). Dimensionally: the code's "speed" is
> being used once as a velocity and once as a dimensionless gain. Either
> normalization ($\texttt{Set}(0,0,1)$, or dropping the second factor)
> restores linear tuning. Flagged in the `16` claim-ledger spirit:
> an observation about the pinned revision, possibly fixed upstream.

```text
scene colliders S (via 31's transforms)
   │                                        │
   │  ray query (§1–§2)                     │  region query (§1, §3)
   ▼                                        ▼
Physics.Raycast — τ* against S         Physics.OverlapSphere — {d_C(c) ≤ r}
   │  (Thm 2.1: sphere tracing,             │  (metric balls; convex case = projection
   │   1-Lipschitz d_S, no smoothness)      │   theorem via support functions / GJK, §3)
   ▼                                        ▼
hitscan shots (PlayerShooting)         blast tiers (Rocket): simple-function damage field
                    └──── contact normals = separating directions (§3) → solver pushes; inertia (§4) spins
```

---

## References Used

- **Hart 1996** — sphere tracing; Theorem 2.1 is its correctness core,
  proved here with the repo's own Lipschitz machinery.
- **Quilez, SDF** — the practitioner SDF canon (§2's corollaries).
- **GJK 1988**; **Ericson, RTCD** — Ch. 9 (GJK), Ch. 5 (ray queries);
  §3's algorithmic layer.
- **Arnold, MMCM** — Ch. 6 (rigid-body motion, principal axes; §4 at
  recognition depth).
- **Unity Docs** — Physics.Raycast, Physics.OverlapSphere,
  Rigidbody.inertiaTensor / inertiaTensorRotation, AddExplosionForce
  (accessed 2026-07-19).
- **ZT** (pinned 2026-07-19 @ `ce956b0`) — `Assets/Scripts/Rocket.cs`
  (two-tier blast, dedup, the speed² flag);
  `Assets/Scripts/Player/PlayerShooting.cs` (hitscan);
  `Assets/Scripts/Tornado.cs`, `IceBullet.cs` (kernel variants).
- **`ml-geometry/15`** Lemma 4.1; **`notes/07`** §1 (Rademacher/eikonal);
  **`01`** §4, §6, §9 (projection theorems, completeness).

Full bibliographic data: `game-geometry/NOTATION_GAME.md` §3–§4.

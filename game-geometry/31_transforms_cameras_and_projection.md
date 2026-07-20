# 31 — Transforms, Cameras, and Projection: the Scene Graph as Geometry

**Cross-references.** `game-geometry/NOTATION_GAME.md` (typing §1.2, `ZT`
convention §4); `game-geometry/30` (rotations — the $A$ inside every rigid
transform); `notes/02` §6 (pushforward vs pullback — §2 is its most
concrete instance in the repository), `notes/04` §1.1 (musical
isomorphisms — why a "normal vector" is a disguised covector); `notes/02`
§1–2 (charts/quotients — homogeneous coordinates in §3); feeds `32`
(colliders live in world space via these transforms) and `34` (the camera
is the measure-theoretic observer).

**Register.** Settled classical (19th-century projective geometry;
1960s–70s graphics pipeline). Engine citations per `NOTATION_GAME.md` §5.

**The Unity mechanics this document owns.** `Transform` hierarchies
(local vs world space), `Camera.ScreenPointToRay`, perspective vs
orthographic cameras (`ZT`'s isometric ↔ first-person switch), and the
inverse-transpose normal matrix that shaders apply.

---

## 0. Standing Definitions

Everything from `NOTATION_GAME.md` §1.2 ($SE(3)$, $\operatorname{Aff}(3)$,
homogeneous coordinates, $\mathbb{RP}^3$, the $4\times4$ transform $T$).
Additionally:

$$
S\subseteq\mathbb{R}^3 \ \text{a surface}, \quad p\in S, \quad T_pS \ \text{its tangent plane}, \quad n\in\mathbb{R}^3 \ \text{a normal: } \langle n,t\rangle = 0 \ \forall t\in T_pS.
$$

$$
A\in GL(3) \quad \text{the linear part of a transform (rotation, scale, shear)}, \qquad A^{-\top} := (A^{-1})^\top = (A^\top)^{-1}.
$$

$$
\text{Camera frame: eye } e\in\mathbb{R}^3, \ \text{orthonormal (right, up, forward)} = (r,u,f), \qquad \text{vertical FOV } 2\phi, \ \text{aspect ratio } a.
$$

---

## 1. The Scene Graph: Composition in $\operatorname{Aff}(3)$

An engine `Transform` is the affine map $x\mapsto Ax+b$, stored as
$T=\begin{bmatrix}A&b\\0&1\end{bmatrix}$ acting on homogeneous columns.
The **scene graph** (parent–child hierarchy) means: a child's world map is
the *composition* $T_{\mathrm{world}} = T_{\mathrm{parent}}\,
T_{\mathrm{local}}$ — matrix products running root-to-leaf. Three typed
facts the engine API encodes:

| Datum | Homogeneous form | Transforms by | Unity API |
|---|---|---|---|
| point | $(x,1)$ | $T$ (rotate + scale + **translate**) | `TransformPoint` |
| direction (tangent vector) | $(v,0)$ | linear part $A$ only | `TransformDirection` / `TransformVector` |
| normal (covector in costume) | — | $A^{-\top}$, then renormalize (§2) | the shader's "normal matrix" |

> **Type-check.** The $w$-coordinate is a *type tag*: $w=1$ marks
> elements of the affine space (translations apply), $w=0$ marks elements
> of its tangent space (translations act trivially — differentiate
> $x\mapsto Ax+b$ and $b$ dies). The third row of the table is not
> expressible by any choice of $w$ at all — normals are not vectors but
> covectors (§2) — which is precisely why graphics APIs ship a *separate*
> normal matrix rather than a fourth $w$-convention. `30` §4's yaw/pitch
> split across two transforms is this section's composition law used as a
> control scheme; `ZT`'s `FirstPersonMovement` computing `movement =
> Orentation.forward * v + Orentation.right * h` (ZT,
> Assets/Scripts/FirstPersonMovement.cs, pinned 2026-07-19 @ ce956b0) is
> the direction row: input is assembled *in the moving frame* and pushed
> to world space by the frame's linear part.

---

## 2. Anchor Proof: Normals Transform by the Inverse-Transpose

**Theorem 2.1.** Let $S$ have tangent plane $T_pS$ with normal $n$ at
$p$, and let $A\in GL(3)$ act on space. Then the image surface $A(S)$ has
tangent plane $A(T_pS)$ at $Ap$, and its normals there are the nonzero
multiples of

$$
m = A^{-\top}n.
$$

For $A\in SO(3)$ (rigid), $A^{-\top} = A$ and the distinction is
invisible; for non-uniform scale or shear, $A^{-\top}\ne A$ and using
$An$ in place of $A^{-\top}n$ is *wrong*.

*Proof.* Tangent vectors are velocities of curves in $S$ (`notes/02`
§3.1), and $A$ is linear, so curves map to curves with velocity $At$:
the image tangent plane is $A(T_pS)$. A normal $m$ of the image must
satisfy, for every $t\in T_pS$,

$$
0 = \langle m, At\rangle = \langle A^\top m, t\rangle,
$$

i.e. $A^\top m$ annihilates $T_pS$, i.e. $A^\top m = \lambda n$ for some
$\lambda\ne0$ (the annihilator of a plane in $\mathbb{R}^3$ is a line).
Hence $m = \lambda A^{-\top}n$. $\blacksquare$

> **Type-check — the covector unmasked.** The invariant object here is
> the *annihilating covector* $\omega = \langle n,\cdot\rangle\in
> (\mathbb{R}^3)^*$ with $T_pS = \ker\omega$. Covectors pull back by the
> transpose (`notes/02` §6.3: $F^*\eta = \eta\circ dF$), so the image
> surface's covector is $\omega\circ A^{-1}$, whose matrix is
> $n^\top A^{-1}$ — transpose it and you are holding $A^{-\top}n$. The
> "normal vector" $n$ was only ever $\sharp\omega$ under the Euclidean
> metric (`notes/04` §1.1), and the musical isomorphism *does not commute
> with non-orthogonal maps* — that failure **is** Theorem 2.1. This is
> the repository's primitive-vs-derived discipline (`notes/NOTATION.md`
> §1.7) caught doing production work: the classic "lighting breaks when I
> scale the model" bug is a type error — a covector shipped as a vector.
> Engines apply exactly $A^{-\top}$ (the "normal matrix") in the vertex
> shader, renormalizing after (Unity Docs; RTR 4e, Ch. 4).

---

## 3. Homogeneous Coordinates Are a Chart on $\mathbb{RP}^3$

The map $(x,1)\sim\lambda(x,1)$ embeds affine space as the $w\neq0$
chart of projective space $\mathbb{RP}^3 = (\mathbb{R}^4\setminus\{0\})/
\mathbb{R}^\times$ — a quotient manifold (`notes/02` §1–2; the benign
free-quotient situation, as with $\mathbb{RP}^3\cong SO(3)$ in `30` §1's
type-check — the *same manifold*, met twice in two documents). The
**perspective divide** $(x,y,z,w)\mapsto(x/w,y/w,z/w)$ is nothing but the
chart map; the "points at infinity" $w=0$ are the chart's complement —
which is why directions ($w=0$, §1) are literally *points at infinity*:
parallel lines meet there, and a direction is the name of that meeting
point. A perspective projection matrix is an element of $GL(4)$ acting on
$\mathbb{RP}^3$ carrying the view frustum onto a cube; that it can move
finite points to infinity and back (the eye plane $\to$ $w=0$) is
possible only because it acts on the projective manifold, not on
$\mathbb{R}^3$. *(Classical projective geometry, stated at operational
depth; RTR 4e, Ch. 4, for the exact matrix conventions.)*

An **orthographic** camera replaces the projective action by an affine
one (parallel projection — the $w$-row left trivial). `ZT`'s two
perspectives are exactly this dichotomy: the isometric top-down view is
the orthographic/affine regime, the first-person view the projective one,
and the switch `(ZT, Assets/Scripts/CamerPOV.cs and CameraMovement.cs,
pinned 2026-07-19 @ ce956b0)` swaps which geometry the player is looking
through.

---

## 4. Anchor Derivation: `ScreenPointToRay`, and the Floor Hit Behind `Turning()`

**Proposition 4.1 (the screen ray).** Fix the camera frame of §0 and a
screen point with normalized device coordinates $(s_x,s_y)\in[-1,1]^2$
(pixel coordinates affinely rescaled). The set of world points that
project to $(s_x,s_y)$ is the ray

$$
\rho(t) = e + t\,\hat d, \qquad \hat d \propto f + (s_x\,a\tan\phi)\,r + (s_y\tan\phi)\,u, \qquad t>0.
$$

*Proof.* In camera coordinates a world point $p$ has coordinates
$(\langle p-e,r\rangle, \langle p-e,u\rangle, \langle p-e,f\rangle) =:
(c_r,c_u,c_f)$, and perspective projection sends it to $(s_x,s_y) =
\big(\tfrac{c_r}{c_f\,a\tan\phi},\ \tfrac{c_u}{c_f\tan\phi}\big)$ (the
frustum's half-height at unit depth is $\tan\phi$; the divide is §3's
chart map). Fixing $(s_x,s_y)$ and letting the depth $c_f = t>0$ vary
gives $c_r = t\,s_x a\tan\phi$, $c_u = t\,s_y\tan\phi$ — exactly the
displayed ray in the $(r,u,f)$ frame. $\blacksquare$

**Corollary 4.2 (the floor hit).** Intersecting $\rho$ with the ground
plane $\{y=0\}$ solves $\langle e,\hat y\rangle + t\langle\hat d,\hat y
\rangle = 0$, i.e. one division, $t^* = -e_y/\hat d_y$, valid when
$\hat d_y\ne0$ (the camera actually looks at the floor). This — composed
with `30` §1's `LookRotation` — is the complete mathematical content of
isometric mouse-aim: `(ZT, Assets/Scripts/Player/PlayerMovement.cs,
Turning(), pinned 2026-07-19 @ ce956b0)` calls
`Camera.main.ScreenPointToRay(Input.mousePosition)` (Proposition 4.1,
with Unity performing the pixel$\to$NDC rescale) and
`Physics.Raycast(camRay, out floorHit, camRayLength, floorMask)`
(Corollary 4.2, generalized from the plane to arbitrary colliders — `32`
§1 — with the `floorMask` restricting to the plane case), then aims the
player at `floorHit.point`.

```text
pixel (mouse) ──affine rescale──► NDC (s_x, s_y) ∈ [−1,1]²
      │ (Prop 4.1: undo the chart map at every depth)
      ▼
world ray ρ(t) = e + t d̂                          ── the fiber of the projection over the pixel
      │ (Cor 4.2 / Physics.Raycast: first intersection)
      ▼
floorHit.point ──(y := 0, LookRotation — 30 §1)──► aim rotation ∈ SO(3) ──MoveRotation──► physics
```

> **Type-check — what a pixel *is*.** Proposition 4.1 says a pixel is not
> a point of the world but a *fiber*: the preimage of a chart value under
> the projection $\mathbb{R}^3\supset\text{frustum}\to[-1,1]^2$. Clicking
> selects a fiber; only a raycast (a choice of point *on* the fiber —
> here, the first collider hit) turns it back into world data. Every
> mouse-driven 3D mechanic decomposes as fiber-then-section this way, and
> `34` §1 will reuse the same projection as a *measure-theoretic* map —
> the change of variables between pixel area and world solid angle.

---

## References Used

- **RTR 4e** — Ch. 4 (transform pipeline, normal matrix, projection
  matrices; the engine-convention companion to §§1–4).
- **Ericson, RTCD** — Ch. 5 (ray–plane and ray–primitive intersection,
  the general form of Corollary 4.2).
- **Unity Docs** — Transform.TransformPoint / TransformDirection,
  Camera.ScreenPointToRay, Physics.Raycast, Camera.orthographic
  (accessed 2026-07-19).
- **ZT** (pinned 2026-07-19 @ `ce956b0`) —
  `Assets/Scripts/Player/PlayerMovement.cs` (Turning());
  `Assets/Scripts/FirstPersonMovement.cs` (frame-relative movement);
  `Assets/Scripts/CamerPOV.cs`, `Assets/Scripts/CameraMovement.cs`
  (perspective switch).
- **`notes/02`** §3, §6 (velocities of curves; pushforward/pullback —
  Theorem 2.1); **`notes/04`** §1.1 (musical isomorphisms — the covector
  reading).

Full bibliographic data: `game-geometry/NOTATION_GAME.md` §3–§4.

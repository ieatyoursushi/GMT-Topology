# Notation and Standing Conventions — Phase 5 (Game-Mechanics Geometry)

**Role of this file.** The Phase-5 analogue of `notes/NOTATION.md`,
`ml-geometry/NOTATION_ML.md`, and `stochastic/NOTATION_STOCH.md`. It adds
only what the game-mechanics phase needs: the typed objects of rotations,
transforms, collision, navigation, and rendering; an engine-facing
citation key; and the cross-repo convention for citing **Zombtoy**, the
author's own Unity game, which plays for this phase the role
DirectIndexing_ML plays for Phases 3–4.

**Status.** Phase 5 is complete: this file, the hub
(`game-geometry/00_game_mechanics_geometry_hub.md`), and deep dives
`30`–`34`.

---

## 0. Inheritance Declaration

Every document in `game-geometry/` is bound by, without restating:

- **`notes/NOTATION.md` §1, §1.1, §2, §4** — house style, depth scale,
  global typing context, pure-math citation key. Phase 5 leans especially
  on `notes/02` (charts, pushforward/pullback), `notes/04` (metrics,
  geodesics, $\exp$, musical isomorphisms), `notes/06`–`07` (Hausdorff
  measure, Lipschitz maps, the area formula).
- **`ml-geometry/NOTATION_ML.md`** — reused directly: `15` Lemma 4.1 (the
  distance function is 1-Lipschitz — load-bearing in `32`), `14` §3 (mesh
  currents — the LOD remark in `34`), and §4's cross-repo citation
  pattern, instantiated for Zombtoy in §4 below.
- **`stochastic/NOTATION_STOCH.md`** — probability objects reused in `34`
  (Monte Carlo estimators live on `01` §13.3's empirical measures and
  `20`'s $L^2$ geometry).

---

## 1. New Typing Context for Phase 5

### 1.1 Quaternions and rotations

$$
\mathbb{H} := \{a + b\,\mathbf{i} + c\,\mathbf{j} + d\,\mathbf{k} : a,b,c,d\in\mathbb{R}\} \quad \text{the quaternions}, \qquad \mathbf{i}^2=\mathbf{j}^2=\mathbf{k}^2=\mathbf{ijk}=-1,
$$

$$
\bar q := a - b\mathbf{i} - c\mathbf{j} - d\mathbf{k}, \qquad |q|^2 := q\bar q = a^2+b^2+c^2+d^2, \qquad \Im(\mathbb{H}) := \operatorname{span}(\mathbf{i},\mathbf{j},\mathbf{k}) \cong \mathbb{R}^3.
$$

$$
S^3 := \{q\in\mathbb{H} : |q|=1\} \quad \text{unit quaternions (a compact 3-manifold — the sphere in } \mathbb{H}\cong\mathbb{R}^4\text{)},
$$

$$
R_q : \mathbb{R}^3\to\mathbb{R}^3, \qquad R_q(v) := q\,v\,\bar q \quad (v\in\Im(\mathbb{H})), \qquad SO(3) := \{A\in\mathbb{R}^{3\times3} : A^\top A = I,\ \det A = 1\}.
$$

$$
\operatorname{slerp}(q_0,q_1;t) := \frac{\sin((1-t)\Omega)}{\sin\Omega}\,q_0 + \frac{\sin(t\Omega)}{\sin\Omega}\,q_1, \qquad \cos\Omega := \langle q_0,q_1\rangle_{\mathbb{R}^4}, \quad \Omega\in(0,\pi).
$$

### 1.2 Affine and projective transforms

$$
SE(3) := \{x\mapsto Ax+b : A\in SO(3),\ b\in\mathbb{R}^3\} \quad \text{rigid motions}, \qquad \operatorname{Aff}(3) \quad \text{the affine group (adds scale/shear: } A\in GL(3)\text{)}.
$$

$$
\text{homogeneous coordinates:} \quad \text{point } (x,1)\in\mathbb{R}^4, \quad \text{direction } (v,0)\in\mathbb{R}^4, \qquad \mathbb{RP}^3 := (\mathbb{R}^4\setminus\{0\})/\mathbb{R}^\times.
$$

$$
T = \begin{bmatrix} A & b \\ 0 & 1\end{bmatrix} \in \mathbb{R}^{4\times4} \quad \text{an engine "transform"}, \qquad \text{scene graph: } T_{\mathrm{world}} = T_{\mathrm{root}}\,T_{\mathrm{child}}\cdots \ \text{(composition, parent-to-leaf)}.
$$

### 1.3 Collision and distance

$$
d_S(x) := \inf_{p\in S}|x-p| \quad \text{distance to a nonempty closed } S\subseteq\mathbb{R}^n \ \text{(1-Lipschitz: `ml-geometry/15' Lemma 4.1)},
$$

$$
f \ \text{an SDF for a closed region } \Omega: \quad f(x) = d_{\partial\Omega}(x) \text{ outside}, \ -d_{\partial\Omega}(x) \text{ inside}, \qquad |f| = d_{\partial\Omega}.
$$

$$
h_A(u) := \sup_{a\in A}\langle a,u\rangle \quad \text{support function of convex } A, \qquad A\ominus B := \{a-b : a\in A,\ b\in B\} \quad \text{Minkowski difference}.
$$

$$
B_r(c) := \{x : |x-c|\le r\} \quad \text{(the metric ball — the engine's \texttt{OverlapSphere} query region)}.
$$

### 1.4 Navigation

$$
G = (V,E,w) \quad \text{a weighted graph}, \ w:E\to[0,\infty), \qquad g(v) \ \text{cost-so-far}, \quad h:V\to[0,\infty) \ \text{a heuristic}, \quad f := g+h.
$$

$$
h \ \text{admissible} \iff h(v) \le \operatorname{dist}_G(v,\mathrm{goal}) \ \forall v; \qquad h \ \text{consistent} \iff h(u) \le w(u,v)+h(v) \ \forall (u,v)\in E.
$$

$$
\Sigma \quad \text{a polyhedral (piecewise-flat) surface — e.g. a navmesh}, \qquad \delta(v) := 2\pi - \textstyle\sum_{f\ni v}\alpha_f(v) \quad \text{the angle defect at a vertex } v,
$$

$\alpha_f(v)$ the interior angle of face $f$ at $v$; $\chi(\Sigma) = V-E+F$
the Euler characteristic (`00` §23; combinatorial form, topological
invariance black-boxed per `01` §17).

### 1.5 Rendering and sampling

$$
\sigma := \mathcal{H}^2\llcorner S^2 \quad \text{the solid-angle measure (`notes/06' §5 — Hausdorff measure on the unit sphere)}, \qquad \sigma(S^2) = 4\pi,
$$

$$
H^2(n) := \{\omega\in S^2 : \langle\omega,n\rangle>0\} \quad \text{the hemisphere about a unit normal } n,
$$

$$
L(p,\omega) \quad \text{radiance}, \qquad f_r(p,\omega_i,\omega_o) \quad \text{BRDF}, \qquad \pi_x : \mathbb{R}^3\setminus\{x\}\to S^2, \ \pi_x(p) := \frac{p-x}{|p-x|} \quad \text{the direction projection}.
$$

$$
\hat I_N := \frac1N\sum_{i=1}^N \frac{f(X_i)}{p(X_i)}, \quad X_i\overset{\mathrm{iid}}\sim p \quad \text{the importance-sampled Monte Carlo estimator of } I = \int f\,d\mathcal{L}^n.
$$

---

## 2. Master Dictionary Supplement: Geometry Object $\to$ Engine Mechanic

| Repo object | Where built | Engine instantiation | File |
|---|---|---|---|
| Geodesics on $S^n$ | `notes/04` §3 | `Quaternion.Slerp` (constant-rate turning/blending) | `30` |
| Chart with a rank-drop singularity | `notes/02` §2 | Euler angles / gimbal lock; the FP pitch clamp at $\pm90°$ | `30` |
| Flow of a vector field / linear ODE | `00` §9, `01` §10 | per-frame `Lerp` smoothing = discretized exponential decay | `30` |
| Pullback of a covector | `notes/02` §6.3, `notes/04` §1.1 | normals transform by inverse-transpose (the non-uniform-scale lighting bug) | `31` |
| Charts on a quotient manifold | `notes/02` §1–2 | homogeneous coordinates, perspective divide ($\mathbb{RP}^3$) | `31` |
| Metric ball | `notes/06` §5 | `Physics.OverlapSphere` (blast radii, range queries) | `32` |
| 1-Lipschitz distance function | `ml-geometry/15` Lemma 4.1 | SDFs; sphere tracing / raymarching | `32` |
| Hilbert projection theorem | `01` §4, §9 | separating hyperplanes → convex collision (GJK) | `32` |
| Spectral theorem | `01` §4.7 | inertia tensor, principal axes of a rigid body | `32` |
| Length minimizers / triangle inequality | `00` §4 | admissible A* heuristics; straight-line paths | `33` |
| Curvature as a concentrated measure | `00` §23, `notes/09` (flavor) | angle defect at navmesh vertices; discrete Gauss–Bonnet | `33` |
| Area formula for Lipschitz maps | `notes/07` §3 | the $\cos\theta/r^2$ geometry factor of light transport | `34` |
| Empirical measures, $L^2$ estimators | `01` §13.3, `stochastic/20` | Monte Carlo rendering; random spawning | `34` |
| Mesh currents converging to a surface | `ml-geometry/14` §3 | LOD / mesh simplification (e.g. Nanite) as convergence control | `34` |

---

## 3. Citation Key — Phase 5

### 3.1 Foundational / classic

- **Shoemake 1985** — K. Shoemake, "Animating Rotation with Quaternion
  Curves," *SIGGRAPH Computer Graphics* 19(3), 1985. *(The paper that
  brought slerp to graphics.)*
- **Hall, Lie** — B. C. Hall, *Lie Groups, Lie Algebras, and
  Representations*, 2nd ed., Graduate Texts in Mathematics 222, Springer,
  2015.
- **Arnold, MMCM** — V. I. Arnold, *Mathematical Methods of Classical
  Mechanics*, 2nd ed., Graduate Texts in Mathematics 60, Springer, 1989.
- **HNR 1968** — P. E. Hart, N. J. Nilsson, B. Raphael, "A Formal Basis
  for the Heuristic Determination of Minimum Cost Paths," *IEEE
  Transactions on Systems Science and Cybernetics* 4(2), 1968. *(A\*.)*
- **GJK 1988** — E. G. Gilbert, D. W. Johnson, S. S. Keerthi, "A Fast
  Procedure for Computing the Distance Between Complex Objects in
  Three-Dimensional Space," *IEEE Journal on Robotics and Automation*
  4(2), 1988.
- **Hart 1996** — J. C. Hart, "Sphere Tracing: A Geometric Method for the
  Antialiased Ray Tracing of Implicit Surfaces," *The Visual Computer*
  12(10), 1996.
- **Kajiya 1986** — J. T. Kajiya, "The Rendering Equation," *SIGGRAPH
  Computer Graphics* 20(4), 1986.
- **Ericson, RTCD** — C. Ericson, *Real-Time Collision Detection*, Morgan
  Kaufmann, 2005.

### 3.2 Practitioner / current (versioned software and living documents — cite by access date)

- **PBR 4e** — M. Pharr, W. Jakob, G. Humphreys, *Physically Based
  Rendering: From Theory to Implementation*, 4th ed., MIT Press, 2023
  (free online at pbr-book.org).
- **RTR 4e** — T. Akenine-Möller, E. Haines, N. Hoffman, A. Pesce, M.
  Iwanicki, S. Hillaire, *Real-Time Rendering*, 4th ed., CRC Press, 2018.
- **Crane, DDG** — K. Crane, *Discrete Differential Geometry: An Applied
  Introduction*, CMU course notes (online; latest revision).
- **Quilez, SDF** — I. Quilez, articles on signed distance functions and
  raymarching, iquilezles.org (accessed 2026-07-19). *(The practitioner
  canon for SDF modeling.)*
- **Karis 2021** — B. Karis, "Nanite: A Deep Dive," *SIGGRAPH 2021
  Advances in Real-Time Rendering* course talk, Epic Games, 2021.
- **Unity Docs** — Unity Technologies, *Unity Manual and Scripting API*,
  docs.unity3d.com (accessed 2026-07-19). Cited inline by API name, e.g.
  `(Unity Docs, Quaternion.Slerp)`.

---

## 4. Cross-Repo Citation Convention: Zombtoy

Phase 5's woven case study is **Zombtoy**
(`https://github.com/ieatyoursushi/Zombtoy`), the author's own Unity 3D
zombie-survival game (isometric + first-person perspectives, .NET score
backend) — playing for this phase the role `DIML` plays for Phases 3–4
(`ml-geometry/NOTATION_ML.md` §4).

**Citation key:** `ZT`.

**Inline form:**

$$
\texttt{(ZT, <path/from/repo/root>, <class or method>, pinned <date> @ <short commit>)}
$$

Example: `(ZT, Assets/Scripts/Player/PlayerMovement.cs, Turning(),
pinned 2026-07-19 @ ce956b0)`.

**Pinning statement (binding for all `ZT` citations in this repo):** all
`ZT` citations refer to the repository as investigated on **2026-07-19**,
at short commit **`ce956b0`** (branch `master`, "WIP: small camera-view
changes"). The repository is under active refactor (parallel
`*Refactored.cs` variants exist for player scripts); citations therefore
name **file + class/method**, never line numbers, and always mean the
*non*-refactored variant unless stated. Where a Phase-5 document flags a
numerical quirk in pinned `ZT` code (e.g. `32` §5 on `Rocket.cs`), the
flag describes the pinned revision and may already be fixed upstream.

---

## 5. Register Note for Phase 5 — Old Mathematics, Versioned Engineering

The *mathematics* of this phase is the most settled in the repository —
Euler angles (18th c.), quaternions (Hamilton 1843), projective geometry
(19th c.), the spectral theorem, A\* (1968), the rendering equation
(1986). Phase-2 register throughout: settled canon, cite the sources.
What is *not* settled is the engineering substrate: engines rename APIs
and games get refactored. Hence the two-tier citation rule: mathematics
by the §3.1 keys; engine behavior by **Unity Docs + access date**; game
code by **`ZT` + pinned commit** (§4). The one place a genuinely
fast-moving research literature enters (GPU-driven LOD pipelines, `34`)
is flagged where it appears.

On the phase's advertised question — *"more Riemannian than Lebesgue?"* —
see the scorecard in the hub, §2.1: the verdict is roughly half-and-half,
and the Lebesgue half is the surprise.

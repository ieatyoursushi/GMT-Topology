# 33 — Navigation: NavMesh Geometry, A\*, and Curvature as a Measure

**Cross-references.** `game-geometry/NOTATION_GAME.md` (typing §1.4, `ZT`
convention §4); `00` §4 (arc length, the straight-line lower bound —
§2's heuristic lemma), `00` §23 (smooth Gauss–Bonnet — §4 is its
polyhedral twin); `notes/04` §4's type-check (the cylinder: flat pieces
can bend extrinsically — §3's unfolding); `notes/09` (curvature as a
weak/measure-like object — §4's reading); `game-geometry/32` §3
(Minkowski sums — navmesh baking in §1); feeds nothing downstream but
closes the phase's AI-movement thread.

**Register.** Settled: A\* is HNR 1968; polyhedral Gauss–Bonnet is
19th-century (Descartes' lost theorem, in fact). Discrete differential
geometry as an organized field is younger but stable at this level
(Crane, DDG). Engine citations per `NOTATION_GAME.md` §5.

**The Unity mechanics this document owns.** `NavMesh` baking,
`NavMeshAgent.SetDestination`, and the geometry of "walkable surface."

---

## 0. Standing Definitions

Everything from `NOTATION_GAME.md` §1.4 (weighted graph, $g,h,f$,
admissible/consistent, polyhedral surface $\Sigma$, angle defect
$\delta(v)$, $\chi$). Additionally: $h(\mathrm{goal}) = 0$ is assumed of
every heuristic; $C^* := \operatorname{dist}_G(\mathrm{start},
\mathrm{goal})$ denotes the optimal cost; and **A\*** means: maintain a
frontier of discovered nodes keyed by $f = g+h$; repeatedly pop a
minimum-$f$ node and relax its out-edges (updating successors'
$g$-values when improved); stop when the goal is popped.

---

## 1. What a NavMesh Is, Geometrically

A **NavMesh** is a piecewise-flat surface-with-boundary $\Sigma$ — a
finite union of planar convex polygons glued edge-to-edge — computed
from the level's collision geometry (`32`) by two operations:

1. **Configuration-space inflation.** An agent of radius $r$ collides
   with an obstacle $O$ iff its *center* lies in the Minkowski sum
   $O\oplus B_r(0)$ — so shrinking the agent to a point at the cost of
   inflating every obstacle (exactly `32` §3's Minkowski calculus) turns
   "can this disk move here" into "is this point in the free space."
   The navmesh is (an approximation of) the free space's walkable
   surface after this erosion — which is why Unity bakes *per agent
   radius* (Unity Docs, NavMesh building).
2. **Polygonalization.** The free surface is meshed into convex cells;
   the **dual graph** (a node per cell, an edge per shared portal,
   weighted by portal-to-portal distance) is the $G=(V,E,w)$ that
   pathfinding actually searches.

So an enemy's "walkable world" is a 2-manifold-with-boundary presented
polyhedrally, and its planner is graph search on the dual — §2 proves the
search correct, §3–§4 study the surface itself.

```text
collision geometry (32) ──⊕ B_r (config-space inflation)──► free space
        │  (mesh into convex cells)
        ▼
polyhedral surface Σ (the NavMesh)  ──dual graph──►  G = (V, E, w)
        │                                               │  (§2: A* with admissible h — optimal)
        │  (§3: unfold faces — geodesics are            ▼
        │   straight lines in the unfolding;         corridor of cells
        │   §4: curvature sits only at vertices)        │  (funnel/string-pulling, recognition)
        ▼                                               ▼
walkable geometry                               taut polygonal path ──► NavMeshAgent steering
```

---

## 2. Anchor Proof: A\* with an Admissible Heuristic Is Optimal

**Lemma 2.1 (consistent $\Rightarrow$ admissible).** If $h$ is
consistent and $h(\mathrm{goal})=0$, then $h$ is admissible.

*Proof.* Let $v = v_0, v_1,\dots,v_m=\mathrm{goal}$ be a cheapest
$v$-to-goal path. Telescoping consistency along it:
$h(v) \le w(v_0,v_1)+h(v_1) \le \cdots \le \sum_i w(v_i,v_{i+1}) +
h(\mathrm{goal}) = \operatorname{dist}_G(v,\mathrm{goal})$. $\blacksquare$

**Lemma 2.2 (the frontier invariant).** At any point of A\*'s execution
before the goal is popped, some node $n'$ on some optimal
start$\to$goal path lies in the frontier with $g(n') =
\operatorname{dist}_G(\mathrm{start},n')$ (its true cost).

*Proof.* Fix an optimal path $P: \mathrm{start}=u_0,u_1,\dots,u_m=
\mathrm{goal}$. Initially $u_0$ qualifies ($g=0$). Inductively, consider
the largest $i$ such that $u_i$ has been popped with exact
$g(u_i)=\operatorname{dist}_G(\mathrm{start},u_i)$ at pop time (pops
only ever finalize the $g$ they were popped with; if no $u_i$ has been
popped yet, $u_0$'s case applies). When $u_i$ was popped, its edge to
$u_{i+1}$ was relaxed, so $u_{i+1}$ entered the frontier with
$g(u_{i+1}) \le g(u_i)+w(u_i,u_{i+1}) =
\operatorname{dist}_G(\mathrm{start},u_{i+1})$ (the prefix of an optimal
path is optimal); $g$-values never increase and never drop below the
true distance (each relaxation adds a genuine path cost), so while
$u_{i+1}$ remains unpopped it sits in the frontier with exact $g$.
$\blacksquare$

**Theorem 2.3.** If $h$ is admissible, A\* terminates (finite $G$,
nonnegative weights) and the goal is popped with $g(\mathrm{goal}) =
C^*$: the returned path is optimal.

*Proof.* Termination: each pop either finalizes a node or re-expands one
with a strictly smaller $g$; $g$-values are sums over a finite set of
nonnegative edge weights with strictly decreasing updates, so pops are
finite. Optimality: suppose the goal is popped with $g(\mathrm{goal}) >
C^*$. At that moment, by Lemma 2.2 there is a frontier node $n'$ on an
optimal path with exact $g(n')$; by admissibility,

$$
f(n') = g(n') + h(n') \ \le\ \operatorname{dist}_G(\mathrm{start},n') + \operatorname{dist}_G(n',\mathrm{goal}) \ =\ C^* \ <\ g(\mathrm{goal}) = f(\mathrm{goal})
$$

(using $h(\mathrm{goal})=0$), contradicting that the goal was the
minimum-$f$ pop. So $g(\mathrm{goal})\le C^*$; and $g$-values are costs
of genuine paths, so $g(\mathrm{goal}) = C^*$. $\blacksquare$

*(HNR 1968, the original; the proof above is the standard modern
arrangement. With consistency, additionally no node is ever usefully
re-expanded — $f$ is nondecreasing along pops — cited at recognition
depth.)*

**Lemma 2.4 (why the Euclidean heuristic is admissible).** If nodes are
points in $\mathbb{R}^n$ and every edge weight is at least the Euclidean
distance between its endpoints, then $h(v) := |v-\mathrm{goal}|$ is
admissible: for any path $v = u_0,\dots,u_m = \mathrm{goal}$, iterated
triangle inequality gives $\sum_i w(u_i,u_{i+1}) \ge \sum_i
|u_{i+1}-u_i| \ge |v-\mathrm{goal}|$. $\blacksquare$ — This is `00` §4's
"straight lines minimize length" in its combinatorial form, and it is
the heuristic navmesh planners actually use.

---

## 3. Geodesics on a Polyhedral Surface: Unfold and Pull Taut

Within one face, $\Sigma$ is a flat polygon; across an edge, two faces
are **jointly flat**: rotating one about the shared edge into the
other's plane is an isometry of the pair onto a planar quadrilateral
(this is `notes/04` §4's cylinder type-check as a tool — the dihedral
bend is extrinsic, invisible to intrinsic length). Consequently a
shortest path crossing an edge sequence is, in the **unfolding** of that
sequence, a straight segment; on $\Sigma$ it is straight inside faces,
straight *across* edges (equal incidence angles), and can turn only at
**vertices** — and, one shows, only at vertices with $\delta(v)\le0$
(around a positively-curved vertex the path can be shortened by cutting
the corner; around a negative one it cannot). The engine's
**string-pulling / funnel** step is exactly this: A\* (§2) returns a
corridor of cells; the funnel algorithm pulls the path taut inside the
corridor, i.e. computes the unfolded straight line subject to the
portal constraints. *(Operational depth; Crane, DDG, for the polyhedral
geodesic theory; Unity Docs for the corridor-then-funnel pipeline.)*

---

## 4. Anchor Proof: Discrete Gauss–Bonnet — Curvature Lives at the Vertices

**Theorem 4.1.** Let $\Sigma$ be a closed (boundaryless) polyhedral
surface with $V$ vertices, $E$ edges, $F$ faces. Then

$$
\sum_{v} \delta(v) \ =\ 2\pi\,\chi(\Sigma), \qquad \chi(\Sigma) = V-E+F.
$$

*Proof.* Double-count interior angles. Summing over vertices first:
$\sum_v\sum_{f\ni v}\alpha_f(v) = \sum_v\big(2\pi-\delta(v)\big) = 2\pi
V - \sum_v\delta(v)$. Summing over faces first: a planar $n_f$-gon has
interior angle sum $\pi(n_f-2)$, so the total is $\sum_f \pi(n_f-2) =
\pi\sum_f n_f - 2\pi F = 2\pi E - 2\pi F$, using $\sum_f n_f = 2E$
(each edge borders exactly two faces on a closed surface). Equating:

$$
2\pi V - \sum_v\delta(v) = 2\pi E - 2\pi F \quad\Longrightarrow\quad \sum_v\delta(v) = 2\pi(V-E+F) = 2\pi\chi. \qquad\blacksquare
$$

*(Descartes' theorem on total angular defect; Crane, DDG, Ch. 5. The
topological invariance of $\chi$ — that any polyhedral decomposition of
the same surface gives the same $V-E+F$ — is the one black-boxed input,
per `01` §17, with `01` §12 as the backdrop.)*

> **Type-check — curvature as a measure, and the guess again.** Compare
> `00` §23: smoothly, $\int_\Sigma K\,dA = 2\pi\chi$. On the polyhedral
> $\Sigma$, faces and edges are flat (the unfolding of §3 — zero
> curvature there), and the *entire* curvature has condensed into point
> masses: the measure $\mu_K := \sum_v \delta(v)\,\delta_{v}$ (angle
> defect times a Dirac at each vertex) satisfies $\mu_K(\Sigma) =
> 2\pi\chi$, the exact Gauss–Bonnet mass. This is the `notes/09` move —
> a pointwise tensor weakening into a measure-valued object under loss
> of smoothness — happening on every game level's navmesh: **curvature
> on a navmesh is not Riemannian data, it is a Radon measure supported
> on the vertex set.** Score one more for the Lebesgue side of the
> phase's wager (hub §2.1); and note the payoff is practical — mesh
> simplification and navmesh baking preserve $\sum\delta$ *exactly*
> whenever they preserve topology, a conservation law used as a sanity
> check in discrete-geometry processing (Crane, DDG).

---

## 5. The `ZT` Weave: a Zombie's World Is a Metric Space

`(ZT, Assets/Scripts/Enemy/EnemyMovement.cs, pinned 2026-07-19 @
ce956b0)` drives every zombie with

```csharp
if (nav.enabled && nav.isOnNavMesh)
    nav.SetDestination(player.position);
```

inside `Update()`, guarded by both combatants being alive. Read through
this document:

- `nav.isOnNavMesh` is a **chart-domain check** — "is the agent's
  position actually a point of the manifold $\Sigma$" — the same
  discipline as `30` §4's pitch clamp, applied to membership rather than
  coordinates. (Its failure mode — an agent knocked off the mesh by
  physics — is a point with no chart, and `SetDestination` there is a
  typed nonsense the guard prevents.)
- Calling `SetDestination(player.position)` **every frame** re-solves §2
  with a moving goal: the realized trajectory is the discrete pursuit
  flow "always move along the current geodesic-toward-the-target,"
  re-linearized each frame — the navigation analogue of `30` §5's
  per-frame lerp (follow a target by repeatedly re-aiming the flow),
  with A\*'s corridor in place of the straight segment.
- All zombie variants (soldiers, mutants, bosses — `ZT` README) share
  one $\Sigma$ per agent radius: their behavioral differences are speed
  and attack parameters, not geometry; a larger-radius boss walks a
  *differently eroded* free space (§1's Minkowski inflation), which is
  why engines bake one navmesh per agent size class.

---

## References Used

- **HNR 1968** — A\*; Theorem 2.3 is the original paper's result in
  modern arrangement.
- **Crane, DDG** — Ch. 5 (angle defect, discrete Gauss–Bonnet —
  Theorem 4.1's home), polyhedral geodesics (§3).
- **Ericson, RTCD** — spatial decomposition background for §1.
- **Unity Docs** — NavMesh building (agent radius), NavMeshAgent.
  SetDestination, NavMeshAgent.isOnNavMesh (accessed 2026-07-19).
- **ZT** (pinned 2026-07-19 @ `ce956b0`) —
  `Assets/Scripts/Enemy/EnemyMovement.cs` (chase loop, guards).
- **`00`** §4, §23 (length lower bound; smooth Gauss–Bonnet);
  **`notes/04`** §4 (intrinsic flatness); **`notes/09`** (the
  curvature-as-measure reading); **`32`** §3 (Minkowski calculus).

Full bibliographic data: `game-geometry/NOTATION_GAME.md` §3–§4.

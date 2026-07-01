# Notation, Standing Conventions, and House Style

**Role of this file.** Every other document in this repository (`00`, `01`, and
`notes/02`–`09`) opens with a short header that links here. This file is the
single source of truth for: (a) the global typing context — the ambient sets,
spaces, and symbols that are always in scope, unless a document explicitly
overrides one; (b) the house style itself, stated once so it does not need to
be re-justified in every file; and (c) a master dictionary translating each
smooth object to its GMT (geometric measure theory) replacement, with types on
both sides.

Read this file once, then treat it as a reference to return to — the way you'd
keep a language's standard-library type signatures open in a second window
while reading its source.

---

## 0. Why redundant typing

Ordinary mathematical prose writes

$$
f(x) = \mathbb{E}[R \mid X = x]
$$

and expects the reader to reconstruct, from context built up over paragraphs,
that $f$ is a function from some space to $\mathbb{R}$, that $x$ ranges over
that space, and that $R$ is real-valued. This repository instead writes

$$
f(x) = \mathbb{E}[R \mid X=x],
\qquad
f : \mathcal{X} \to \mathbb{R},
\qquad
X \in \mathcal{X},
\qquad
R \in \mathbb{R}.
$$

The second form is *redundant on purpose*. Every symbol's type is declared at
the moment it is introduced, immediately next to the equation that defines it,
rather than left to be inferred from prose several sentences away. This is the
mathematical analogue of a statically typed programming language: a reader
should be able to determine the type of every symbol on a line by looking at
that line, without needing to hold the entire surrounding paragraph in
working memory.

This convention costs nothing mathematically — it changes no proof, no
theorem, no definition. It only removes ambiguity for a reader (an accelerated
undergraduate, in this repo's case) who is fluent in calculus and linear
algebra but is still building fluency with the notational conventions of
differential geometry and geometric measure theory.

---

## 1. House Style — the rules every document follows

1. **Full type signatures.** Every map or object is introduced with its
   defining equation *plus* domain and codomain *plus* the type of every free
   symbol in the equation, stated redundantly rather than left implicit.
   Canonical shape:
   $$
   \omega_p : \underbrace{T_pM \times \cdots \times T_pM}_{k \text{ factors}} \to \mathbb{R},
   \qquad
   \omega \in \Omega^k(M),
   \qquad
   p \in M,
   \qquad
   \omega_p \in \Lambda^k(T_p^*M).
   $$

2. **Standing Definitions (§0 of each document).** Every document opens with
   a "§0 Standing Definitions" section that declares the ambient spaces and
   every symbol used throughout that document — a typing context, in the
   sense that a type-checker maintains a context $\Gamma$ of variable-to-type
   bindings. If a symbol is used in §3 of some document, it was declared in
   that document's §0 or in this file.

3. **Type tables.** Whenever several related coordinate- or component-level
   objects are introduced together, they are collected into a table of the
   shape

   | Symbol | Formula / Definition | Type | Meaning |
   |---|---|---|---|
   | $\kappa$ | $\lVert T'(s)\rVert$ | $[0,\infty)$ | curvature of a unit-speed curve |

   rather than left scattered through prose.

4. **Pipeline / dependency diagrams.** Each document contains at least one
   fenced diagram showing how objects flow into one another, with the type of
   each arrow annotated, e.g.:

   ```text
   Smooth manifold M, dim M = n
         ↓  (chart)
   Local coordinates (x¹,…,xⁿ) ∈ φ(U) ⊆ ℝⁿ
         ↓  (partial derivatives at p)
   Coordinate tangent basis {∂/∂x¹|_p,…,∂/∂xⁿ|_p} ⊂ T_pM
         ↓  (dual basis)
   Coordinate cotangent basis {dx¹|_p,…,dxⁿ|_p} ⊂ T_p^*M
   ```

5. **Cross-reference header.** Every document opens with a short block naming
   this file and the sibling documents it depends on or feeds into, e.g.
   `Cross-references: notes/NOTATION.md (typing conventions), notes/02_manifolds_and_tangent_spaces.md (T_pM), 00_..._foundations.md §7 (tangent spaces, survey level).`

6. **Type-check callouts.** Boxed or blockquoted remarks that make explicit:
   (a) degree/dimension-matching rules (a $k$-form integrates only over an
   oriented $k$-dimensional object); (b) what *extra structure* an object
   requires to exist (the gradient $\nabla f$ requires a metric $g$; the
   differential $df$ does not); and (c) which type-level fact breaks when a
   smoothness hypothesis is dropped (the tangent space $T_pM$, defined at
   *every* point of a smooth manifold, degrades to an approximate tangent
   plane defined only $\mathcal{H}^k$-almost everywhere on a rectifiable set).

7. **Primitive-vs-derived discipline.** Whenever an object depends on a choice
   (a metric, an orientation, a chart), the document says so explicitly and
   names the more primitive, choice-free object it is built from. This is the
   same discipline as distinguishing a value from a coordinate representation
   of that value.

8. **Precise inline citations.** Nontrivial theorems are cited inline, in the
   form `(Author, Short Title, location)`, using the Citation Key in §4 below,
   e.g. `(Lee, ISM, Thm 16.11)`, `(Federer, GMT, §3.2.3)`. Each document ends
   with a short "References used" list restating, in full, the sources cited
   in that document.

9. **Selective rigor.** Each document states its main theorems with full
   hypotheses and conclusion (not just a slogan), and works out **at least
   one anchor proof in full** rather than sketching it. Everything else may
   be treated at recognition or operational depth, per the depth scale below.

10. **Tone.** Precise and unhurried, but not needlessly formal — explanatory
    asides are welcome, provided every claim they make is one the surrounding
    formalism actually supports.

### 1.1 Depth scale (used throughout)

Borrowed and kept from the original prerequisite review, because it is a
genuinely useful way to self-assess:

| Depth | You can... | Example |
|---|---|---|
| **A — Recognition** | identify the object/technique when it appears | "this proof is extracting a convergent subsequence via compactness" |
| **B — Operational** | use the theorem correctly in a computation or proof | "the set is compact and $f$ continuous, so $f$ attains a maximum" |
| **C — Ownership** | reproduce the proof from definitions | "I can prove Heine–Borel in $\mathbb{R}$ from nested intervals" |

Each document's anchor proof(s) are written at depth C. Everything else in a
document is typically B, occasionally A when a result is cited but not needed
in full.

---

## 2. Global Typing Context

These bindings are assumed in every document unless locally overridden. A
document's own §0 only needs to declare symbols *beyond* this list.

### 2.1 Euclidean and linear-algebraic base

$$
\mathbb{R}, \quad \mathbb{R}^n, \quad \mathbb{R}_{>0}, \quad \mathbb{Z}_{\ge 0}, \quad \mathbb{N}.
$$

$$
U \subseteq \mathbb{R}^n \text{ open}, \qquad V, W \text{ finite-dimensional real vector spaces}, \qquad V^* = \operatorname{Hom}(V,\mathbb{R}).
$$

$$
\Lambda^k(V^*) = \text{space of alternating $k$-linear maps } V\times\cdots\times V \to \mathbb{R} \quad (k \text{ factors}).
$$

### 2.2 Manifolds

$$
M, N \quad \text{smooth manifolds}, \qquad \dim M = n \in \mathbb{Z}_{\ge 0}.
$$

$$
p, q \in M, \qquad (U,\varphi) \text{ a chart}, \qquad \varphi: U \to \varphi(U) \subseteq \mathbb{R}^n.
$$

$$
T_pM \quad \text{tangent space at } p, \qquad T_pM \cong \mathbb{R}^n \text{ (as a vector space, after a chart)}.
$$

$$
T_p^*M = \operatorname{Hom}(T_pM,\mathbb{R}) \quad \text{cotangent space at } p.
$$

$$
TM = \bigsqcup_{p\in M} T_pM \quad \text{tangent bundle}, \qquad T^*M = \bigsqcup_{p\in M}T_p^*M \quad \text{cotangent bundle}.
$$

$$
C^\infty(M) \quad \text{smooth real-valued functions on } M, \qquad \Omega^0(M) := C^\infty(M).
$$

$$
\Omega^k(M) \quad \text{smooth $k$-forms on } M, \qquad \Omega^k(M) = 0 \text{ for } k > n = \dim M.
$$

$$
\partial M \quad \text{boundary of a manifold-with-boundary } M, \qquad \dim(\partial M) = \dim M - 1.
$$

### 2.3 Riemannian geometry

$$
g \quad \text{Riemannian metric}, \qquad g_p : T_pM \times T_pM \to \mathbb{R} \text{ symmetric, bilinear, positive-definite for every } p \in M.
$$

$$
\nabla \quad \text{a connection (Levi-Civita, unless stated otherwise)}, \qquad \nabla_X Y \in \Gamma(TM) \text{ for } X,Y \in \Gamma(TM).
$$

$$
\Gamma^k_{ij} \quad \text{Christoffel symbols (coordinate-dependent, not tensorial)}.
$$

$$
R(X,Y)Z \quad \text{Riemann curvature endomorphism}, \qquad R : \Gamma(TM)^3 \to \Gamma(TM).
$$

$$
\Delta_g \quad \text{Laplace–Beltrami operator}, \qquad \Delta_g : C^\infty(M) \to C^\infty(M).
$$

### 2.4 Measure theory and GMT

$$
(\Omega,\mathcal{F},\mu) \quad \text{a measure space}, \qquad \mathcal{F} \subseteq 2^\Omega \text{ a } \sigma\text{-algebra}, \qquad \mu : \mathcal{F} \to [0,\infty].
$$

$$
\mathcal{L}^n \quad \text{Lebesgue measure on } \mathbb{R}^n, \qquad \mathcal{H}^k \quad \text{$k$-dimensional Hausdorff measure}, \qquad k \in [0,\infty).
$$

$$
E \subseteq \mathbb{R}^n \quad \text{a Lebesgue/Hausdorff-measurable set}, \qquad f : \mathbb{R}^n \to \mathbb{R}^m \quad \text{Lipschitz (unless stated otherwise)}.
$$

$$
\operatorname{Lip}(f) := \sup_{x\neq y} \frac{|f(x)-f(y)|}{|x-y|} \quad \text{Lipschitz constant of } f.
$$

$$
\mathcal{M}(\mathbb{R}^n) \quad \text{finite (signed) Radon measures on } \mathbb{R}^n.
$$

$$
\Omega_c^k(\mathbb{R}^n) \quad \text{compactly supported smooth $k$-forms (test forms)}.
$$

$$
D_k(\mathbb{R}^n) \quad \text{$k$-dimensional currents}, \qquad T : \Omega_c^k(\mathbb{R}^n) \to \mathbb{R} \text{ continuous linear}, \qquad T \in D_k(\mathbb{R}^n).
$$

$$
V_k(\mathbb{R}^n) \quad \text{$k$-varifolds}, \qquad G(n,k) \quad \text{Grassmannian of $k$-planes through the origin in } \mathbb{R}^n.
$$

$$
BV(\Omega) \quad \text{functions of bounded variation on } \Omega \subseteq \mathbb{R}^n, \qquad P(E) \quad \text{perimeter of } E\subseteq\mathbb{R}^n.
$$

### 2.5 Index conventions

Latin indices $i,j,k,\ell \in \{1,\dots,n\}$ range over coordinate directions
unless stated otherwise. **Einstein summation convention** is used throughout:
a repeated index, once as a subscript and once as a superscript in the same
term, is implicitly summed:

$$
v = v^i \frac{\partial}{\partial x^i}\Big|_p
\quad \text{means} \quad
v = \sum_{i=1}^n v^i \frac{\partial}{\partial x^i}\Big|_p.
$$

This convention is suspended only inside explicit $\sum$ notation or where a
document says so locally.

---

## 3. Master Dictionary: Smooth Geometry $\to$ GMT, With Types

This upgrades the informal "smooth vs. GMT" tables in `00` and `01` by
attaching a type to each row, so the *degradation of structure* is visible as
a change of type, not just a change of vocabulary.

| Smooth object | Type (smooth) | GMT replacement | Type (GMT) | What is lost |
|---|---|---|---|---|
| smooth $k$-submanifold $M$ | $M \subseteq \mathbb{R}^n$, locally a $C^\infty$ chart image | countably $k$-rectifiable set $E$ | $E \subseteq \mathbb{R}^n$, $E \approx \bigcup_j f_j(A_j)$, $f_j$ Lipschitz | charts only Lipschitz, not $C^\infty$; parametrization only up to $\mathcal{H}^k$-null sets |
| tangent space, everywhere | $T_pM$, defined $\forall p \in M$ | approximate tangent plane | $T_pE$, defined for $\mathcal{H}^k$-a.e. $p \in E$ | "everywhere" $\to$ "almost everywhere" |
| smooth chart $\varphi$ | $\varphi: U \to \mathbb{R}^n$, $C^\infty$, invertible | Lipschitz parametrization $f_j$ | $f_j : A_j \subseteq \mathbb{R}^k \to \mathbb{R}^n$, Lipschitz | differentiability only a.e. (Rademacher) |
| Riemannian volume/area form $dV_g$ | smooth $n$-form built from $g$ | Hausdorff measure $\mathcal{H}^k$ | outer measure on subsets of $\mathbb{R}^n$ | no smooth density; only a set function |
| oriented submanifold, as integration domain | $\int_M \omega$, $\omega \in \Omega^k(M)$ | integral current $T \in D_k(\mathbb{R}^n)$ | $T(\omega) := \int_M \omega$, extended to $T: \Omega_c^k(\mathbb{R}^n)\to\mathbb{R}$ | object is now defined *by* its action on test forms, not as a point-set |
| classical boundary $\partial M$ | smooth $(n{-}1)$-manifold | current boundary $\partial T$ | $\partial T(\omega) := T(d\omega)$, $\partial T \in D_{k-1}(\mathbb{R}^n)$ | boundary is defined by duality, not as a point-set operation |
| unoriented smooth surface | $M \subseteq \mathbb{R}^n$, no orientation data | varifold | $V \in V_k(\mathbb{R}^n)$, a Radon measure on $\mathbb{R}^n \times G(n,k)$ | orientation and multiplicity sign are discarded; only mass + tangent-plane data remain |
| classical topological boundary of a set | $\partial E = \overline{E}\setminus \operatorname{int}(E)$ | reduced boundary | $\partial^* E \subseteq \mathbb{R}^n$, points with a measure-theoretic normal | topological boundary can be $\mathcal{H}^{n-1}$-infinite even when $E$ is well-behaved measure-theoretically |
| classical derivative | $Df_p : \mathbb{R}^n \to \mathbb{R}^m$, exists at every $p$ | approximate / distributional derivative | exists $\mathcal{L}^n$-a.e. (Rademacher) or as a measure (BV) | pointwise existence $\to$ a.e. existence or distributional existence |
| classical Stokes theorem | $\int_M d\omega = \int_{\partial M}\omega$ | current boundary identity | $\partial T(\omega) = T(d\omega)$ | statement becomes definitional rather than a derived theorem |
| curvature tensor $R(X,Y)Z$ | pointwise, from $\nabla$ | first variation of a varifold | a linear functional on vector fields, may be a measure | pointwise curvature $\to$ a weak/integrated notion |

---

## 4. Citation Key

Short keys used inline throughout this repository; full bibliographic data
here so it is stated once.

- **Lee, ISM** — J. M. Lee, *Introduction to Smooth Manifolds*, 2nd ed., Graduate Texts in Mathematics 218, Springer, 2013.
- **Lee, IRM** — J. M. Lee, *Introduction to Riemannian Manifolds*, 2nd ed., Graduate Texts in Mathematics 176, Springer, 2018.
- **Tu** — L. W. Tu, *An Introduction to Manifolds*, 2nd ed., Universitext, Springer, 2011.
- **Spivak, CoM** — M. Spivak, *Calculus on Manifolds*, Addison-Wesley, 1965.
- **Bott–Tu** — R. Bott, L. W. Tu, *Differential Forms in Algebraic Topology*, Graduate Texts in Mathematics 82, Springer, 1982.
- **do Carmo, RG** — M. do Carmo, *Riemannian Geometry*, Birkhäuser, 1992.
- **do Carmo, DGCS** — M. do Carmo, *Differential Geometry of Curves and Surfaces*, rev. 2nd ed., Dover, 2016.
- **Federer, GMT** — H. Federer, *Geometric Measure Theory*, Grundlehren der mathematischen Wissenschaften 153, Springer, 1969.
- **Simon, LGMT** — L. Simon, *Lectures on Geometric Measure Theory*, Proceedings of the Centre for Mathematical Analysis, ANU, vol. 3, 1983.
- **Evans–Gariepy, MTFP** — L. C. Evans, R. F. Gariepy, *Measure Theory and Fine Properties of Functions*, rev. ed., CRC Press, 2015.
- **Maggi, SFP&GVP** — F. Maggi, *Sets of Finite Perimeter and Geometric Variational Problems*, Cambridge Studies in Advanced Mathematics 135, Cambridge University Press, 2012.
- **Falconer** — K. Falconer, *The Geometry of Fractal Sets*, Cambridge Tracts in Mathematics 85, Cambridge University Press, 1985.
- **Rudin, RCA** — W. Rudin, *Real and Complex Analysis*, 3rd ed., McGraw-Hill, 1987.
- **Munkres** — J. R. Munkres, *Topology*, 2nd ed., Prentice Hall, 2000.

> **On theorem numbers.** Theorem/section numbers drift between editions and
> printings. Where a document cites a specific number, treat it as pointing to
> the *edition listed above*; if you are working from a different edition,
> use the theorem's name and chapter to relocate it rather than trusting the
> number alone.

---

## 5. How the repository is organized

```text
GMT-topology/
├── README.md
├── 00_differential_geometry_foundations_for_GMT_Topology.md   ← survey/hub: smooth DG through the GMT bridge
├── 01_prerequisite_systematic_review_for_GMT_Topology.md       ← survey/index: prerequisite map + proof-anchor menu
└── notes/
    ├── NOTATION.md                              ← this file
    ├── 02_manifolds_and_tangent_spaces.md        ← deep dive
    ├── 03_differential_forms_and_stokes.md       ← deep dive
    ├── 04_riemannian_metrics_and_curvature.md    ← deep dive
    ├── 05_topology_and_de_rham_cohomology.md      ← deep dive
    ├── 06_measure_theory_bridge.md                ← deep dive
    ├── 07_rectifiability_and_hausdorff_measure.md ← deep dive
    ├── 08_currents.md                             ← deep dive
    └── 09_varifolds_and_finite_perimeter.md        ← deep dive
```

`00` and `01` are **survey-altitude**: they cover the whole arc quickly, with
enough rigor to be trustworthy, and link down into `notes/02`–`09` wherever a
topic deserves a full typed treatment with a worked proof. The `notes/`
documents are **deep-dive-altitude**: each owns one band of the dependency
chain, proves at least one theorem from scratch, and links back up to `00`/`01`
and sideways to its prerequisites.

Recommended reading order: `00` $\to$ `01` $\to$ `notes/02` $\to \cdots \to$
`notes/09`, keeping this file open as a reference throughout.

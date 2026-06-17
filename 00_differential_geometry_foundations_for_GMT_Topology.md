# Differential Geometry Notes for a GMT-Topology Path

**Audience.** Curious accelerated undergraduate with strong multivariable-calculus intuition, informal exposure to manifolds/forms/topology, and a long-term goal of understanding geometric measure theory (GMT), especially spaces that are not assumed smooth.

**Repository role.** This file is the smooth-differential-geometry foundation for a repo about analyzing manifolds through the lens of measure, topology, and weak/non-smooth geometry.

**Core idea.**

> Differential geometry studies spaces where calculus works locally.  
> GMT asks how much of that geometric calculus survives when the space is only measurable, rectifiable, finite-perimeter, varifold-like, current-like, or otherwise not classically smooth.

---

## 0. Mental Model

### 0.1 The local-to-global principle

A smooth manifold is a space that may be globally curved, twisted, or topologically nontrivial, but locally behaves like Euclidean space.

Formally, an \(n\)-dimensional smooth manifold \(M\) is a set equipped with enough structure so that near every point \(p \in M\), there is a chart

$$
\varphi: U \subseteq M \to \varphi(U) \subseteq \mathbb{R}^n.
$$

The local coordinate variables are usually written

$$
(x^1,\dots,x^n).
$$

The point is not that \(M\) literally is \(\mathbb{R}^n\). The point is that calculus on \(M\) is performed by moving locally into \(\mathbb{R}^n\), doing calculus there, then checking that the result does not depend on the coordinate system.

### 0.2 The three layers

| Layer | Object | Intuition |
|---|---|---|
| Topological layer | \(M\) as a space | What points are near each other? What is continuous? |
| Smooth layer | charts with smooth transitions | What does differentiability mean? |
| Metric / geometric layer | \(g\), connection, curvature | How do we measure length, angle, volume, bending? |

GMT weakens the smooth layer and tries to retain enough of the metric/measure/integration layer to still do geometry.

---

## 1. Euclidean Calculus as the Seed

Before manifolds, everything lives in open subsets of Euclidean space.

Let

$$
U \subseteq \mathbb{R}^n.
$$

A scalar field is a function

$$
f: U \to \mathbb{R}.
$$

A vector field is a function

$$
F: U \to \mathbb{R}^n,
\qquad
F(x)=\big(F^1(x),\dots,F^n(x)\big).
$$

A curve is a function

$$
\gamma: I \to \mathbb{R}^n,
\qquad
I \subseteq \mathbb{R}.
$$

A parametrized surface is a function

$$
X: \Omega \to \mathbb{R}^3,
\qquad
\Omega \subseteq \mathbb{R}^2.
$$

More generally, a parametrized \(k\)-dimensional object in \(\mathbb{R}^n\) is a map

$$
X: \Omega \subseteq \mathbb{R}^k \to \mathbb{R}^n.
$$

This is the primitive form of a \(k\)-dimensional manifold sitting inside an ambient \(n\)-dimensional space.

---

## 2. Derivatives as Linear Maps

The derivative of a map is not fundamentally a slope. It is a best linear approximation.

Let

$$
F: U \subseteq \mathbb{R}^n \to \mathbb{R}^m.
$$

At a point \(p \in U\), the derivative is a linear map

$$
DF_p: \mathbb{R}^n \to \mathbb{R}^m.
$$

In coordinates, this linear map is represented by the Jacobian matrix

$$
J_F(p)
=
\begin{bmatrix}
\frac{\partial F^1}{\partial x^1}(p) & \cdots & \frac{\partial F^1}{\partial x^n}(p)\\
\vdots & \ddots & \vdots\\
\frac{\partial F^m}{\partial x^1}(p) & \cdots & \frac{\partial F^m}{\partial x^n}(p)
\end{bmatrix}.
$$

If \(v \in \mathbb{R}^n\), then

$$
DF_p(v)
$$

is the directional output velocity of \(F\) at \(p\) in input direction \(v\).

### Manifold translation

On manifolds, this becomes

$$
dF_p: T_pM \to T_{F(p)}N.
$$

So the deep version of the Jacobian is:

> A derivative maps tangent directions in the domain to tangent directions in the codomain.

---

## 3. Curves, Tangents, and Arc Length

Let

$$
\gamma: I \to \mathbb{R}^n
$$

be a differentiable curve. Its velocity is

$$
\gamma'(t)=\frac{d\gamma}{dt}(t)\in \mathbb{R}^n.
$$

The tangent line at \(\gamma(t_0)\) is determined by the vector \(\gamma'(t_0)\).

The speed is

$$
\|\gamma'(t)\|.
$$

The arc length of \(\gamma\) from \(a\) to \(b\) is

$$
L(\gamma)
=
\int_a^b \|\gamma'(t)\|\,dt.
$$

This is already a measure-like idea: a 1-dimensional object is measured using a 1-dimensional integral, even if the object lives in \(\mathbb{R}^n\).

### Reparametrization

If

$$
\tilde{\gamma}(s)=\gamma(\phi(s))
$$

for a smooth monotone change of parameter \(\phi\), then the geometric curve is the same, even though the coordinate speed changes.

A good differential-geometric object should not depend on the arbitrary parameter chosen to describe it.

---

## 4. Line Integrals

### 4.1 Scalar line integral

Let

$$
f:\mathbb{R}^n\to \mathbb{R},
\qquad
\gamma:[a,b]\to \mathbb{R}^n.
$$

The scalar line integral is

$$
\int_\gamma f\,ds
=
\int_a^b f(\gamma(t))\|\gamma'(t)\|\,dt.
$$

This integrates a scalar density over a curve.

### 4.2 Vector line integral

Let

$$
F:\mathbb{R}^n\to \mathbb{R}^n.
$$

The vector line integral is

$$
\int_\gamma F\cdot d\mathbf{r}
=
\int_a^b F(\gamma(t))\cdot \gamma'(t)\,dt.
$$

This is orientation-sensitive. Reversing the orientation of the curve changes the sign.

### Differential-form translation

A vector field

$$
F=(P,Q,R)
$$

in \(\mathbb{R}^3\) corresponds to the 1-form

$$
\omega = P\,dx+Q\,dy+R\,dz.
$$

Then

$$
\int_\gamma F\cdot d\mathbf{r}
=
\int_\gamma \omega.
$$

This is the beginning of forms replacing vector calculus notation.

---

## 5. Conservative Fields and Potential Functions

A vector field

$$
F:U\subseteq \mathbb{R}^n\to \mathbb{R}^n
$$

is conservative if there exists a scalar function

$$
f:U\to \mathbb{R}
$$

such that

$$
F=\nabla f.
$$

Then for any smooth curve

$$
\gamma:[a,b]\to U,
$$

the fundamental theorem for line integrals says

$$
\int_\gamma F\cdot d\mathbf{r}
=
f(\gamma(b))-f(\gamma(a)).
$$

So a conservative field has path-independent line integrals.

In differential-form language:

$$
F=\nabla f
\quad\Longleftrightarrow\quad
\omega = df.
$$

Here \(df\) is the exterior derivative of the 0-form \(f\).

### Exact vs closed

A 1-form \(\omega\) is exact if

$$
\omega=df
$$

for some 0-form \(f\).

A 1-form is closed if

$$
d\omega=0.
$$

Every exact form is closed:

$$
d(df)=d^2f=0.
$$

The converse depends on topology. On a simply connected domain, closed 1-forms often behave like exact forms. On spaces with holes, closed does not always imply exact.

This is where topology enters the calculus.

---

## 6. From Euclidean Domains to Manifolds

### 6.1 Topological manifold

An \(n\)-dimensional topological manifold is a space \(M\) such that every point \(p\in M\) has a neighborhood \(U\subseteq M\) homeomorphic to an open subset of \(\mathbb{R}^n\).

That means there exists a map

$$
\varphi:U\to \varphi(U)\subseteq \mathbb{R}^n
$$

such that \(\varphi\) is continuous, bijective, and has continuous inverse.

A chart is the pair

$$
(U,\varphi).
$$

An atlas is a collection of charts covering \(M\).

### 6.2 Smooth manifold

A smooth manifold is a topological manifold whose charts are smoothly compatible.

If two charts overlap,

$$
(U,\varphi),
\qquad
(V,\psi),
\qquad
U\cap V\neq \varnothing,
$$

then the transition map is

$$
\psi\circ \varphi^{-1}:
\varphi(U\cap V)\to \psi(U\cap V).
$$

The atlas is smooth if all transition maps are smooth.

This condition means: calculus done in one coordinate system agrees smoothly with calculus done in another coordinate system.

---

## 7. Tangent Spaces

For each point \(p\in M\), the tangent space is denoted

$$
T_pM.
$$

It is an \(n\)-dimensional vector space attached to \(p\).

There are several equivalent ways to define it.

### 7.1 Tangent vectors as velocities of curves

A tangent vector at \(p\) can be thought of as the velocity of a curve passing through \(p\).

Let

$$
\gamma:(-\varepsilon,\varepsilon)\to M
$$

with

$$
\gamma(0)=p.
$$

Then \(\gamma'(0)\) represents a tangent vector in \(T_pM\).

Two curves represent the same tangent vector if they have the same derivative in local coordinates.

### 7.2 Tangent vectors as derivations

A tangent vector can also be defined as an operator

$$
v:C^\infty(M)\to \mathbb{R}
$$

satisfying linearity and the Leibniz rule:

$$
v(af+bg)=a\,v(f)+b\,v(g),
$$

$$
v(fg)=f(p)v(g)+g(p)v(f).
$$

This definition is important because it does not require viewing \(M\) as embedded in Euclidean space.

### 7.3 Coordinate basis

Given local coordinates

$$
(x^1,\dots,x^n),
$$

the tangent space \(T_pM\) has coordinate basis

$$
\left\{
\frac{\partial}{\partial x^1}\bigg|_p,
\dots,
\frac{\partial}{\partial x^n}\bigg|_p
\right\}.
$$

Any tangent vector can be written

$$
v
=
v^i \frac{\partial}{\partial x^i}\bigg|_p,
$$

using Einstein summation convention.

The repeated index \(i\) means

$$
v
=
\sum_{i=1}^n v^i\frac{\partial}{\partial x^i}\bigg|_p.
$$

---

## 8. Vector Fields and Flows

A vector field assigns a tangent vector to each point:

$$
X:M\to TM,
\qquad
X(p)\in T_pM.
$$

Here

$$
TM=\bigsqcup_{p\in M}T_pM
$$

is the tangent bundle.

In local coordinates,

$$
X
=
X^i\frac{\partial}{\partial x^i}.
$$

A flow line or integral curve of \(X\) is a curve

$$
\gamma:I\to M
$$

satisfying

$$
\gamma'(t)=X(\gamma(t)).
$$

Thus an ODE is geometrically a rule saying:

> the tangent vector of the curve must equal the vector field at each point.

This is why ODEs naturally live inside differential geometry.

---

## 9. Cotangent Spaces and 1-Forms

The cotangent space at \(p\) is the dual vector space

$$
T_p^*M = \operatorname{Hom}(T_pM,\mathbb{R}).
$$

Elements of \(T_p^*M\) are covectors.

A 1-form is a smooth assignment

$$
\omega(p)\in T_p^*M.
$$

Given local coordinates

$$
(x^1,\dots,x^n),
$$

the cotangent basis is

$$
\{dx^1|_p,\dots,dx^n|_p\}.
$$

A 1-form has local expression

$$
\omega
=
\omega_i\,dx^i.
$$

It eats a vector field and returns a scalar field:

$$
\omega(X)
=
\omega_iX^i.
$$

### Vector field vs 1-form

A vector field is a direction at each point.

A 1-form is a measurement device for directions at each point.

A Riemannian metric can convert between them, but without a metric they are different types of objects.

---

## 10. Differential Forms

A \(k\)-form is an alternating multilinear map

$$
\omega_p:
\underbrace{T_pM\times\cdots\times T_pM}_{k\text{ copies}}
\to \mathbb{R}.
$$

The space of smooth \(k\)-forms on \(M\) is denoted

$$
\Omega^k(M).
$$

A \(0\)-form is a smooth function:

$$
\Omega^0(M)=C^\infty(M).
$$

A 1-form eats one tangent vector.

A 2-form eats two tangent vectors and measures oriented infinitesimal area.

A 3-form eats three tangent vectors and measures oriented infinitesimal volume.

On an \(n\)-dimensional manifold, the highest nonzero degree is \(n\):

$$
\Omega^k(M)=0
\qquad
\text{for }k>n.
$$

### Wedge product

The wedge product combines forms:

$$
\wedge:\Omega^k(M)\times \Omega^\ell(M)\to \Omega^{k+\ell}(M).
$$

It is antisymmetric:

$$
dx^i\wedge dx^j
=
- dx^j\wedge dx^i.
$$

In particular,

$$
dx^i\wedge dx^i=0.
$$

This encodes orientation and signed area/volume.

---

## 11. Exterior Derivative

The exterior derivative is an operator

$$
d:\Omega^k(M)\to \Omega^{k+1}(M).
$$

It satisfies

$$
d^2=0.
$$

That is,

$$
d(d\omega)=0
$$

for every differential form \(\omega\).

This single identity contains many familiar vector-calculus identities.

### 11.1 Degree 0

Let

$$
f\in \Omega^0(M).
$$

Then

$$
df\in \Omega^1(M).
$$

In coordinates,

$$
df
=
\frac{\partial f}{\partial x^i}dx^i.
$$

In Euclidean vector calculus, this is the gradient information.

### 11.2 Degree 1 in \(\mathbb{R}^3\)

Let

$$
\omega=P\,dx+Q\,dy+R\,dz.
$$

Then

$$
d\omega
=
(Q_x-P_y)\,dx\wedge dy
+
(R_x-P_z)\,dx\wedge dz
+
(R_y-Q_z)\,dy\wedge dz.
$$

This corresponds to curl after using the Euclidean metric and orientation to identify 2-forms with vector fields.

### 11.3 Degree 2 in \(\mathbb{R}^3\)

Let

$$
\eta
=
P\,dy\wedge dz
+
Q\,dz\wedge dx
+
R\,dx\wedge dy.
$$

Then

$$
d\eta
=
(P_x+Q_y+R_z)\,dx\wedge dy\wedge dz.
$$

This corresponds to divergence.

### 11.4 Translation table

| Form statement | Vector-calculus shadow in \(\mathbb{R}^3\) |
|---|---|
| \(d:\Omega^0\to\Omega^1\) | gradient |
| \(d:\Omega^1\to\Omega^2\) | curl |
| \(d:\Omega^2\to\Omega^3\) | divergence |
| \(d^2=0\) | \(\nabla\times(\nabla f)=0\), \(\nabla\cdot(\nabla\times F)=0\) |

Important warning:

> Gradient, curl, and divergence are not three unrelated miracles. They are coordinate/metric-dependent shadows of the exterior derivative in different degrees.

---

## 12. Pullbacks

Let

$$
F:M\to N
$$

be a smooth map.

The derivative pushes tangent vectors forward:

$$
dF_p:T_pM\to T_{F(p)}N.
$$

Forms pull back in the opposite direction:

$$
F^*:\Omega^k(N)\to \Omega^k(M).
$$

If \(\omega\in\Omega^k(N)\), then \(F^*\omega\in\Omega^k(M)\).

This matters because integration on manifolds is usually computed by pulling a form back to a coordinate domain.

For a parametrization

$$
X:\Omega\subseteq \mathbb{R}^k\to M,
$$

the integral over \(X(\Omega)\) is computed as

$$
\int_{X(\Omega)}\omega
=
\int_\Omega X^*\omega.
$$

This is the clean intrinsic version of substitution/Jacobians.

---

## 13. Orientation and Boundary

An orientation on an \(n\)-manifold is a consistent choice of positive ordered basis in each tangent space.

For a surface in \(\mathbb{R}^3\), orientation is often represented by a choice of normal vector.

For an oriented manifold \(M\) with boundary, the boundary is denoted

$$
\partial M.
$$

The boundary inherits an orientation from \(M\).

Examples:

| \(M\) | \(\partial M\) |
|---|---|
| interval \([a,b]\) | endpoints \(b-a\) as oriented 0-boundary |
| disk \(D^2\) | circle \(S^1\) |
| solid ball \(B^3\) | sphere \(S^2\) |
| closed sphere \(S^2\) | empty boundary |

The slogan:

> Boundaries are one dimension lower.

If

$$
\dim M=k,
$$

then

$$
\dim \partial M=k-1.
$$

---

## 14. Integration on Manifolds

A \(k\)-form naturally integrates over an oriented \(k\)-dimensional manifold.

If

$$
\omega\in\Omega^k(M)
$$

and \(S\subseteq M\) is oriented \(k\)-dimensional, then

$$
\int_S \omega
$$

is defined.

Degree matching is essential:

$$
\text{\(k\)-forms integrate over \(k\)-dimensional oriented objects.}
$$

Examples:

| Object | Dimension | Integrand |
|---|---:|---|
| curve | 1 | 1-form |
| surface | 2 | 2-form |
| volume | 3 | 3-form |
| \(k\)-submanifold | \(k\) | \(k\)-form |

This is more structural than memorizing \(ds\), \(dA\), \(dV\).

Those are measure elements; forms also encode orientation.

---

## 15. Generalized Stokes Theorem

Let \(M\) be an oriented smooth \(k\)-manifold with boundary \(\partial M\). Let

$$
\omega\in\Omega^{k-1}(M).
$$

Then

$$
\boxed{
\int_M d\omega
=
\int_{\partial M}\omega
}
$$

This is the generalized Stokes theorem.

It says:

> The integral of a derivative over an interior equals the integral of the original quantity over the boundary.

But this does **not** mean the boundary contains all information about the interior. It means this specific derivative-type accumulation has a boundary representation.

### 15.1 Special cases

| Theorem | \(M\) | \(\partial M\) | Form degree |
|---|---|---|---|
| Fundamental theorem of calculus | interval | endpoints | 0-form |
| Fundamental theorem for line integrals | curve | endpoints | 0-form |
| Green's theorem | planar region | boundary curve | 1-form |
| Classical Stokes theorem | surface | boundary curve | 1-form |
| Divergence theorem | solid region | boundary surface | 2-form |

### 15.2 Why this matters for GMT

GMT generalizes the objects \(M\) and \(\partial M\).

Instead of requiring \(M\) to be a smooth manifold, GMT may work with currents, rectifiable sets, sets of finite perimeter, or varifolds.

The question becomes:

> Can we still define boundary, tangent planes, orientation, area, and integration when the object is not smooth?

---

## 16. Riemannian Metrics

A smooth manifold has calculus, but not automatically lengths or angles.

A Riemannian metric assigns an inner product to each tangent space:

$$
g_p:T_pM\times T_pM\to \mathbb{R}.
$$

For each \(p\in M\), \(g_p\) is symmetric, bilinear, and positive definite.

In local coordinates,

$$
g
=
g_{ij}\,dx^i\otimes dx^j.
$$

For tangent vectors

$$
v=v^i\frac{\partial}{\partial x^i},
\qquad
w=w^j\frac{\partial}{\partial x^j},
$$

their inner product is

$$
g(v,w)
=
g_{ij}v^iw^j.
$$

The length of \(v\) is

$$
\|v\|_g=\sqrt{g(v,v)}.
$$

The angle between nonzero \(v,w\) is determined by

$$
\cos \theta
=
\frac{g(v,w)}{\|v\|_g\|w\|_g}.
$$

A Riemannian manifold is a pair

$$
(M,g).
$$

### Metric as geometry generator

Once \(g\) exists, one can define:

$$
\text{length of curves},
\quad
\text{angle},
\quad
\text{area/volume},
\quad
\text{gradient},
\quad
\text{divergence},
\quad
\text{Laplacian},
\quad
\text{geodesics},
\quad
\text{curvature}.
$$

This is why metrics are central.

---

## 17. Gradients on Riemannian Manifolds

On \(\mathbb{R}^n\), we often identify \(df\) with \(\nabla f\). On a manifold, the more primitive object is

$$
df\in T_p^*M.
$$

The gradient is the vector field \(\nabla f\) satisfying

$$
g(\nabla f,X)=df(X)
$$

for every vector field \(X\).

So the metric converts the covector \(df\) into a vector \(\nabla f\).

Without \(g\), \(df\) still exists, but \(\nabla f\) does not canonically exist.

---

## 18. Covariant Derivatives

Ordinary derivatives compare vectors at nearby points. But on a curved manifold, tangent vectors at different points live in different vector spaces:

$$
T_pM \neq T_qM.
$$

A connection gives a rule for differentiating vector fields along directions.

The covariant derivative is written

$$
\nabla_XY.
$$

Here \(X\) is the direction of differentiation and \(Y\) is the vector field being differentiated.

In coordinates,

$$
\nabla_{\partial_i}\partial_j
=
\Gamma^k_{ij}\partial_k.
$$

The coefficients

$$
\Gamma^k_{ij}
$$

are the Christoffel symbols.

They are not tensors. They depend on coordinates. Their job is to correct ordinary partial derivatives so that differentiation of vector fields makes geometric sense on curved spaces.

### Levi-Civita connection

Given a Riemannian metric \(g\), there is a unique connection satisfying:

1. metric compatibility:

$$
X(g(Y,Z))
=
g(\nabla_XY,Z)+g(Y,\nabla_XZ),
$$

2. torsion-free condition:

$$
\nabla_XY-\nabla_YX=[X,Y].
$$

This connection is called the Levi-Civita connection.

---

## 19. Geodesics

A geodesic is a curve that locally moves as straight as the manifold allows.

Let

$$
\gamma:I\to M.
$$

Its velocity field is

$$
\dot{\gamma}(t)\in T_{\gamma(t)}M.
$$

The geodesic equation is

$$
\nabla_{\dot{\gamma}}\dot{\gamma}=0.
$$

In coordinates, this becomes

$$
\frac{d^2x^k}{dt^2}
+
\Gamma^k_{ij}
\frac{dx^i}{dt}
\frac{dx^j}{dt}
=
0.
$$

Intuition:

> A geodesic has zero intrinsic acceleration.

It may appear curved from the outside, but intrinsically it is not turning.

Examples:

| Space | Geodesics |
|---|---|
| \(\mathbb{R}^n\) | straight lines |
| sphere \(S^2\) | great circles |
| cylinder | helices and straight vertical/horizontal unwraps |

---

## 20. Curvature

Curvature measures failure of Euclidean behavior.

There are multiple curvature notions.

### 20.1 Curvature of a curve

For an arc-length parametrized curve

$$
\gamma(s),
$$

the unit tangent is

$$
T(s)=\gamma'(s).
$$

The curvature is

$$
\kappa(s)=\|T'(s)\|.
$$

This measures how quickly the tangent direction changes.

### 20.2 Gaussian curvature

For a surface, Gaussian curvature measures intrinsic bending.

Examples:

| Surface | Gaussian curvature |
|---|---:|
| plane | \(0\) |
| cylinder | \(0\) |
| sphere of radius \(R\) | \(1/R^2\) |
| saddle | negative |

The cylinder having zero Gaussian curvature is important: it bends extrinsically in \(\mathbb{R}^3\), but intrinsically it is locally like a plane.

### 20.3 Riemann curvature tensor

For a Riemannian manifold, curvature is encoded by

$$
R(X,Y)Z
=
\nabla_X\nabla_YZ
-
\nabla_Y\nabla_XZ
-
\nabla_{[X,Y]}Z.
$$

This measures the failure of covariant derivatives to commute.

Conceptually:

> Curvature measures path-dependence of parallel transport and noncommutativity of directional differentiation.

---

## 21. Laplacian and PDE Geometry

In Euclidean space,

$$
\Delta f
=
\sum_{i=1}^n \frac{\partial^2 f}{\partial (x^i)^2}.
$$

On a Riemannian manifold, the Laplace-Beltrami operator is

$$
\Delta f
=
\operatorname{div}(\nabla f).
$$

In coordinates,

$$
\Delta f
=
\frac{1}{\sqrt{|g|}}
\partial_i
\left(
\sqrt{|g|}\,g^{ij}\partial_j f
\right).
$$

Here:

$$
|g|=\det(g_{ij}),
$$

and

$$
(g^{ij})=(g_{ij})^{-1}.
$$

This is the geometric Laplacian.

### Heat equation

On a manifold, the heat equation becomes

$$
\frac{\partial u}{\partial t}
=
\Delta_g u.
$$

Here

$$
u:M\times[0,\infty)\to \mathbb{R}.
$$

The geometry of \(M\) affects diffusion through \(\Delta_g\).

### Wave equation

The wave equation becomes

$$
\frac{\partial^2 u}{\partial t^2}
=
\Delta_g u.
$$

This is the same conceptual operator, but the time behavior differs.

---

## 22. De Rham Cohomology

Because

$$
d^2=0,
$$

every exact form is closed.

Define:

$$
Z^k(M)=\ker(d:\Omega^k(M)\to\Omega^{k+1}(M)),
$$

the space of closed \(k\)-forms.

Define:

$$
B^k(M)=\operatorname{im}(d:\Omega^{k-1}(M)\to\Omega^k(M)),
$$

the space of exact \(k\)-forms.

Since \(d^2=0\),

$$
B^k(M)\subseteq Z^k(M).
$$

The \(k\)-th de Rham cohomology group is

$$
H^k_{\mathrm{dR}}(M)
=
Z^k(M)/B^k(M).
$$

Intuition:

> de Rham cohomology measures the obstruction to solving \(\omega=d\eta\) when \(d\omega=0\).

Or:

> It measures holes using differential forms.

Examples:

$$
H^1_{\mathrm{dR}}(S^1)\neq 0
$$

because the circle has a 1-dimensional hole.

$$
H^1_{\mathrm{dR}}(\mathbb{R}^n)=0
$$

because Euclidean space has no such hole.

This is the clean version of why topology affects conservative fields.

---

## 23. Gauss-Bonnet as Local-to-Global Geometry

For a compact oriented surface \(M\), Gauss-Bonnet says

$$
\int_M K\,dA
=
2\pi \chi(M)
$$

when \(M\) has no boundary.

Here:

$$
K=\text{Gaussian curvature},
$$

$$
dA=\text{area element},
$$

$$
\chi(M)=\text{Euler characteristic}.
$$

This is a prototype local-to-global theorem:

| Local quantity | Global/topological quantity |
|---|---|
| curvature density \(K\,dA\) | Euler characteristic \(\chi(M)\) |

This is the kind of theorem that explains why differential geometry is not just calculus. It uses calculus to detect topology.

---

## 24. Smoothness vs Measurability

Smooth differential geometry assumes a lot:

$$
C^\infty \text{ charts},
\quad
smooth transition maps,
\quad
smooth tangent spaces,
\quad
smooth vector fields,
\quad
smooth forms.
$$

GMT asks what happens when these assumptions are too strong.

A set might be:

- measurable but not smooth,
- rectifiable but not a manifold everywhere,
- smooth except for singularities,
- fractal-like,
- a weak limit of smooth manifolds,
- a minimizer of area with unavoidable singular points.

The core philosophical shift is:

> Smooth differential geometry starts with differentiability.  
> GMT starts with measure, approximation, and weak tangent structure.

This is analogous to the way Lebesgue integration extends the reach of Riemann-style integration: the measurable framework handles objects too irregular for the classical smooth/Riemann picture.

---

## 25. Hausdorff Measure

Lebesgue measure measures \(n\)-dimensional volume in \(\mathbb{R}^n\).

Hausdorff measure generalizes the idea of measuring \(k\)-dimensional size inside \(\mathbb{R}^n\).

The \(k\)-dimensional Hausdorff measure is denoted

$$
\mathcal{H}^k.
$$

Examples:

| Object in \(\mathbb{R}^3\) | Natural measure |
|---|---|
| curve | \(\mathcal{H}^1\) |
| surface | \(\mathcal{H}^2\) |
| solid region | \(\mathcal{H}^3\) |

This is the measure-theoretic replacement for length, area, and volume.

---

## 26. Rectifiable Sets

A set \(E\subseteq \mathbb{R}^n\) is countably \(k\)-rectifiable if, up to an \(\mathcal{H}^k\)-null set, it can be covered by countably many Lipschitz images of subsets of \(\mathbb{R}^k\).

Informally:

$$
E
\approx
\bigcup_{j=1}^{\infty} f_j(A_j)
$$

where

$$
A_j\subseteq \mathbb{R}^k,
\qquad
f_j:A_j\to \mathbb{R}^n
$$

are Lipschitz maps, modulo a set of \(\mathcal{H}^k\)-measure zero.

This means \(E\) may not be a smooth manifold, but it is still made out of countably many \(k\)-dimensional pieces in a measure-theoretic sense.

### Smooth manifold vs rectifiable set

| Smooth manifold | Rectifiable set |
|---|---|
| smooth charts | Lipschitz parametrizations |
| tangent space everywhere | approximate tangent plane almost everywhere |
| smooth area form | \(\mathcal{H}^k\)-measure |
| classical boundary | measure-theoretic boundary or current boundary |
| pointwise differential geometry | almost-everywhere geometry |

This is one of the first serious bridges from differential geometry to GMT.

---

## 27. Approximate Tangent Planes

A smooth \(k\)-manifold has a tangent space

$$
T_pM
$$

at every point.

A rectifiable set may only have an approximate tangent plane at \(\mathcal{H}^k\)-almost every point.

This is written informally as

$$
T_pE
\quad
\text{exists for }\mathcal{H}^k\text{-a.e. }p\in E.
$$

The phrase "almost everywhere" is essential. GMT often replaces everywhere-smooth structure with almost-everywhere structure.

This is the measure-theoretic version of the tangent-space idea.

---

## 28. Currents

A smooth oriented \(k\)-dimensional submanifold \(M\) defines a functional on compactly supported smooth \(k\)-forms:

$$
T_M(\omega)
=
\int_M \omega.
$$

A \(k\)-current generalizes this idea.

A current is not primarily a set. It is an integration functional.

Symbolically,

$$
T:\Omega_c^k(\mathbb{R}^n)\to \mathbb{R}.
$$

The boundary of a current is defined by duality with exterior derivative:

$$
\partial T(\omega)
=
T(d\omega).
$$

This is extremely important.

It means generalized Stokes becomes almost definitional:

$$
\partial T(\omega)=T(d\omega).
$$

For a smooth manifold current \(T_M\),

$$
\partial T_M = T_{\partial M}.
$$

So currents are a natural GMT object because they retain:

$$
\text{orientation}
+
\text{multiplicity}
+
\text{boundary}
+
\text{integration}
$$

without requiring the object to be a smooth manifold.

---

## 29. Varifolds

Varifolds are another weak generalization of surfaces.

Where currents care about orientation, varifolds are more measure-like and do not require orientation.

A \(k\)-varifold in \(\mathbb{R}^n\) can be thought of as a measure on pairs

$$
(x,P),
$$

where

$$
x\in \mathbb{R}^n
$$

and

$$
P
$$

is a \(k\)-dimensional plane through the origin, representing a tangent plane direction.

Informally:

$$
\text{varifold} = \text{distribution of mass + tangent planes}.
$$

Varifolds are useful for studying weak limits of surfaces, especially in minimal surface theory.

---

## 30. Sets of Finite Perimeter

A set

$$
E\subseteq \mathbb{R}^n
$$

has finite perimeter if its indicator function

$$
\mathbf{1}_E
$$

has distributional derivative given by a finite Radon measure.

This moves boundary theory into weak derivatives.

The boundary is no longer just the topological boundary. GMT distinguishes:

| Boundary type | Meaning |
|---|---|
| topological boundary | closure minus interior |
| measure-theoretic boundary | density neither 0 nor 1 |
| reduced boundary | points with measure-theoretic normal |
| current boundary | boundary defined by action on forms |

This matters because the topological boundary can be too wild, while the measure-theoretic boundary captures the geometry relevant to perimeter and variational problems.

---

## 31. Area and Coarea Formulae

The classical change-of-variables theorem uses the determinant of the Jacobian.

GMT generalizes this through area and coarea formulae.

For a Lipschitz map

$$
f:\mathbb{R}^m\to\mathbb{R}^n,
$$

one can integrate over images and level sets using generalized Jacobians.

The philosophical point:

> Jacobians survive beyond smooth maps, but they become almost-everywhere and measure-theoretic objects.

This is crucial for probability, statistics, and machine learning because many useful maps are not smooth everywhere but are still Lipschitz, measurable, or differentiable almost everywhere.

---

## 32. The Smooth-to-GMT Dictionary

| Smooth differential geometry | GMT / weak geometry |
|---|---|
| smooth manifold | rectifiable set / current / varifold |
| tangent space \(T_pM\) | approximate tangent plane a.e. |
| smooth chart | Lipschitz parametrization |
| smooth boundary \(\partial M\) | current boundary / reduced boundary |
| area form \(dA\) | Hausdorff measure \(\mathcal{H}^k\) |
| vector field | test field / weak vector field |
| differential form | test form |
| Stokes theorem | boundary of current defined by \(T(d\omega)\) |
| curvature tensor | weak curvature / first variation |
| minimal surface | area-minimizing current / stationary varifold |

---

## 33. What to Learn Next

### Phase 1: Smooth foundations

Focus on:

1. smooth manifolds,
2. tangent and cotangent spaces,
3. vector fields,
4. differential forms,
5. exterior derivative,
6. integration on manifolds,
7. generalized Stokes.

Goal:

$$
\int_{\partial M}\omega=\int_M d\omega
$$

should feel like the master theorem behind vector calculus.

### Phase 2: Riemannian geometry

Focus on:

1. metrics,
2. gradients/divergence/Laplacian,
3. covariant derivatives,
4. geodesics,
5. curvature,
6. volume forms,
7. Gauss-Bonnet.

Goal:

Understand how geometry is encoded by \(g\) and its derivative structure.

### Phase 3: Topology interface

Focus on:

1. fundamental group,
2. homology,
3. cohomology,
4. de Rham cohomology,
5. degree theory,
6. compactness and orientability.

Goal:

Understand why calculus detects holes and global structure.

### Phase 4: Measure-theoretic geometry

Focus on:

1. measure theory,
2. Hausdorff measure,
3. Lipschitz maps,
4. rectifiability,
5. area/coarea formulae,
6. BV functions,
7. finite-perimeter sets.

Goal:

Replace everywhere-smooth structure with almost-everywhere structure.

### Phase 5: GMT objects

Focus on:

1. currents,
2. integral currents,
3. varifolds,
4. first variation,
5. monotonicity formulae,
6. minimal surfaces,
7. regularity and singularities.

Goal:

Understand geometric objects as weak limits, measures, and integration functionals.

---

## 34. Core Intuitions to Keep

### 34.1 A manifold is a space where calculus works locally

The phrase "locally Euclidean" means:

$$
\text{near each point, the space can be coordinatized by } \mathbb{R}^n.
$$

### 34.2 Tangent spaces are local linearizations of the space

At each point \(p\),

$$
T_pM
$$

is the best linear approximation to \(M\) at \(p\).

### 34.3 Forms are the natural objects of integration

A \(k\)-form integrates over a \(k\)-dimensional oriented object.

This replaces ad hoc vector-calculus formulas with one degree-matching rule.

### 34.4 Stokes is the master theorem

$$
\int_M d\omega
=
\int_{\partial M}\omega.
$$

It unifies FTC, Green, classical Stokes, and divergence theorem.

### 34.5 The metric is extra structure

A smooth manifold does not automatically know length or angle.

A metric adds those.

### 34.6 Curvature is noncommutativity of local linear comparison

Curvature appears when moving vectors around depends on the path.

### 34.7 GMT keeps geometry after smoothness breaks

GMT asks for the weakest structure needed to preserve length, area, boundary, tangent planes, and variational calculus.

---

## 35. Recommended Repo Structure

A clean repo structure could be:

```text
GMT-Topology/
├── README.md
├── notes/
│   ├── 00_differential_geometry_foundations.md
│   ├── 01_manifolds_and_tangent_spaces.md
│   ├── 02_differential_forms_and_stokes.md
│   ├── 03_riemannian_metrics_and_curvature.md
│   ├── 04_topology_and_de_rham_cohomology.md
│   ├── 05_measure_theory_bridge.md
│   ├── 06_rectifiability.md
│   ├── 07_currents.md
│   └── 08_varifolds.md
├── examples/
│   ├── curves_surfaces_forms.md
│   ├── stokes_special_cases.md
│   └── smooth_vs_rectifiable_examples.md
└── references/
    └── bibliography.md
```

This current file can be placed as:

```text
notes/00_differential_geometry_foundations.md
```

---

## 36. Minimal Problem Set for Self-Calibration

These are not computational busywork. They are concept checks.

### Problem 1

Let

$$
\gamma(t)=(\cos t,\sin t),
\qquad
t\in[0,2\pi].
$$

Compute:

$$
\gamma'(t),
\qquad
\|\gamma'(t)\|,
\qquad
L(\gamma).
$$

Interpret this as integrating a 1-dimensional measure over a curved object.

### Problem 2

Let

$$
F(x,y)=(-y,x).
$$

Compute

$$
\int_{S^1}F\cdot d\mathbf{r}.
$$

Then write the corresponding 1-form

$$
\omega=-y\,dx+x\,dy.
$$

Compute \(d\omega\). Interpret using Green's theorem.

### Problem 3

Let

$$
f(x,y,z)=x^2+y^2+z^2.
$$

Compute

$$
df.
$$

Then use the Euclidean metric to identify \(df\) with \(\nabla f\).

Explain why \(df\) is more primitive than \(\nabla f\).

### Problem 4

Let \(S^2\subseteq\mathbb{R}^3\). Explain why \(S^2\) is a 2-dimensional manifold, even though it lives in 3-dimensional space.

What is

$$
T_pS^2
$$

for a point \(p\in S^2\)?

### Problem 5

State generalized Stokes:

$$
\int_M d\omega=\int_{\partial M}\omega.
$$

Then identify \(M\), \(\partial M\), and \(\omega\) for:

1. FTC,
2. Green's theorem,
3. classical Stokes theorem,
4. divergence theorem.

### Problem 6

Give an example of a space that is not smooth everywhere but should still have a reasonable notion of length or area.

Explain why this motivates Hausdorff measure or rectifiability.

---

## 37. Reference Map

### Smooth manifolds and forms

- John M. Lee, *Introduction to Smooth Manifolds*, 2nd ed.
- Loring W. Tu, *An Introduction to Manifolds*.
- MIT OpenCourseWare, *Geometry of Manifolds*.

### Curves, surfaces, and classical differential geometry

- Theodore Shifrin, *Differential Geometry: A First Course in Curves and Surfaces*.
- Manfredo do Carmo, *Differential Geometry of Curves and Surfaces*.
- Barrett O'Neill, *Elementary Differential Geometry*.

### Riemannian geometry

- John M. Lee, *Introduction to Riemannian Manifolds*.
- do Carmo, *Riemannian Geometry*.

### GMT bridge

- Leon Simon, *Introduction to Geometric Measure Theory* / *Lectures on Geometric Measure Theory*.
- Lawrence C. Evans and Ronald F. Gariepy, *Measure Theory and Fine Properties of Functions*.
- Francesco Maggi, *Sets of Finite Perimeter and Geometric Variational Problems*.
- Herbert Federer, *Geometric Measure Theory*.

---

## 38. One-Sentence Summary

Differential geometry builds calculus on smooth spaces using tangent spaces, forms, metrics, and curvature; GMT keeps the geometric-integration machinery while weakening smoothness into measure, rectifiability, currents, varifolds, and almost-everywhere tangent structure.
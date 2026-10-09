Familiar notation	Measure-theoretic interpretation
\(P(X=a)\)	\(P(X^{-1}(\{a\}))\)
\(P(X\leq a)\)	\(P(X^{-1}((-\infty,a]))\)
\(P(a<X\leq b)\)	\(P(X^{-1}((a,b]))\)
\(P(X\in A)\)	\(P(X^{-1}(A))=P_X(A)\)
\(P(X\leq a,Y\leq b)\)	\(P(X^{-1}((-\infty,a])\cap Y^{-1}((-\infty,b]))\)
\(P(X+Y\leq a)\)	\(P((X+Y)^{-1}((-\infty,a]))\)
\(P(X=a\mid Y=b)\)	\(\dfrac{P(\{X=a\}\cap\{Y=b\})}{P(\{Y=b\})}\), if denominator is positive


 

# PSTAT 210 — Advanced Homework Set

Measure-Theoretic Probability · High-difficulty graduate problem set

Difficulty

## 8–10 / 10

Problems

# 6

Suggested time

# 8–12 hours

Prerequisites: measure spaces, measurable functions, integration, modes of convergence, and basic \\(L^p\\) spaces. Some problems introduce new theorems you may need to establish.

## Problem 1 — The exact relationship between convergence modes (15 points)

Let \\((\Omega,\mathcal F,P)\\) be a probability space and let \\(X_n,X\\) be real-valued measurable functions.

Recall that:

- \\(X_n\to X\\) almost surely if \\(P(X_n\to X)=1\\).
- \\(X_n\to X\\) in probability if, for every \\(\varepsilon>0\\),

\\[ P(|X_n-X|>\varepsilon)\longrightarrow 0. \\]

\- \\(X_n\to X\\) in \\(L^p\\), for \\(p\ge1\\), if

\\[ \mathbb E[|X_n-X|^p]\longrightarrow0. \\]

(a) Prove that \\(L^p\\) convergence implies convergence in probability. Identify precisely where the assumption \\(p\ge1\\) is used, if at all.

(b) Prove that almost-sure convergence implies convergence in probability on a probability space.

(c) Construct a sequence \\(X_n\\) such that \\(X_n\to0\\) in probability but \\(X_n\not\to0\\) almost surely.

Use \\(\Omega=[0,1]\\) with Lebesgue probability measure. You must explicitly define the sequence and calculate the relevant probabilities.

(d) Construct a sequence \\(X_n\to0\\) almost surely for which \\(X_n\\) does not converge to zero in \\(L^1\\).

(e) Are the implications in (a) and (b) reversible? Justify each answer by proof or counterexample.

Challenge: Can you find necessary and sufficient additional conditions under which convergence in probability implies \\(L^1\\) convergence? This leads directly to Problem 2.

## Problem 2 — Uniform integrability and the Vitali convergence theorem (20 points)

Let \\((\Omega,\mathcal F,P)\\) be a probability space and let \\(\\{X_n\\}\_{n\ge1}\subset L^1(P)\\).

The family is called uniformly integrable if

\\[ \lim\_{K\to\infty} \sup\_{n\ge1} \mathbb E\left[ |X_n|\mathbf 1\_{\\{|X_n|>K\\}} \right]=0. \\]

(a) Prove that if there exists \\(p>1\\) such that

\\[ \sup\_{n\ge1}\mathbb E[|X_n|^p]<\infty, \\]

then \\(\\{X_n\\}\\) is uniformly integrable.

(b) Show that boundedness in \\(L^1\\) alone does not imply uniform integrability.

Construct an explicit sequence on \\(([0,1],\mathcal B([0,1]),\lambda)\\).

(c) Suppose \\(X_n\to X\\) in probability and \\(\\{X_n\\}\\) is uniformly integrable.

Prove that \\(X\in L^1\\) and

\\[ \mathbb E[|X_n-X|]\longrightarrow0. \\]

You may use Fatou's lemma, but you must prove the main convergence claim rather than merely cite it.

(d) Prove the converse: if \\(X_n\to X\\) in \\(L^1\\), then \\(\\{X_n\\}\\) is uniformly integrable.

(e) State and prove a precise version of the Vitali convergence theorem, identifying the role of convergence in probability and uniform integrability.

Why this is hard: The difficulty isn't calculating an expectation. It's controlling an entire sequence of functions whose mass can escape into increasingly rare events.

## Problem 3 — Fatou's lemma, dominated convergence, and failure of hypotheses (15 points)

Let \\((\Omega,\mathcal F,\mu)\\) be an arbitrary measure space. Unlike a probability space, \\(\mu(\Omega)\\) need not equal one or even be finite.

(a) Prove Fatou's lemma for a sequence of nonnegative measurable functions \\(f_n\\):

\\[ \int\_\Omega\liminf\_{n\to\infty} f_n\\,d\mu \le \liminf\_{n\to\infty}\int\_\Omega f_n\\,d\mu. \\]

You may use the monotone convergence theorem, but not Fatou's lemma itself.

(b) Construct a sequence of nonnegative measurable functions for which the inequality in Fatou's lemma is strict.

(c) Let \\(f_n\to f\\) almost everywhere and suppose there exists \\(g\in L^1(\mu)\\) with

\\[ |f_n|\le g\quad\mu\text{-almost everywhere} \\]

for every \\(n\\). Prove the dominated convergence theorem:

\\[ \lim\_{n\to\infty}\int f_n\\,d\mu = \int f\\,d\mu. \\]

Derive the result using Fatou's lemma.

(d) Construct an example where \\(f_n\to0\\) almost everywhere but

\\[ \lim\_{n\to\infty}\int f_n\\,d\mu\ne0. \\]

Explain precisely which hypothesis of dominated convergence fails.

(e) Suppose \\(f_n\to f\\) almost everywhere, \\(f_n\ge0\\), and \\(\int f_n\\,d\mu\to\int f\\,d\mu<\infty\\). Does this imply \\(L^1\\) convergence? Prove your answer.

## Problem 4 — Radon–Nikodym derivatives and change of measure (20 points)

Let \\((\Omega,\mathcal F)\\) be a measurable space, and let \\(\mu,\nu\\) be finite measures on it.

Recall that \\(\nu\ll\mu\\) means

\\[ \mu(A)=0\implies\nu(A)=0 \qquad\text{for all }A\in\mathcal F. \\]

The Radon–Nikodym theorem states that if \\(\nu\ll\mu\\), then there exists a nonnegative measurable function \\(f\\) such that

\\[ \nu(A)=\int_A f\\,d\mu \qquad\text{for all }A\in\mathcal F. \\]

The function \\(f\\) is denoted \\(d\nu/d\mu\\).

(a) Prove uniqueness almost everywhere: if \\(f,g\ge0\\) both satisfy

\\[ \nu(A)=\int_A f\\,d\mu =\int_A g\\,d\mu \\]

for every measurable \\(A\\), then \\(f=g\\) \\(\mu\\)-almost everywhere.

(b) Let \\(P\\) and \\(Q\\) be probability measures with \\(Q\ll P\\), and define \\(L=dQ/dP\\). Prove that

\\[ \mathbb E_P[L]=1. \\]

(c) For any nonnegative measurable \\(X\\), prove the change-of-measure identity

\\[ \mathbb E_Q[X]=\mathbb E_P[LX]. \\]

Explain how the identity extends to integrable signed random variables.

(d) Suppose \\(Q\ll P\\), with density \\(L\\). Prove that \\(Q\\) and \\(P\\) are mutually absolutely continuous if and only if \\(L>0\\) \\(P\\)-almost surely.

(e) Consider two probability densities \\(p,q\\) with respect to Lebesgue measure. Assuming \\(q>0\\) wherever \\(p>0\\), express the likelihood ratio and explain how it transforms expectations under one distribution into expectations under the other.

Challenge: Which parts of the argument require \\(Q\ll P\\), and what breaks when this assumption fails?

## Problem 5 — Product measures, Tonelli, and Fubini (15 points)

Let \\((S,\mathcal S,\mu)\\) and \\((T,\mathcal T,\nu)\\) be sigma-finite measure spaces.

Let \\(\mu\otimes\nu\\) denote the product measure on \\((S\times T,\mathcal S\otimes\mathcal T)\\).

(a) For a nonnegative measurable function \\(f:S\times T\to[0,\infty]\\), prove Tonelli's theorem:

\\[ \int\_{S\times T}f\\,d(\mu\otimes\nu) = \int_S\left(\int_T f(s,t)\\,d\nu(t)\right)d\mu(s). \\]

You may invoke the standard theorem for indicator functions of measurable rectangles and the monotone convergence theorem.

(b) Explain why sigma-finiteness is important to the standard product-measure construction. Identify where it enters the relevant theorem.

(c) If \\(f\in L^1(\mu\otimes\nu)\\), prove Fubini's theorem and show that the iterated integrals exist almost everywhere and agree with the product-space integral.

(d) Construct an example of a nonnegative measurable function on \\(\mathbb R^2\\) for which the two iterated integrals are unequal or one cannot be interpreted as an ordinary finite integral. Explain why Tonelli's theorem is not contradicted.

Important distinction: For nonnegative functions, Tonelli permits infinite integrals. Fubini's theorem requires integrability to conclude that the iterated integrals are finite almost everywhere and agree as finite values.

## Problem 6 — Conditional expectation as a Radon–Nikodym derivative (15 points)

Let \\((\Omega,\mathcal F,P)\\) be a probability space, let \\(\mathcal G\subseteq\mathcal F\\) be a sub-sigma-algebra, and let \\(X\in L^1(P)\\).

The conditional expectation \\(\mathbb E[X\mid\mathcal G]\\) is a \\(\mathcal G\\)-measurable integrable random variable \\(Y\\) satisfying

\\[ \int_A Y\\,dP=\int_A X\\,dP \qquad\text{for all }A\in\mathcal G. \\]

(a) Using the Radon–Nikodym theorem, construct \\(\mathbb E[X\mid\mathcal G]\\) when \\(X\ge0\\).

Hint: Define a measure on \\((\Omega,\mathcal G)\\) by

\\[ \nu(A)=\int_A X\\,dP. \\]

(b) Extend the construction to arbitrary \\(X\in L^1(P)\\), and prove uniqueness up to almost-sure equality.

(c) Prove the tower property. If \\(\mathcal H\subseteq\mathcal G\subseteq\mathcal F\\), show that

\\[ \mathbb E[\mathbb E[X\mid\mathcal G]\mid\mathcal H] = \mathbb E[X\mid\mathcal H] \quad\text{a.s.} \\]

(d) Prove that if \\(X\in L^1\\) is independent of \\(\mathcal G\\), then

\\[ \mathbb E[X\mid\mathcal G]=\mathbb E[X] \quad\text{a.s.} \\]

(e) Suppose \\(X\in L^2(P)\\). Prove that \\(\mathbb E[X\mid\mathcal G]\in L^2(P)\\) and that

\\[ \mathbb E\left[ \left(X-\mathbb E[X\mid\mathcal G]\right)Z \right]=0 \\]

for every \\(Z\in L^2(\Omega,\mathcal G,P)\\).

Interpret this as an orthogonality relation in \\(L^2\\).

Why this belongs near the top of the set: This problem connects measure theory, probability, Radon–Nikodym derivatives, and Hilbert-space geometry in a single construction.&#x20;

## How to use this set

I'd rank the problems by conceptual difficulty as follows:

| Rank | Problem | Main obstacle                                                  |
| ---- | ------- | -------------------------------------------------------------- |
| 1    | 1       | Constructing examples that separate convergence modes          |
| 2    | 3       | Deriving convergence theorems from first principles            |
| 3    | 2       | Controlling tail behavior uniformly across a sequence          |
| 4    | 5       | Justifying interchange of integrals                            |
| 5    | 4       | Understanding measure changes and almost-everywhere uniqueness |
| 6    | 6       | Building conditional expectation from a measure derivative     |

The ordering is approximate: Problem 6 is especially demanding because it combines several areas of mathematics.

My suggested starting point is Problem 2(b)–(c). These force you to understand why convergence of random variables and convergence of their expectations are genuinely different things, and what additional structure bridges the gap.

If you want something even more theoretical after this, the next tier would be a problem set centered on martingale convergence, uniform integrability of martingales, \\(L^p\\) duality, weak convergence of probability measures, and the construction of conditional expectation as an \\(L^2\\) projection. Those topics push toward the more advanced end of graduate probability, with the exact placement depending on the course syllabus.

# Note on Analysis

## Motivation & preliminaries

### Sets and functions

* intersection: $A \cap B$
* union: $A \cup B$
* complement: $A^c = \Omega \setminus A$
* difference: $B \setminus A = \{x \in B | x \notin A\} = B \cap A^c$
* symmetric difference: $A \Delta B = (A \setminus B) \cup (B \setminus A)$
  * if $A \Delta B = \emptyset$ iff. $A=B$
* Extension to collections
  * $\cap_{\alpha=\Lambda} A_\alpha = \{x| \forall \alpha \in \Lambda, \enspace x \in A_\alpha \}$
  * $\cup_{\alpha=\Lambda} A_\alpha = \{x| \exists \alpha \in \Lambda, \enspace x \in A_\alpha \}$
* de Morgan's laws
  * $(\cup_\alpha A_\alpha)^c = \cap_\alpha A^c_\alpha$
  * $(\cap_\alpha A_\alpha)^c = \cup_\alpha A^c_\alpha$
  * if $A \cap B = \emptyset$, $A$ and $B$ are disjoint
  * A family of sets $(A_\alpha)_{\alpha \in \Lambda}$ is **pairwise disjoint**, if $A_\alpha \cap A_\beta = \emptyset$, whenever $\alpha \neq \beta$ ($\alpha, \beta \in \Lambda$)
* cartesian product $A \times B$ is the set of ordered pairs
  * $A \times B = \{ (a,b)| a \in A, b \in B \}$
  * $\N$: Naturals
  * $\Z$: Integers
  * $\mathbb{Q}$: Rationals
  * $\R$: Reals
* interval $[a, b) = \{ x \in \R | a \leq x < b \}$
  * $+\infty$, $-\infty$ for unbounded interval
  * $(-\infty, b) = \{ x \in \R | x < b\}$
  * $[0, \infty) = \{ x \in \R | x \geq 0\} = \R^+$
* $\R^n$ is the n-fold cartesian product of $\R$
* a function $f: A \rightarrow B$ is a subset of $A \times B$
  * $(a,b), (a,c) \in f \implies b = c$
  * domain $D_f = \{ a \in A | \exists b \in B, (a,b) \in f \}$
  * range $R_f = \{ b \in B | \exists a \in A, (a,b) \in f \}$
* The set $X \subset A$ has an image $f(X) = \{ b \in B | b = f(a) \enspace \exists a \in X \}$
* The inverse image of a set $Y \subset B$ is $f^{-1}(Y) = \{ a \in A| f(a) \in Y\}$
* composition $h = f_2 \circ f_1$
  * $f_1: A \rightarrow B$, $f_2: B \rightarrow C$
  * $h: A \rightarrow C$
  * $h(a) = f_2(f_1(a))$
* The function $g$ extends $f$ if $D_f \subset D_g$ and $g=f$ on $D_f$
  * (alternative) $f$ restricts $g$ to $D_f$
* The indicator function $\mathbf{1}_A$ of the set $A$ is the function
  * $\mathbf{1}_A(x) = \begin{cases} 1 & x \in A \\ 0 & x \notin A \end{cases}$
    * $\mathbf{1}_{A \cap B} = \mathbf{1}_A \cdot \mathbf{1}_B$
    * $\mathbf{1}_{A \cup B} = \mathbf{1}_A + \mathbf{1}_B - \mathbf{1}_A\mathbf{1}_B$
    * $\mathbf{1}^c = 1 - \mathbf{1}_A$

* Equivalence relation on E (any set)
  * a subset $R$ of $E \times E$, where $x \sim y$, $(x,y) \in R$
    1. reflexive: $\forall x \in E$, $x \sim x$
    1. symmetric: $x \sim y \implies y \sim x$
    1. transitive: $x \sim y, y \sim z \implies x \sim z$
  * an equivalence relation $\sim$ on $E$ partitions $E$ into disjoint equivalence classes;
    * given $x \in E$, $[x] = \{z| z \sim x \}$ for equivalence class of $x$
    * if $x \in [x]$, $E = \cup_{x \in E} [x]$ (disjoint union)
    * if $[x] \cap [y] \neq \emptyset$ then there is $z \in E$ which $x \sim z$, $z\sim y$ $\implies$ $x \sim y$ so that $[x] = [y]$.

### Countable and uncountable sets in $\R$

* $A$ is **countable** if there is a one to one correspondence between $A$ and a subset of $\N$
  * $f:A \rightarrow \N$ that takes a distinct point to a distinct point
* $A$ is **finite** if this correspondence can be set up using only an initial segment $\{1, 2, ..., N\}$ of $\N$ for some $N \in \N$
* $A$ is **countable infinite**  or **denumerable** if all of $\N$ is used
  * counterable unions of countable sets are countable
  * $\mathbb{Q}$ of rationals is countable
  * Contor showed that the set $\R$ cannot be placed in one-to-one correspondence with $\N$ i.e. uncountable set
* If a set is countable, we could write its elements as a sequence $(x_n)_{n\geq1}$
* Union of two countable sets $\rightarrow$ countable
  * $\mathbb{Q}$ is countable, $\R$ is uncountable $\rightarrow$ $\R \setminus \mathbb{Q}$ is uncountable

### Topological properties of sets in $\R$

* **open set** $O \subset R$
  * A subset $O$ of the real line $\R$ is open if it is a union of open intervals,
    * $(I_\alpha)_{\alpha \in \Lambda}$ where $\Lambda$ is some index set (countable or not) $O = \cup_{\alpha \in \Lambda} I_\alpha$
* a set is **closed** if its complement is open
* open sets in $\R^n$ can be defined as unions of n-fold products of open invervals
* If $\Lambda$ is an index set and $I_\alpha$ is an open interval $\forall \alpha \in \Lambda$, then there exists a countable collection $(I_{\alpha_k})_{k\geq1}$ of these intervals whose union equals $\cup_{\alpha \in \Lambda} I_\alpha$
* finite intersection of open sets is open
* a countable intersection of open sets need not to be open
  * let $O_n = (\frac{1}{n}, 1)$ for $n\geq1$ then $E = \cap_{n=1}^\infty = [0, 1)$ is not open
* $\R$, (unlike $\R^n$ or more general spaces), has a **linear order**, i.e. given $x,y \in \R$ we can decide whether $x \leq y$ or $y \leq x$
* $u$ is an **upper bound** for a set $A \subset \R$ if $a \leq u$ for $\forall a \in A$, **lower bound** is defined similarly.
* The **supremum** $\sup A$ (or least upper bound) is then the minimum of all upper bounds.
* The **infimum** $\inf A$ (or greatest lower bound) is defined as the maximum of all lower bounds.
* The completeness property of $\R$ can be expressed by the statement that every set which is bounded above has a supremum.
* a real function $f$ is said to be **continuous** if $f^{-1}(O)$ is open for each open set $O$
* every contionuous real function defined on a **closed bounded set** attains its bounds on such a set, i.e. has a minimum and maximum
  * if continuous, closed $\rightarrow$ closed, open $\rightarrow$ open
  * if $f:[a, b] \rightarrow \R$ is continuous,
    * $M = \sup \{ f(x)| x \in [a, b]\} = f(x_{max})$
    * $m = \inf \{ f(x)| x \in [a, b]\} = f(x_{min})$
    * for some points $x_{max},x_{min} \in [a, b]$

| **The intermediate value theorem** |
| --- |
|a continuous function takes all intermediate values between the extreme ones, i.e. for each $y \in [m, M]$ there is a $\theta \in [a, b]$ such that $y = f(\theta)$|

* **the upper limit** $\lim \sup_n x_n = \inf \{ \sup_{m \geq n} x_m, n \in \N\}$
* **the lower limit** $\lim \inf_n x_n = \sup \{ \inf_{m \geq n} x_m, n \in \N\}$
* The sequence $x_n$ converges iff. these quantities coincide and their common value is then its limit.
  * $x = \lim_{n \rightarrow \infty} x_n$, if $\forall \epsilon > 0$, there is an $N \in \N$ such that $| x_n - x| < \epsilon$ whenever $n \geq N$
* a series $\sum_{n\geq1} a_n$ converges, if the sequence $x_m = \sum_{n=1}^m a_n$ of its partial sum converges, its limit $\sum_{n=1}^\infty a_n$ of the series.

### The Riemann integral: scope & limitations

Why it does not suffice for more advanced application?

* Let $f:[a,b] \rightarrow \R$ be a bounded real function, where $a$, $b$ with $a<b$, are real numbers.
  * A partition of $[a, b]$ is a finite set $P=\{a_0, a_1, ..., a_n\}$ with $a = a_0 < a_1 < a_2 < ... < a_n = b$
  * The partition $P$ gives rise to the upper and lower Riemann sums.
    * $U(P, f) = \sum^n_{i=1} M_i \Delta a_i$
    * $L(P, f) = \sum^n_{i=1} m_i \Delta a_i$
      * where $\Delta a_i = a_i - a_{i-1}$
      * $M_i = \sup_{a_{i-1}\leq x \leq a_i}f(x)$
      * $m_i = \inf_{a_{i-1}\leq x \leq a_i}f(x)$
    * Note that $M_i$ and $m_i$ are well defined real numbers since $f$ is bounded on each interval $[a_{i-1}, a_i]$
* In order to define the Riemann integral of $f$, one first shows that for any given partition $P$, $L(P,f) \leq U(P,f)$
* for any refinement, i.e. a partition $P' \supset P$, we must have
  * $L(P',f) \leq L(P, f)$
  * $U(P,f) \leq U(P', f)$
* The set $\{L(P, f): P \text{ is a partition of }[a,b] \}$ is bounded above in $\R$
  * $\sup L(P, f) = \underline{\int^b_a} f$ on $[a,b]$
  * $\inf U(P, f) = \overline{\int^b_a} f$ on $[a,b]$
* a function $f$ is said to be Riemann-integrable on $[a,b]$ if these two numbers coincide and their common value is the Riemann integral of $f$, more commonly $\int^b_a f(x) dx$

| **Theorem 1.2 - Riemann's Criterion** |
| --- |
| $f:[a,b] \rightarrow \R$ is Riemann-integrable iff. for every $\epsilon > 0$ there exists a partition $P_\epsilon$ such that $U(P_\epsilon, f) - L(P_\epsilon, f) < \epsilon$ |

* any bounded monotone function belongs to Riemann-integrable.
* any continuous function $f:[a,b] \rightarrow \R$ will be Riemann-integrable.

| **Theorem 1.4 - fundamental theorem of calculus** |
| --- |
| $f:[a,b] \rightarrow \R$ is continuous and the function $F:[a,b] \rightarrow \R$ has derivative $f$ (i.e. $F'=f$ on $[a,b]$|
| $F(b) - F(a) = \int^b_a f(x) dx$ |
| $F(x) = \int^x_{-a} f(x) dx$ up to a constant|

* We can relax the continuity requirement
  * **trivial step**: assume $f$ bounded and continuous on $[a,b]$ except at *finitely* many points
    * $f$ is Riemann-integrable
  * **taking this further**: (require Lesbesgue theory)
    * $f$ is Riemann-integrable iff. it is continous at *almost all* points of $[a,b]$
* **Dirichlet function**
  * $f(x) = \begin{cases} 1 & \text{if } x \in \mathbb{Q} \\ 0 & \text{if } x \notin \mathbb{Q}  \end{cases}$
  * $f$ is continuous at irrational and discontinuous at rational $\implies$ continuous *almost all*

What is wrong with the simple Riemann integral?

1. it doesn't deal with all the kinds of function that we hope to handle.
    * integrals over unbounded intervals $\int^\infty_{-\infty} e^{-x^2} dx$
    * integrals of an unbounded function $\int^1_0 \frac{1}{\sqrt x} dx$
    * we have to resort to *improper* Riemann integrals $\int^n_{-n} e^{-x^2} dx$, $\int^1_\epsilon \frac{1}{\sqrt x} dx$
    * this isn't all that serious a flaw
2. dependence on intervals - no easy ways of integrating over more general sets.
    * upper and lower sums for $\mathbf{1}_\mathbb{Q}$ of $\mathbb{Q}$ over $[0,1]$
    * sub intervals of partition $[0,1]$  must contain both rational and irrational points
    * each upper sum 1, lower sum 0 $\implies$ no convergence (**too discontinuous**)
3. lack of completeness
    * Riemann integral doesn't interact well with taking the limit of a sequence of functions
    * if a sequence $(f_n)$ of Riemann-integrable functions converges to $f$, then $\int^b_a f_n dx \rightarrow \int^b_a f dx$ is not always satisfied!
    * ex1) the limit need not be Riemann-integrable, and so the convergence question does not even make sense.
      * $f=\mathbf{1}_\mathbb{Q}$, $f_n = \mathbf{1}_{A_n}, A_n=\{q_1, ... ,q_n \}$ the sequence $(q_n)$, $n \geq 1$ is a enumeration of the rationals, so that $(f_n)$ is even monotone increasing.
    * ex2) the limit is Riemann-integrable, but the convergence of Riemann integrals does not hold.
      * $[a,b] = [0,1]$, $f_n(x) = \begin{cases} 4n^2 x & 0 \leq x < \frac{1}{2n} \\ 4n-4n^2 x & \frac{1}{2n} \leq x < \frac{1}{n} \\ 0 & \frac{1}{n} \leq x \leq 1 \end{cases}$
      * above is a continuous function with integral 1. the sequence $f_n(x)$ converges to $f=0$
* **Uniform convergence** (to avoid above (3) case)
  * a sequence $(fn)$ in $C[0,1]$ converges uniformly to $f$ if the sequence $a_n = \sup \{ |f_n(x) - f(x) | 0 \leq x \leq 1 \}$ converges to 0
  * in this case one can easily prove the convergence of the Riemann integrals:
    * $\int^1_0 f_n(x) dx \rightarrow \int^1_0 f(x) dx$
  * the *distance* $\sup\{ f(x) - g(x) | 0 \leq x \leq 1\}$ has nothing to do with integration as such and the uniform convergence is too restrictive for many applications.

## Measure

* countably additive
  * $m^* (\cup^\infty_{n=1} E_n) = \sum^\infty_{n=1} m^* (E_n)$

* $\sigma$-field
  * If it contains the base set and is **closed** under complements and counterable unions.

* **measure**
  * a [0, $\infty$] valued function defined on a **$\sigma$-field**.
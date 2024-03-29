+++
title = "The Yoneda Lemma"
date = 2023-02-28
draft = false

[taxonomies]
tags = ["mathematics", "scala", "functional-programming"]

[extra]
author = "Jan-Willem Buurlage"
+++

{{ ctptoc() }}

---

The Yoneda Lemma relates a category \\(\mathcal{C}\\) with the functors from \\(\mathcal{C}\\) to \\(\mathbf{Set}\\). Before we can introduce the lemma we will introduce a number of concepts; first we introduce a class of functors called *hom-functors*, we introduce the notion of *representable functors*, we will discuss the *Yoneda embedding* and finally we will move on to the Yoneda Lemma; one of the important tools in category theory

## Hom-functors

The *hom-functor* for some fixed object \\(c\\), is a functor that sends any object \\(a\\) to the hom-set \\(\text{Hom}(c, a)\\), the set of all arrows between \\( c \\) and \\(a\\). It is clear that for each object we get an associated object in **Set**, but what should this functor do with arrows? We will denote the candidate functor with \\(F = \text{Hom}(c, -)\\). Say we have an arrow \\(f: a \to b\\):

```ascii
      F
 a ────────► Hom(c, a)
 │               │
 │               │
f│               │?
 │               │
 ▼    F          ▼
 b ────────► Hom(c, b)
 ```

The arrow with a question mark is an arrow in **Set**. Arrows in sets are functions, which we can define by saying what it does on elements. The elements of the hom-sets are arrows in \\(\mathcal{C}\\). Given some element of \\(\text{Hom}(c, a)\\), i.e. an arrow in \\(\mathcal{C}\\): \\(g: c \to a\\), we need to obtain an element of \\(\text{Hom}(c, b)\\), i.e. an arrow from \\(c \to b\\). We have the following data:

```ascii
      Ff(g)=?
┌─────────────┐
│             ▼
c ───► a      b
   g
```

We can go to \\(a\\) from \\(c\\) using \\(g\\), but then we need a way to get from \\(a\\) to \\(b\\). We actually have a way to do this, namely the arrow \\(f: a \to b\\) that we started with. We need only to compose! This motivates the following definition:

**Definition**. Let \\(\mathcal{C}\\) be a category, and let \\(c \in \mathcal{C}\\) and \\(f: a \to b \in \mathcal{C}\\). We define the (covariant) **hom-functor** \\(\text{Hom}(c, -): \mathcal{C} \to \mathbf{Set}\\) as:
\begin{align*}
\text{Hom}(c, -)(a) = &\text{Hom}(c, a) \\\\
\text{Hom}(c, -)(f) : &\text{Hom}(c, a) \to \text{Hom}(c, b),\\\\
                      &g \mapsto f \circ g
\end{align*}
Clearly the identity arrow gets mapped to the identity map. To show that compositions are preserved, we compute for any arrow \\(h: c \to a\\):
\begin{align*}
\text{Hom}(c, -)(g \circ f)(h) &= (g \circ f) \circ h \\\\
                               &= g \circ (f \circ h) \\\\
                               &= g \circ (\text{Hom}(c, -)(f)(h)) \\\\
                               &= \text{Hom}(c, -)(g) \left( \text{Hom}(c, -)(f)(h)\right) \\\\
                               &= \left(\text{Hom}(c, -)(g) \circ \text{Hom}(c, -)(f) \right)(h)
\end{align*}

We can also define the contravariant hom-functor: \\(\mathcal{C}^{\text{op}} \to \mathbf{Set}\\) by *precomposing with \\(f\\)*, and we denote it as \\(\text{Hom}(-, d)\\).

Let us introduce a term; functors are called **naturally isomorphic** if there is a natural transformation between them for which all components are isomorphisms. Hom-functors are such an important class of functors from \\(\mathcal{C} \to \mathbf{Set}\\), that they motivate the following definition:

**Definition**. A functor \\(F: \mathcal{C} \to \mathbf{Set}\\) is called **representable** if it is naturally isomorphic to a hom-functor.

To simplify the notation in the upcoming sections, we will denote the covariant hom-functor \\(\text{Hom}(a, -) = h^a\\) and the contravariant hom-functor \\(\text{Hom}(-, b) = h_b\\).

## Yoneda Embedding

For any category \\(\mathcal{C}\\) the Yoneda embedding is a functor between the opposite category and the category of functors between \\(\mathcal{C}\\) and **Set**. Let us first introduce this target category.

**Definition**. Let \\(\mathcal{C}\\) and \\(\mathcal{D}\\) be two categories, then we define \\(\mathbf{Fun}(\mathcal{C}, \mathcal{D})\\) as the category that has functors \\(\mathcal{C} \to \mathcal{D}\\) as objects, and natural transformations between these functors as arrows.

Now, we are ready to describe the Yoneda embedding. Note that because it is a functor between *the opposite of* \\(\mathcal{C}\\) and the category of *functors* between \\(\mathcal{C}\\) and **Set**, it should take objects to functors, and arrows to natural transformations. For all objects, we have introduced a functor associated to it in the previous section; the *hom-functor*. We can call the mapping between the object and its (covariant) hom-functor \\(Y\\).

```ascii
             Y       a
    ┌──►a ────────► h
    │   │           ▲
 op │   │           │   op
f   │  f│           │ Yf
    │   │           │
    │   ▼    Y      │b
    └───b ────────► h
```

The natural transformation \\(Yf\\) should have components which are arrows in **Set**, indexed by objects in \\(\mathcal{C}\\). Let \\(k: c \to d\\), the corresponding naturality square looks like this:

```ascii
             (Yf^op)c
  Hom(a, c) ◄───────── Hom(b, c)
      │                    │
  a   │                    │  b
 h (k)│                    │ h (k)
      ▼                    ▼
  Hom(a, d) ◄───────── Hom(b, d)
             (Yf^op)d
```

So the natural components should be maps between hom-sets. As we will see, we can again find these maps by composition! This is summarized in the following definition:

**Definition**. The **Yoneda functor** \\(Y: \mathcal{C}^{\text{op}} \to \mathbf{Fun}(\mathcal{C}, \mathbf{Set})\\), is defined as follows. Let \\(a \in \mathcal{C}\\) and \\(f: b \to c\\) in \\(\mathcal{C}\\).
\begin{align*}
Ya =& h^a \\\\
Yf^{\text{op}} :& h^c \to h^b \\\\
(Yf^{\text{op}})_a:& \text{Hom}(c, a) \to \text{Hom}(b, a) \\\\
                  :& (g: c \to a) \mapsto (g \circ f: b \to a) \\\\
                  =& h_a f
\end{align*}

Note that the component is defined using *pre-composition*, it is a contravariant hom-functor, whereas the objects \\(Ya\\) are *covariant* hom-functors, i.e. use *post-composition*. Let us check that \\(Yf\\) is indeed a natural transformation by looking at the naturality square introduced above, let \\(\ell: a \to c\\), and lets trace it through the diagram for some \\(k: c \to d\\) and \\(g: b \to a\\):

```
            
         (Yg^op)c
     l  ─────────► l . g
     │             │
 a   │             │  b
h (k)│             │ h (k)
     ▼             ▼
 k . l  ─────────► k . l . g
         (Yg^op)d
```

In words: the naturality condition corresponds simply to the associativity in \\(\mathcal{C}\\). We say that \\(Yf\\) is the _induced natural transformation_ of \\(f\\).

The reason that the Yoneda functor is of such interest is because of the following:

**Theorem 1**. The Yoneda functor \\(Y\\) is _full_ and _faithful_.

We will prove this in the next section, after we state and prove the Yoneda lemma. This theorem has the following corollary:

**Corollary 1**. Let \\(\mu: h^a \to h^b\\) be a natural transformation between hom-functors, then it is given by composition with a unique arrow \\(f: b \to a\\). Furthermore, \\(\mu\\) is a (natural) isomorphism if and only if \\(f\\) is an isomorphism.

This means in particular that if a set-valued functor \\(F\\) is represented by both \\(a\\) and \\(b\\), then there is an isomorphism \\(a \overset{\sim}{\rightarrow} b\\).

Again, by duality, there exists also a full and faithful functor from \\(\mathcal{C} \to \mathbf{Fun}(\mathcal{C}^{\text{op}}, \mathbf{Set})\\).

## Examples of applications

**Example.** _Matrix row operations_. In linear algebra, row operations can be performed without changing the solutions of the linear system. Examples are row permutations, adding the j-th row to the i-th row, or multiplying a row by a (non-zero) scalar. We will show that these _row operations_ are natural, in the following sense.

Let \\(\mathcal{C}\\) be the category where the objects are natural numbers \\(1, 2, 3, \ldots\\), and where arrows \\(n \to m\\) correspond to \\(m \times n\\) matrices. Composition is given by matrix multiplication, indeed if we have arrows:
```
    A_(m x n)   B_(k x m)
n  ─────────► m ─────────► k
```

then the composite \\(B_{k \times m} A_{m \times n} = C_{k \times n}\\) is an arrow from \\(n\\) to \\(k\\), as required. Consider contravariant hom-functors \\(h_n\\) for this category. The hom-set \\(h_n k = \text{Hom}(k, n)\\) consists of \\(n \times k\\) matrices. To show that row operations can be seen as natural transformations \\(\mu: h_n \Rightarrow h_n\\), we fix some \\(k \times m\\) matrix \\(B\\), and look at the following naturality square:

```
            mu_k
   h_n k  ─────────► h_n k
       │             │ 
       │             │
 h_n B │             │ h_n B
       │             │ 
       ▼             ▼
   h_n m ──────────► h_n m
            mu_m
```

Considering some \\(n \times k\\) matrix \\(A\\), the naturality condition states:

\\[\mu(A) B \overset{?}{=} \mu(AB).\\]

To show this, we observe that for all row transformations we have:

\\[\mu(A) = A + \tilde{A}\\]

where the rows of \\(\tilde{A}\\) are either empty, or are multiples of rows of \\(A\\), or:

\\[\mu(A) = A + \Lambda A.\\]

Where \\(\Lambda\\) is a matrix whose elements \\(\Lambda_{ij}\\) represent how many times row \\(j\\) should be added to row \\(i\\). This means we have

\\[\mu(A) B = (A + \Lambda A) B = AB + \Lambda AB = \mu(AB).\\]

as required. By Corollary 1 we have that any natural transformation \\(\mu: h_n \Rightarrow h_n\\) is given by postcomposition (in this category: left-multiplication) with a unique arrow \\(D: n \to n\\). The Yoneda lemma allows us to identify this arrow; it is equal to:

\\[D = \mu_n(\text{Id}_n),\\]

so to perform row operations on a matrix, one can equivalently left multiply with a matrix obtained by applying these operations to the identity matrix. This powers the technique for manually inverting a matrix \\(A\\), where you perform row operations to the matrix \\(A\\) and simultaneously to another matrix \\(B\\) that is initially the identity matrix, until you reduce \\(A\\) to the identity matrix. The resulting matrix \\(B\\), when left multiplied with the original \\(A\\) will perform the row operations, and hence \\(BA = \text{Id}\\), or \\(B = A^{-1}\\).

**Example**.
Another application of Yoneda is the following classic result from group theory known as _Cayley's Theorem_: Any group \\(G\\) is isomorphic to a subgroup of a permutation group.

 _proof_. Recall that we can view a group \\(G\\) as a category \\(\mathcal{C}_G\\) with a single object \\(\{ \bullet \}\\) and with arrows \\(\bullet \to \bullet\\) corresponding to the elements of \\(g\\). Consider the Yoneda embedding \\(Y\\) of this category into \\(\mathbf{Fun}(\mathcal{C}\_G^{\text{op}}, \mathbf{Set})\\),
 and in particular we consider the shape of the image of \\(\bullet\\) under the contravariant hom-functor \\(h\_\bullet\\):
 
 ```
G     Y      Nat(h_., h_.)
. ─────────► h_. 
```


The arrows on the left (displayed collectively as `G`), corresponding to the elements of \\(G\\), get mapped _fully and faithfully_ (by Theorem 1) to the natural transformations between \\(h_\bullet\\) and itself (natural endomorphisms).
 
 The natural endomorphisms \\(h_\bullet\\) are characterized, by Corollary 1, (at the only component \\(G\\)) by left-multiplication of elements \\(G\\) on the set \\(h_\bullet \bullet \simeq G_{\text{set}}\\) which is the underlying set of \\(G\\) (since it is \\(\text{Hom}(\bullet, \bullet)\\)). For each element \\(g \in G\\) we obtain an automorphism \\(G_{\text{set}} \to G_{\text{set}}\\) given by \\(h \mapsto gh\\).
 
 Recall that \\(\text{Aut}(G_{\text{set}})\\) is a group (a permutation group), and note that the collection of automorphisms defined by left multiplication of elements of \\(G\\) is indeed a subgroup of this permutation group. The correspondence between \\(G\\) and the "automorphisms by left-multiplication" is seen to be a group isomorphism by checking the definition directly. \\( \Box \\)

## The Yoneda Lemma

Corollary 1 tells us that any natural transformation between covariant hom-functors \\(h^a\\) and \\(h^b\\) is given by composition with an arrow in the reverse direction \\(f: b \to a\\). Note that this arrow is an element of \\(h^b a = \text{Hom}(b, a)\\).

Less obviously, this result holds also for natural transformations between \\(h^a\\) and any other set-valued functor \\(F\\).

What would a function between \\(h^a\\) and \\(F\\) look like? We see that a component of the natural transformation should take an element from \\(h^a b\\), i.e. an arrow \\(g: a \to b\\), to some element of \\(Fb\\). We can do this by *evaluating* the lifted arrow \\(Fg\\) , which is a map between the sets \\(Fa\\) and \\(Fb\\), at a fixed \\(x \in F a\\).

This gives us an idea for a natural transformation corresponding to an element of \\(Fa\\). We summarize this in the following proposition.


**Proposition**. Let \\(F: \mathcal{C} \to \mathbf{Set}\\) be a functor, and \\(a \in \mathcal{C}\\). Any element \\(x \in Fa\\) induces a natural transformation from \\(h^a\\) to \\(F\\), by evaluating any lifted arrow in \\(x\\).

_proof_. We have to show that this induces a natural transformation, i.e. that the following diagram commutes:

```
      a    F_(x)
     h b  ─────────► F b
       │             │ 
   a   │             │
  h f  │             │ F f
       │             │ 
       ▼             ▼
      a  ──────────► F c
     h c    F_(x)
```

Here we denote:
\\(F \\_ (x): h^a b \to F b,~f \mapsto F f(x).\\)
To show that the diagram commutes, fix an arrow \\(g: a \to b \in h^a b\\). If we start taking it along the top side we obtain:
\begin{align*}
F f (F g(x)) &= (F f \circ F g)(x) = F(f \circ g)(x) \\\\
&= (F \\_ (x))(f \circ g) = (F \\_ (x))((h^a f)(g))
\end{align*}
which is equal to taking it along the bottom, hence the diagram commutes. \\(\Box\\)

The Yoneda lemma states that *all natural transformations between \\(h^a\\) and \\(F\\) are of this form*.

**Theorem 2**. _The Yoneda lemma_. Let \\(\mathcal{C}\\) be a category, let \\(a \in \mathcal{C}\\), and let \\(F: \mathcal{C} \to \mathbf{Set}\\) be a set-valued functor. There is a one-to-one correspondence between elements of \\(Fa\\), and natural transformations:
\\(\mu: h^a \Rightarrow F.\\)

_proof_. We already saw that each element of \\(Fa\\) induces a natural transformation, so we have a map:
\\(\Phi: F a \to \text{Nat}(h^a, F).\\)
Here, \\(\text{Nat}(h^a, F)\\) denotes the set of natural transformations between \\(h^a\\) and \\(F\\).

 We now need to show that \\(\Phi\\) has an inverse.
Let \\(\mu\\) be any natural transformation, then we can obtain an element of \\(F a\\) by looking at the component \\(\mu_a\\) and let it act on the identity arrow \\(\text{id}_c \in h^a a\\), i.e.:

\\(\Psi: \mu \mapsto \mu_a(\text{id}_a).\\)

Now let us show that \\(\Phi\\) and \\(\Psi\\) are inverses of each other. First, we compute:

\\( \Psi(\Phi(x)) = \Psi(F \\_ (x)) = F \text{id}\_a (x) = \text{id}\_{F a}(x) = x \\)

so \\(\Psi\\) is a left inverse of \\(\Phi\\). To show that it is also a right inverse, we need to show that:
\\(\Phi(\Psi(\mu)) = \mu,\\)
or in components:
\\(\Phi(\Psi(\mu))_b = \mu_b.\\)
We note that by definition, for any \\(f: a \to b\\) in \\(h^a b\\):
\\(\Phi(\Psi(\mu))_b (f) = (\Phi(\mu_a(\text{id}_a)))_b (f) = Ff (\mu_a(\text{id}_a)).\\)
Since \\(\mu\\) is a natural transformation we have that the following diagram commutes:
```
            mu_a(x)
   h^a a  ─────────► F a
       │             │ 
       │             │
 h^a f │             │ F f
       │             │ 
       ▼             ▼
   h^a b ──────────► F b
            mu_b(x)
```
In particular, consider the element \\(\text{id}_a \in h^a a\\). Tracing this along bottom this gets mapped to \\(\mu_b (f)\\), while along the top it gives precisely \\(Ff (\mu_a(\text{id}_a))\\), so we have shown that:
\\(\Phi(\Psi(\mu))_b (f) = Ff (\mu_a(\text{id}_a)) = \mu_b(f).\\)
And hence, \\(\Psi\\) is also a right inverse of \\(\Phi\\), and thus \\(\Phi\\) is a bijection, as required. \\(\Box\\)

One can also show, that this correspondence is 'natural' in \\(a \in \mathcal{C}\\) and \\(F\\).

Let us now prove Theorem 1.

> _proof of Theorem 1_. By Yoneda's Lemma there is a bijection between the sets:
> \\(\text{Nat}(h^b, h^a) \simeq h^a b = \text{Hom}(a, b)\\)
> for all objects \\(a\\) and \\(b\\) of \\(\mathcal{C}\\), which directly implies that the functor \\(Y\\) is full and faithful.

Let us recap what we have seen so far. We discussed a special class of set-valued functors called *hom-functors*. These hom-functors, like hom-sets, relate objects directly with the arrows between them.

Next we showed that we can *embed* any category into the category of contravariant set-valued functors of this category, sending objects to their hom-functors. We also showed that this embedding, as a functor, is *full* and *faithful*, which suggests that all the information of the category and its objects, is contained in its hom-functors.

When looking at what this means for the arrows, we noted that in particular any natural transformation between hom-functors is given by composition with arrows of our category.

To prove this, we stated and proved the Yoneda lemma -- which is an important result in its own right. It shows that for an arbitrary set-valued functor, there is a bijection between elements of the set \\(F a\\) and natural transformations from \\(h^a\\) to \\(F\\),

All functors in programming are set-valued, since types can be viewed as set. We first show two simple applications of Yoneda's lemma in mathematics, and next we see some initial applications of the Yoneda lemma to programming.

## Yoneda in programming

We will discuss a hopefully intuitive way of looking at the Yoneda lemma in Scala, by pinpointing a function with a single evaluation. 

Let us first see how we can translate the relevant tools of Yoneda to Scala. We have the following concepts:

- *hom-sets*: the hom-set of types `A` and `B` are the arrows between `A` and `B`, i.e. functions of the type `A => B`. Note that this hom-set is again in the category of types.
- The *hom-functor* corresponding to a type `A` should be a functor, i.e. a type constructor, that produces the hom-set `A => B` when given a type `B`, for some fixed type `A`. On functions `B => C` it should get a function between the hom-sets of `A` and `B, C` respectively, i.e.:

  ```scala
  def map[A, B, C](f: B => C, g: A => B): A => C =
    f compose g
  ```

  And indeed, we see that we can simply use composition.
- Yoneda's lemma says that for any other functor `F`, we can produce a natural transformation (i.e. polymorphic function in a type `B`) from the hom-functor for a fixed `A` by looking at elements of `F[A]`.

Next we look at a simple example of how to apply this final point in Scala.

### Reverse engineering machines

I first encountered this example here: <http://blog.sigfpe.com/2006/11/yoneda-lemma.html>.

We set `F` equal to `Id`, the identity functor, and consider a natural transformation between `HomFunctor[A]` and `Id`, this has the form (at the component `B`):

```scala
// We fix A
type A = ...;

// HomFunctor[A, B]    Id[B]
//                |        |
def machine[B](f: A => B): B =
  // ...?
```
Say we are given any function with this signature, and we want to know how it is implemented. We can actually do this in a *single evaluation*, using the Yoneda lemma. The Yoneda lemma says precisely that such a *machine* is given uniquely by any element of `Id[A] = A`, i.e. some value of the type `A`. This makes a lot of sense in this context, since we can be given *any* `B`, and the only tool that we have to produce a value for `B` is to use the function `f: A => B` that is supplied to us. Furthermore, the polymorphic function should behave the same for any type, so it can only be implemented as:
```scala
def machine[A, B](f: A => B): B =
  f(x)
```
where `x` is some fixed element of type `A`. Now, the Yoneda lemma also tells us a way to obtain `x`, we simply supply the identity `id = (y: Any) => y`:
```scala
x = machine(id) // obtain the 'hidden element'
```

What if `F` is not the identity function, but say the `Word` functor introduced [in a previous post](/blog/ctp-programmer-category.md). The story actually does not change much, we now have a function with the signature:
```scala
def machine[B](f: A => B): Word[B] =
  // ...?
```
the Yoneda lemma says that internally, any function of this signature should maintain a word of the type `Word[A]`, and when given a function `f: A => B` it maps this over the internal list to produce a value of the type `Word[B]`. Again, we can get this list by feeding the `id` function into the machine.

### Continuation Passing Style

In programming, there is an equivalence between what is called *direct* style, where functions return values, and *continuation passing style* (CPS), where each *called function* takes an additional argument which is a *handler function* that does something with the result of the called function.

Say we have some function in C++.

```cpp
T add(T a, T b) {
    return a + b;
}
```
Which we can use by calling e.g. `auto x = add(1, 2)`. The CPS version of this function looks like
```cpp
void add_cps(T a, T b, F cont) {
    cont(a + b);
}
```
and the way it is used is:
```cpp
add_cps(1, 2, [](auto result) {
    // ...
});
```
In other words, the CPS version of the function does not *return a value*, but rather passes the result to a handler. We do not bind the result of a function to a value, but rather to the *argument of a handler function*.

You may recognize this style of programming from writing asynchronous programs, where continuations can be used to deal with values produced in the future by other threads without blocking/suspending the at the callsite. Continuations are also often used in UI frameworks, where a handler is used whenever e.g. a button is pressed, or the value of a slider has changed.

This CPS passing style can also be used to implement exceptions. Say we have a function that can throw:
```cpp
void can_throw(F raise, G cont) {
    // ...
}
```
Here, the idea is that `raise` gets called if an error occurs, while `cont` gets called when a result has been computed succesfully. What is also interesting is that CPS can be used to implement *control flow*. For example, the called function can call cont multiple times (loops), or only conditionally.

Let us show that the *continuation passing transform* (CPT), i.e. going from direct style to CPS, is nothing more then the Yoneda embedding. Say we have a function:
```scala
f: A => B
```
Let us remind ourselves that the Yoneda embedding takes such an arrow, and produces a map \\((Yf)_c = \text{Hom}(c, b) \rightarrow \text{Hom}(c, a)\\) for all \\(c \in \mathcal{C}\\). In Scala, this embedding could be implemented like this:
```scala
def yoneda [A, B]: X => (f: (A => B), g: (B => X)): (A => X) =
  (k: A => X) => k compose f
```
Going the other way around is easy, we simply pass `id` as our continuation `k`.

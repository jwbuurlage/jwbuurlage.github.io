+++
title = "Products, coproducts, and algebraic data types"
date = 2022-07-26
draft = true

[taxonomies]
tags = ["mathematics", "scala", "functional-programming"]

[extra]
author = "Jan-Willem Buurlage"
+++

{{ ctptoc() }}

## Duality and products of objects

### Products

Initial objects and terminal objects have a *universal property*, meaning that they are defined by the property that e.g. all other objects have a *unique morphism to the object*. A more involved example of such a universal property is the *notion of a product of objects*. The categorical product is a unifying definition for many 'products' encountered in mathematics, such as the cartesian product, product group, products of topological spaces, and so on.

**Definition**. Let \\(\mathcal{C}\\) be a category, and let \\(a, b \in \mathcal{C}\\) be objects in \\(\mathcal{C}\\). A _product_ of \\(a\\) and \\(b\\) is an object \\(a \times b \in \mathcal{C}\\) along with two arrows \\(p_1: a \times b \to a\\) and \\(p_2: a \times b \to b\\) (the _projections_) so that for all objects \\(c \in \mathcal{C}\\) and arrows \\(f: c \to a\\) and \\(g: c \to b\\) there exists a unique morphism \\(q: c \to a \times b\\) that makes the following diagram commute:

```
  ┌────── c ──────┐
  │       │       │ 
 f│       │q      │g
  ▼       ▼       ▼
  a ◄── a x b ──► b
     p1       p2
```

In this case, the (unique) arrows \\(q\\) are what gives the product a *universal mapping property*. If a product exists, it is unique up to unique isomorphism.

We say that the functions \\(f\\) and \\(g\\) *factors* through \\(a \times b\\), or that \\(a \times b\\) *factorizes* \\(f\\) and \\(g\\). The reason for this name is clear when making the analogy with numbers. From the diagram above, we have the following two equalities:

\\(f = p_1 \circ q,~g = p_2 \circ q.\\)

Let's give a concrete example with numbers. We consider the category of natural numbers, where an arrow \\( a \to b \\) indicates that \\( b \\) is a multiple of \\( a \\).

```
   ┌───── 3 ──────┐
x10│      │x5     │x15
   ▼      ▼       ▼
  30 ◄─── 15 ───► 45
     x2       x3
```

Note that the two equalities translate to

- \\( 10 = 5 \times 2 \\)
- \\( 15 = 5 \times 3 \\)

In this category, the product corresponds to the greatest common divisor of \\( a \\) and \\( b \\). Indeed, \\( c \\) would have to be _any_ common divisor of \\( a \\) and \\( b \\) for \\( f \\) and \\( g \\) to exist. The _greatest_ common divisor, is always a multiple of any given common divisor.

**Example**. Let us consider the product of objects in **Set**. Consider two sets \\(A, B\\). We have a clear candidate for a product; the cartesian product \\(A \times B\\). Given any element (or _pair_) \\((a, b) \in A \times B\\), the projections \\(p_1, p_2\\) send it to \\(a\\) and \\(b\\) respectively. Is this indeed a product?

Let \\(V\\) be any other set, with arrows (functions) \\(f\\) to \\(A\\) and \\(g\\) to \\(B\\). Can we construct a (unique) arrow \\(q\\) to \\(A \times B\\)?

```
   ┌────── V ──────┐
   │       │       │ 
  f│       │q?     │g
   ▼       ▼       ▼
   A ◄── A x B ──► B
      p1       p2
```

Consider any element \\(v \in V\\). It gets mapped to \\(f(v) \in A\\), and \\(g(v) \in B\\). Let \\(q: v \mapsto (f(v), g(v))\\), then \\((p_1 \circ q)(v) = f(v)\\), and thus \\(p_1 \circ q = f\\). Similarly \\(p_2 \circ q = g\\).

Indeed, we have constructed an arrow that makes the above diagram commute. It should also be clear that this is the _only arrow_ that satisfies this, as if \\( q \\) maps any of the two components differently, after projecting we would not end up with the correct value. We conclude that \\(A \times B\\) is the product of \\(A\\) and \\(B\\) in the category **Set**. Another valid example of a product of sets would be \\(B \times A\\), which is clearly isomorphic to \\(A \times B\\). The isomorphism corresponds to 'swapping' the elements, which is its own inverse.

For a completely different example, we consider the category corresponding to a poset.

**Example.** Let us consider the product of objects in the category corresponding to some poset \\(P\\). Consider two elements \\(x, y \in P\\). A product \\(z \equiv x \times y\\) would be equipped with two arrows \\(z \to x\\) and \\(z \to y\\), which means \\(z \leq x\\) and \\(z \leq y\\). Furthermore, for any element \\(w\\) with arrows to \\(x, y\\) (i.e. \\(w \leq x\\) and \\(w \leq y\\)), there has to be an arrow \\(q: w \to z\\) (i.e. \\(w \leq z\\)). This is the same as saying that, in addition to \\(z \leq x\\) and \\(z \leq y\\), we have for all elements \\(w\\) of the poset:
\\(w \leq x \text{ and } w \leq y \implies w \leq z\\)
This means that \\(z\\) is the "largest element that is smaller or equal to \\(x\\) and \\(y\\)", also called the _infimum_ of \\(x\\) and \\(y\\).

### Duality

For any category, we can define the category with all arrows (and composition) reversed.

**Definition**. The _opposite category_ \\(\mathcal{C}^{\text{op}}\\) of a category \\(\mathcal{C}\\) is the category with:

- The same objects as \\(\mathcal{C}\\).
- For all arrows \\(f: a \to b\\) in \\(\mathcal{C}\\), there is an arrow \\(f^{\text{op}}: b \to a\\)
- The composition of \\(f^{\text{op}} : a \to b\\) and \\(g^{\text{op}}: b \to c\\) is given by:
\\(g^{\text{op}} \circ f^{\text{op}} = (f \circ g)^{\text{op}}\\)

The opposite category is very useful, because many concepts defined in the original category have 'dual notions' in the opposite category. Clearly, for example, an *initial object in \\(\mathcal{C}\\) is a terminal object in \\(\mathcal{C}^{\text{op}}\\)*. Similarly, an arrow that is *mono in \\(\mathcal{C}\\) is epi in \\(\mathcal{C}^{\text{op}}\\)*. This is called **duality**, and provides so-called 'co-' notions of constructs, as well as co- versions of theorems.

Whenever defining something it always make sense to see what this means in the opposite category, giving you a lot of free information. For example, we showed that faithful functors reflects monos. Looking at the dual category, we immediately have that it also reflects epis!


### Coproducts

Let us apply the idea of *duality*. What would be the dual notion of the product? Let us take the product diagram, and reverse the arrows:

```
 ┌─────► c ◄─────┐
 │       ▲       │ 
f|       |q      |g
 |       |       |
 a ──► a + b ◄── b
    p1       p2
```

We have arrows going from objects \\(a, b\\) into the coproduct (written \\( a + b \\), we will see why soon), and from this coproduct arrows going to arbitrary target objects \\(c\\). The arrows \\(a \to a + b\\) and \\(b \to a + b\\) already look kind of like an inclusion. Let us see what happens when we apply duality to the product definition, and change some names.

**Definition**. Let \\(\mathcal{C}\\) be a category, and let \\(a, b \in \mathcal{C}\\) be objects in \\(\mathcal{C}\\). A _coproduct_ of \\(a\\) and \\(b\\) is an object \\(a + b \in \mathcal{C}\\) along with two arrows \\(i_1: a + b \leftarrow a\\) and \\(i_2: a + b \leftarrow b\\) (the _inclusions_) so that for all objects \\(c \in \mathcal{C}\\) and arrows \\(f: c \leftarrow a\\) and \\(g: c \leftarrow b\\) there exists a unique morphism \\(q: c \leftarrow a + b\\) that makes the following diagram commute:

```
 ┌─────► c ◄─────┐
 │       ▲       │ 
f|       |q      |g
 |       |       |
 a ──► a + b ◄── b
    i1       i2
```

Note that this is precisely the definition of the product, with all arrows reversed and the projections renamed to \\(i_1\\) and \\(i_2\\).

Because of the properties that we will soon discover, the coproduct is also called the *sum*. While it may seem like a similar construction, this dual notion turns out to be fundamentally different. To show this, let us explore what it means in the category **Set**.

**Example.** Consider two sets \\(A, B\\). When looking at the diagram for the coproduct, we see that we need to find some kind of set in which elements of \\(A\\) and \\(B\\) are represented but completely independent; since \\(c\\) is now the target of the function $q$ we have to factor through \\(a + b\\): it needs to be able to reflect the full image under \\( f \\), as well as the full image under \\( g \\).

This means that \\( A + B \\) has to be the _disjoint union_ of \\(A\\) and \\(B\\). The ordinary union of two non-disjoint sets would not be enough, as in the intersection of \\(A\\) and \\(B\\) we would not know whether \\(q\\) should represent \\(f\\) or \\(g\\). We represent the disjoint union as:

\\(A + B \equiv \{ (a, 0)~|~a \in A \} \cup \{ (b, 1)~|~b \in B \}.\\)

It is clear what \\(i_1\\) and \\(i_2\\) should be: they add the relevant tag \\( 0 \\) or \\( 1 \\) as the second component of the pair.

Let \\(V\\) be any set, and assume we are given arrows (functions) \\(f: A \to V\\) and \\(g: B \to V\\).

```
 ┌─────► V ◄─────┐
 │       ▲       │ 
f|       |q      |g
 |       |       |
 A ──► A + B ◄── B
    i1       i2
```

Consider any element \\(a \in A\\). It gets mapped to \\(f(a) \in V\\), and to \\(i_1(a) = (a, 0)\\) in \\(A + B\\). Then we should set \\(q(a, 0) \equiv f(a)\\), and similarly we set \\(q(b, 1) \equiv g(b)\\). This already defines \\(q\\) uniquely and completely, so we conclude that the disjoint union is indeed the coproduct in the category **Set**.

We note there that the coproduct (and product) of two objects, generalizes also to products of more than 2 objects (by simply adding more maps \\(i_1, i_2, i_3 \ldots\\)).

## Algebraic data types

Let us apply the product (and coproduct) concepts to the category of types. Since we already saw what these constructs mean for sets, namely the cartesian product and the disjoint union respectively, it should be clear what this means for types.

Given a type \\(A\\) and a type \\(B\\), the product corresponds to a *pair* (or more generally, a _tuple_), written `(A, B)` in Scala. For example:

```scala
val x: (Int, String) = (3, "foo")
```

To allow us to experiment a bit more freely, let us define our own version of a pair.
```scala
enum Pair[A, B]:
  case Pair(a: A, b: B)
```

Here, we give the unique value constructor the same name as its type constructor. The coproduct (or _sum type_) corresponds to a value that has either type `A`, or type `B`. This is implemented as the `Either` data type we already saw in a [previous post](./blog/ctp-programmer-category).

```scala
enum Either[A, B]:
  case Left(a: A)
  case Right(b: B)
```

Here, the two value constructors take an element of type `A`, or an element of type `B` respectively.

A sum type means choosing an alternative between types, while the product type is a combination of the types. A simple example of a sum type has only three 0-ary value constructors.

```scala
enum Enum:
    case One
    case Two
    case Three
```

We can also combine sum and product types. A node of a binary tree of type `A` has a sum type: it is either a `Leaf`, or it is the product type of:
    - Tree on the left
    - value of type `A` for the value of the node
    - Tree on the right

```scala
enum Tree[+A]:
    case Node (left: Tree[A], value: A, right: Tree[A])
    case Leaf
```

## ADTs in C++

In C++, using ADTs requires a variety of different language and library features.

- We can use `std::pair` (or a `std::tuple` for n-ary products) to mimic a type as `Pair`. However, its using it is arguably a bit awkward and convoluted.

- You could also use a `struct` (of one or more elements) to implement product types.  

- A sum type corresponds roughly to a `union`. In C++17 a standardized 'tagged union' `std::variant` was added that more accurately models the coproduct.

- An `enum` represents a fixed number of alternative constants. This correponds to the sum type of multiple 0-ary value constructors (implicitly the finite sum type of the type `()` with itself).

## ADTs as a semi-ring

Using the product and sum types, we can turn the type system into a semi-ring, where we define:

- \\(0\\) = `Void`
- \\(1\\) = `()`
- \\(A + B\\) = `Either[A, B]`
- \\(A \times B\\) = `(A, B)`

Let us check that \\(0\\) really works as \\(0\\). If we add `Void` to a type `A`, we get `Either[A, Void]`. Since there can never be a value of type `Void`, we are guaranteed to get a `Left a` when we see a value of type `Either[A, Void]`, which means the type is isomorphic to `A`.

In other words:

\\(A + 0 = a.\\)

Similarly, if we have a product with `Void`, we can never instantiate a pair (because there is no value for `Void`), so the corresponding product type is again `Void`.

\\(A \times 0 = 0.\\)

Although this is all a bit of a stretch, this analogy has some interesting properties, and we can do some real algebra with our types and try to interpret the results. Consider the `Word` type:

```haskell
enum Word[+A]:
    case Cons(x: A, xs: Word[A])
    case Empty
```

In our 'semi-ring', writing \\(x\\) for `Word[A]`, this would look like the expression:
\\(x = 1 + a \times x\\)
This is unsolvable, but we can try to iteratively substitute \\(x\\) into the right hand side:

\begin{align*}
    x &= 1 + a \times (1 + a x)\\\\
      &= 1 + a + a^2 x\\\\
      &= 1 + a + a^2 (1 + a x)\\\\
      &= 1 + a + a^2 + a^3 (1 + ax) \\\\
      &= \ldots
 \end{align*}

 Which can be read as 'a list is either empty, or it has one element of type `a`, or it has two elements of type `a`, etc.
 
 Although this is mostly an entertaining (and, depending on your view, an overly complicated) way of looking at types, a similar correspondence from types to logical operations forms the basis of the Curry-Howard isomorphism that connects type theory to logic in a very fundamental way.

## Bi-functors

**Definition**. Given two categories \\(\mathcal{C}, \mathcal{D}\\) their product category \\(\mathcal{C} \times \mathcal{D}\\) is given by:
- The objects are pairs \\((c, d)\\) where \\(c \in \mathcal{C}\\) and \\(d \in \mathcal{D}\\).
- The arrows are pairs of arrows, \\((f, g): (c, d) \to (c', d')\\) for \\(f: c \to c'\\) in \\(\mathcal{C}\\) and \\(g: d \to d'\\) in \\(\mathcal{D}\\).
- The identity arrow for \\((c, d)\\) is the pair \\((\text{id}_c, \text{id}_d)\\).
- Composition of arrows happens per component, i.e.\ when \\(f, g \in \mathcal{C}\\) and \\(h, k \in \mathcal{D}\\):
\\((f, h) \circ (g, k) \equiv (f \circ g, h \circ k)\\)

Note that alternatively we could define this as the product of objects in the category **Cat**.

This brings us to the concept of a bifunctor, which can be seen as a 'functor of two arguments'.

**Definition**. Let \\(\mathcal{C}, \mathcal{D}, \mathcal{E}\\) be categories. A bifunctor is a functor:
\\(F: \mathcal{C} \times \mathcal{D} \to \mathcal{E}.\\)


In Scala the bifunctor can be implemented as a `trait` as follows:

```scala
trait Bifunctor[F[_, _]]:
  def bimap[A, B, C, D](f: (A => C), g: (B => D), x: F[A, B]): F[C, D] =
    first(f, second(g, x)) 
  
  def first[A, B, C](f: (A => C), x: F[A, B]): F[C, B] =
    bimap(f, (y: B) => y, x)

  def second[A, B, D](g: (B => D), x: F[A, B]): F[A, D] =
    bimap((y: A) => y, g, x)
```

Here you see a circular definition. This means it is enough to *either* provide the `bimap`, or the `first` and `second` functions.

We can prove why this is enough mathematically, as shown in the following proposition. We denote pairs as \\(\langle c, d \rangle \in \mathcal{C} \times \mathcal{D}\\):

**Proposition**. Let \\(F: \mathcal{C} \times \mathcal{D} \to \mathcal{E}\\) be a bifunctor. Then:

\\(
    F \langle c, - \rangle \equiv G_c: \mathcal{D} \to \mathcal{E}, d \mapsto F \langle c, d \rangle, (g: d \to d') \mapsto F \langle \text{id}_c, g \rangle
\\)
\\(
     F \langle -, d \rangle \equiv H_d: \mathcal{C} \to \mathcal{E}, c \mapsto F \langle c, d \rangle, (f: c \to c') \mapsto F \langle f, \text{id}_d \rangle
\\)

are functors for all \\(c \in \mathcal{C}\\) and \\(d \in \mathcal{D}\\) respectively, and furthermore they satisfy:

\\(G_c d = H_d c\\)

\\(G_{c'} g \circ H_d f = H_{d'} f \circ G_c g\\)

for all \\(c, c' \in \mathcal{C}\\) and \\(d, d' \in \mathcal{D}\\).

Conversely, let \\(G_c, H_d\\) be family of functors so that the above two equalities hold, then:

\\(\tilde{F}: \mathcal{C} \times \mathcal{D} \to \mathcal{E}, \langle c, d \rangle \mapsto G_c d, \langle f, g \rangle \mapsto H_{d'} f \circ G_c g\\)

is a bifunctor, and satisfies \\(\tilde{F} \langle c, - \rangle = G_c\\) and \\(\tilde{F} \langle -, d \rangle = H_d\\).

_proof_. Let us first show that we can construct the functors \\(G_c\\) and \\(H_d\\) from a bifunctor \\(F\\). We show that \\(G_c\\) is a functor, \\(H_d\\) follows similarly.

\\( G_c(\text{id}\_d) = F \langle \text{id}\_c, \text{id}\_d \rangle = \text{id}\_{F \langle c, d \rangle} \\)

\\(G_c(g \circ g') = F \langle \text{id}_c, g \circ g' \rangle = F (\langle \text{id}_c, g \rangle \circ \langle \text{id}_c, g' \rangle) \\)

\\( F \langle \text{id}_c, g \rangle \circ F \langle \text{id}_c, g' \rangle = G_c g \circ G_c g' \\)

The mapped arrows have the correct (co)domains, hence \\(G_c\\) is a functor for all \\(c\\). We immediately have \\( G_c d = H_d c \\), as by definition both sides are equal to \\(F \langle c, d \rangle\\). To show \\( (G_{c'} g \circ H_d f = H_{d'} f \circ G_c g \\) we compute:

\begin{align*}
G_{c'} g \circ H\_d f &= F \langle \text{id}\_{c'}, g \rangle \circ F \langle f, \text{id}\_d \rangle \\\\
&= F(\langle \text{id}\_c, g \rangle \circ \langle f, \text{id}\_d \rangle) = F(\langle f, g \rangle) = F(\langle f, \text{id}\_{d'} \rangle \circ \langle \text{id}\_c, g \rangle) \\\\
&= F\langle f, \text{id}\_{d'} \rangle \circ F\langle \text{id}\_c, g \rangle = H\_{d'} f \circ G\_c g
\end{align*}

To show the converse statement, we compute:

\begin{align*}
F \langle \text{id}\_c, \text{id}\_d \rangle &= G\_c \text{id}\_d \circ H\_d \text{id}\_c = \text{id}\_{G\_c d} \circ \text{id}\_{H\_d c} = \text{id}\_{F \langle c, d \rangle} \circ \text{id}\_{F \langle c, d \rangle} = \text{id}\_{F \langle c, d \rangle} \\\\
F(\langle f, g \rangle \circ \langle f', g' \rangle) &= F\langle f \circ f', g \circ g' \rangle = G\_{c'} g \circ G\_{c'} g' \circ H\_d f \circ H\_d f' \\\\
&= G\_{c'} g \circ H\_{d'} f \circ G\_{c} g' \circ H\_d f' = F \langle f, g \rangle \circ F \langle f', g' \rangle
\end{align*}

Which concludes the proof.

**Example**. Whenever you have a category \\(\mathcal{C}\\) where the product of two objects exists for all pairs of objects, then this gives rise to a bifunctor:

\begin{align*}
\times&: \mathcal{C} \times \mathcal{C} \to \mathcal{C}\\\\
      &: (a, b) \mapsto a \times b\\\\
      &: (f: a \to a', g: b \to b') \mapsto (f \times g: a \times b \to a' \times b')
\end{align*}

where we find \\(f \times g\\) by looking at the diagram:

```
     p1       p2
  a ◄── a x b ──► b
  │       │       │ 
 f│       │ f x g │g
  ▼       ▼       ▼
  a'◄── a'x b'──► b'
     p1'      p2'
```

By definition of the product \\(a' \times b'\\), we have that for any object \\(c\\) that has arrows to \\(a'\\) and \\(b'\\), there should be a _unique_ arrow \\(c \to a' \times b'\\). Note that \\(f \circ p_1\\) and \\(g \circ p_2\\) are arrows from \\(a \times b\\) to \\(a'\\) and \\(b'\\) respectively. Taking \\( c \equiv a \times b \\), this means that we can set \\(f \times g\\) to the unique arrow going between \\(a \times b\\) and \\(a' \times b'\\).

By duality, there is also a bifunctor corresponding to the coproduct if it is defined everywhere.

What would these two examples mean in Scala? The product is the 'pair functor' `Pair`, and the coproduct is the sum type `Either`.

```scala
given Bifunctor[Either] with
  override def bimap[A, B, C, D](f: (A => C), g: (B => D), x: Either[A, B]): Either[C, D] =
    x match 
      case Either.Left(x) => Either.Left(f(x))
      case Either.Right(y) => Either.Right(g(y))

given Bifunctor[Pair] with
  override def bimap[A, B, C, D](f: (A => C), g: (B => D), x: Pair[A, B]): Pair[C, D] =
    x match
      case Pair.Pair(x, y) => Pair.Pair(f(x), g(y))
```

These are examples of type constructors (or algebraic data types, as we have seen). Since functors compose, we could ask ourselves: "Are all algebraic data types functors?". The answer is positive; functor implementations can be automatically derived for all ADTs! 
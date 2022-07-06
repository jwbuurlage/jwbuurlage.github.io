+++
title = "Categories, functors, and natural transformations"
date = 2022-07-06
draft = false
[extra]
author = "Jan-Willem Buurlage"
+++

{{ ctptoc() }}

## Preface

Mathematical theories seem to have a number of common ingredients: there are _objects_ such as numbers, sets, or shapes, and _arrows_ that somehow relate these objects. For example, a number is related to its divisors, functions can relate sets, and shapes can be transformed into each other. These relations can be _composed_. In category theory, domain-specific knowledge is not used and only _objects_ and _arrows_ are considered. Results about categories only make use of a single operation: composition of arrows. Using _only_ these simple concepts, it is still possible to obtain rich results that automatically apply for all categories.

A _type_ in a programming language is a set of possible values. We can view a function `f` that takes `a: A` (denoting a variable `a` of type `A`) and outputs `b: B` as an arrow between objects `A` and `B`. This means that also results from category theory can be applied to programming.

## Definition of categories

We start with giving the definition of a category:

**Definition**. A _category_ \\( C = (O, A, \circ) \\) consists of:
- a collection \\( O \\) of _objects_, written \\( a,b,\ldots \in O \\).
- a collection \\( A \\) of _arrows_ written \\( f,g,\ldots \in A \\) between these objects, e.g. \\( f: a \to b \\).
- a notion of _composition_ \\( f \circ g \\) of arrows.
- an identity arrow \\( \text{id}_a \\) for each object \\( a \in O \\).
The composition operation and identity arrow should satisfy the following laws:

1. _Composition_: If \\( f: a \to b \\) and \\( g: b \to c \\) then \\( g \circ f: a \to c \\).

    ```
       f      g
    a ───► b ───► c
    │             ▲
    └─────────────┘
          g.f
    ```

1. _Composition with identity arrows_:  If \\( f: x \to a \\) and \\( g: a \to x \\) where \\( x \\) is arbitrary, then:
    $$\text{id}_a \circ f = f,~g \circ \text{id}_a = g.$$


    ```
     id
    ┌───┐
    └─► a ───► x
           g
    ```

1. _Associativity_: If \\( f: a \to b \\), \\( g: b \to c \\) and \\( h: c \to d \\) then:
    $$(h \circ g) \circ f = h \circ (g \circ f).$$
    This is the same as saying that the following diagram commutes:
    

    ```
          g.f
    ┌─────────────┐
    │  f      g   ▼  h
    a ───► b ───► c ───► d
           │             ▲
           └─────────────┘
                 h.g
    ```
    
    Saying a diagram commutes means that for all pairs of vertices \\( a' \\) and \\( b' \\) all paths from between them are equivalent (i.e. correspond to the same arrow of the category).

---


If \\( f: a \to b \\), then we say that \\( a \\) is the *domain* and \\( b \\) is the *codomain* of \\( f \\). It is also written as:
$$\text{dom}(f) = a,~\text{cod}(f) = b.$$
The composition \\( g \circ f \\) is only defined on arrows \\( f \\) and \\( g \\) if the domain of \\( g \\) is equal to the codomain of \\( f \\).

We will write for objects and arrows respectively simply \\( a \in C \\) and \\( f \in C \\), instead of \\( a \in O \\) and \\( f \in A \\).

### Examples of categories

A number of simple examples of categories:

- **0**, the empty category \\( O = A \equiv \emptyset \\).
- **1**, the category with a single element and (identity) arrow:

    ```
     id
    ┌───┐
    └─► a 
    ```
- **2**, the category with a two elements and a single arrow between these elements
  ```
   id          id
  ┌───┐      ┌───┐
  └─► a ───► b ◄─┘
  ```
- \\( \rightrightarrows \\): the category with two elements and two parallel arrows between these elements:

  ```
   id          id
  ┌───┐ ───► ┌───┐
  └─► a ───► b ◄─┘
  ```

From now on we will sometimes omit the identity arrows when drawing categories.

- Another example of a category is a _monoid category_, which is a specific kind of category with a single object.

    **Definition**.
    A _monoid_ \\( (M, \cdot, e) \\) consists of:
    - a set \\( M \\)
    - an associative binary operation \\( (\cdot): M \times M \to M \\)
    - a unit element w.r.t. \\( (\cdot) \\), i.e. \\( \forall_m~e \cdot m = m \\)

    For those familiar with group theory, it is a group structure without requirement of inverse elements. It is also called a _semi-group with unit_)

    This corresponds to a category \\( C(M) \\) where:
    - There is a single object (for which we simply write \\( M \\))
    - There are arrows \\( m: M \to M \\) for each element \\( m \in M \\).
    - Composition is given by the binary operation of the monoid: \\( m_1 \circ m_2 \equiv m_1 \cdot m_2 \\).
    - The identity arrow \\( \text{id}_M \\) is equal to \\( e \\), the unit of the monoid.


- We can also consider natural numbers \\( \mathbb{N}_{> 0} \\), with arrows going from each number to its multiples.

  ```
  ┌───┬───┬───┬───┬───┐
  1   2   3   4   5   6
      └───┼───┴──────┘│
          └───────────┘
  ```

- A partially ordered set (poset): a binary relation \\( \leq \\) over a set \\( S \\) s.t. for \\( a,b,c \in S \\):

    - \\( a \leq a \\)
    - \\( a \leq b,~b \leq a \implies a = b \\)
    - \\( a \leq b,~b \leq c \implies a \leq c \\)

    also corresponds to a category.

Some examples of categories from mathematics:

| Name     | Objects            | Arrows                 |
|----------|--------------------|------------------------|
| **Set**  | sets               | maps                   |
| **Top**  | topological spaces | continuous functions   |
| **Vect** | vector spaces      | linear transformations |
| **Grp**  | groups             | group homomorphisms    |

In all these cases, arrows correspond to functions, although this is by no means required. All these categories correspond to objects from mathematics, along with *structure preserving maps*. **Set** will be of primary interest when we start talking about concrete applications to programming.

## Functors

A functor is a map between categories. This means it sends objects to objects, and arrows to arrows.

**Definition**: A *functor* \\( T \\) between categories \\( C \\) and \\( D \\) consists of two functions (both denoted simply by \\( T \\)):

- An *object function* that maps objects \\( a \in C \\): \\( a \mapsto Ta \in D \\)
- An *arrow function* that assigns to each arrow \\( f: a \to b \\) in \\( C \\) an arrow \\( Tf: Ta \to Tb \\) in \\( D \\), such that:

$$T(\text{id}\_a) = \text{id}_{Ta},~T(g \circ f) = Tg \circ Tf.$$

A functor is a very powerful concept, since it allows you to translate between different branches of mathematics! They also play an important role in functional programming where among many other things, they are useful for defining _container types_ or more generally _type constructors_.

Functors can be composed, and this allows one to define a category of categories **Cat**, where the arrows are functors.

### Examples of functors

* The identity functor: \\( \text{id}\_{C}: C \to C \\) is defined as:
\begin{align\*}
\text{id}_{C}:~&a \mapsto a\\
&f \mapsto f
 \end{align\*}

* The constant functor \\( \Delta\_d: C \to D \\) for fixed \\( d \in D \\):
\begin{align\*}
\Delta\_{d}:~&a \mapsto d\\
&f \mapsto \text{id}_d
 \end{align\*}

* The _power-set functor_: \\( P: \\) **Set** \\( \to \\) **Set** sends subsets to their image under maps. Let \\( A, B \in \\) **Set**, \\( f: A \to B \\) and \\( S \subset A \\):
\begin{align\*}
PA &= P(A),\\\\
Pf&: P(A) \to P(B),~S \mapsto f(S)
\end{align\*}

* From many categories representing 'sets with added structure' (groups, vector spaces, rings, topological spaces, ...) there is a *forgetful functor* going to **Set**, where objects are sent to their underlying sets.

    As an additional example, there is also a forgetful functor \\( F: \mathbf{Cat} \to \mathbf{Graph} \\), sending each category to the graph defined by its objects and arrows.

* Dual-set functor

\begin{align\*}
*&: \textbf{Vect} \to \textbf{Vect}\\\\
&: W \mapsto W^\*\\\\
&: (f: V \to W) \mapsto (f^\*: W^\* \to V^\*)
\end{align\*}

This is an example of a _contravariant functor_ (a functor from \\( \textbf{Vect} \\) to \\( \textbf{Vect}^{\text{op}} \\), the category with reversed arrows and composition rules.

## Special objects, arrows and functors

### Terminal and initial objects

For objects, we distuinguish two special kinds:

**Definition**. An object \\( x \in C \\) is **terminal** if for all \\( a \in C \\) there is exactly one arrow \\( a \to x \\). Similarly, it is **initial** if there is exactly one arrow \\( x \to a \\) to all objects.


```
┌─────┐
│  ┌─►a──┐
│  │     ▼
└─►i ──► t─┐
   │     ▲ │
   └─►b──┘ │
      ▲    │
      └────┘
```

Here, \\( i \\) is initial, and \\( t \\) is terminal. \\( a \\) and \\( b \\) are neither initial nor terminal.

### Epi, mono, and iso arrows

There are a number of special kind of arrows:

**Definition**. An arrow \\( f: a \to b \in C \\) is a **monomorphism** (or simply mono), if for all objects \\( x \\) and all arrows \\( g, h: x \to a \\) and \\( g \neq h \\) we have:
$$f \circ g \neq f \circ h.$$

To put this into perspective, we show that in the category **Set** monomorphisms correspond to injective functions;


**Theorem.** In \\( \textbf{Set} \\) a map \\( f \\) is mono if and only if it is an injection.

_proof_. Let \\( f: A \to B \\). Suppose \\( f \\) is injective, and let \\( g, h: X \to A \\). If \\( g \neq h \\), then \\( g(x) \neq h(x) \\) for some \\( x \\). But since \\( f \\) is injective, we have \\( f(g(x)) \neq f(h(x)) \\), and hence \\( f \circ g \neq f \circ h \\), thus \\( f \\) is mono.

For the contrary, suppose \\( f \\) is mono. Let \\( \\{ * \\} \\) be the set with a single element. Then for \\( x \in A \\) we have an arrow \\( \\{ * \\} \to A \\) corresponding to the constant function \\( \tilde{x}(\*) = x \\), then \\( f \circ \tilde{x}(\*) = f(x) \\). Let \\( x \neq y \\). Since \\( f \\) is mono, \\( (f \circ \tilde{x})(\*) \neq (f \circ \tilde{y})(\*) \\), and hence \\( f(x) \neq f(y) \\), thus \\( f \\) is an injection.

There is also an generalization of the notion of _surjections_.

**Definition**.An arrow \\( f: a \to b \in C \\) is an **epimorphism** (or simply epi), if for all objects \\( x \\) and all arrows \\( g, h: b \to x \\) we have:
$$g \circ f = h \circ f \implies g = h.$$

Finally, we introduce the notion of an 'invertible arrow'.

**Definition**. An arrow \\( f: a \to b \in C \\) is an **isomorphism** if there exists an arrow \\( g: b \to a \\) so that:
$$g \circ f = \text{id}_a~\text{ and }~f \circ g = \text{id}_b.$$

In set, **epi** and **mono** imply **iso**. This however does not hold for all categories!

### Special functors

Lastly, we turn our attention to special kinds of functors. For this we first introduce the notion of a _hom-set_ of \\( a \\) and \\( b \\), the set of all arrows from \\( a \\) to \\( b \\):
$$\text{Hom}_C(a, b) = \\{ f \in C~|~f: a \to b \\}.$$

**Definition**. A functor \\( F: C \to D \\) is full if for all pairs \\( a, b \in C \\) the induced function:
\begin{align*}
F:~\text{Hom}_C(a, b) &\to \text{Hom}_D(Fa, Fb),\\
   f &\mapsto Ff
\end{align*}
is a surjection. It is called **faithful** if it is an injection.

When after applying \\( F \\) an arrow \\( Ff \\) or an object \\( Fa \\) has a certain property (i.e. being initial, terminal or epi, mono), it is implied that \\( f \\) (or \\( a \\)) had this property, then we say the \\( F \\) _reflects_ the property.

This allows for statements such as this:

**Theorem**. A faithful functor reflects epis and monos.

_proof_. As an example we will prove it for an \\( Ff \\) that is mono. Let \\( f: a \to b \\) such that \\( Ff \\) is mono, and let \\( h,g: x \to a \\) such that \\( h \neq g \\).

```
    g
   ──►    f
 x ──► a ──► b
 │  h  │     │
F│    F│    F│
 ▼ Fg  ▼     ▼
Fx ──► Fa ──►Fb
   ──►    Ff
   Fh
```

Since \\( g \neq h \\) and \\( F \\) is faithful, we have \\( Fg \neq Fh \\). This implies, because \\( Ff \\) is mono, that \\( Ff \circ Fg \neq Ff \circ Fh \\), and since \\( F \\) is a functor we have \\( F(f \circ g) \neq F(f \circ h) \\), implying \\( f \circ g \neq f \circ h \\), and hence \\( f \\) is mono.

## Natural transformations

**Definition.** A **natural transformation** \\( \mu \\) between two functors \\( F, G: C \to D \\) is a family of morphisms:
$$\mu = \\{ \mu_a: Fa \to Ga~|~a \in C \\},$$
indexed by objects in \\( C \\), so that for all morphisms \\( f: a \to b \\) the diagram

```
    ua
 Fa ──► Ga
  │     │
Ff│     │Gf
  ▼     ▼
 Fb ──► Gb
    ub
```

commutes. This diagram is called the _naturality square_.  We write \\( \mu: F \Rightarrow G \\), and call \\( \mu_a \\) _the component of \\( \mu \\) at \\( a \\)_.

We can _compose_ natural transformations, turning the set of functors from \\( C \to D \\) into a category. Let \\( \mu: F \Rightarrow G \\) and \\( \nu: G \Rightarrow H \\), then we have \\( \nu \circ \mu: F \Rightarrow H \\) defined by (in components):
$$(\nu \circ \mu)_a = \nu_a \circ \mu_a.$$
Where the composition of the rhs is simply composition in \\( D \\).

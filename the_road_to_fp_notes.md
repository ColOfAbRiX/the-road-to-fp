# The road to FP

I never studied functional programming at university and until few years ago it was just a fancy
word that I read on some nice blogs online. But then I  discovered Scala and I was captured by it
and I started to look around and study. But I quickly found out that information is very spread
around, blogs, videos, papers, books, librarie and so on and I never found a place that told me what
I was supposed to learn and what steps would come next.

This project is born from a personal struggle where I had to find my way to learn functional
programming. I thought it would be very useful to share the knowledge I built about *learning*
functional programming.

What I want to achieve with this project is to put together *in a structured way* the information
that is already publicly available and to top it up with my personal comments, notes and ideas and
with examples and exercises to make the life of people that want to get into FP easier.

I want to use a step-by-step approach where I guide the reader from no prior knowledge of FP to
get the grasp of the most advanced topics. I will introduce concepts little by little, providing
understanding and intuition behind each of them and also what real world problem they solve. I will
only assume that the reader is already well grounded in other programming paradigms.

At the moment this page is just a dump of raw ideas of what to put in the various chapters and to
have a rough organization of the content. The topics below will have to be properly expanded, with
references, comments, examples and exercises.

## Target of this content

* To learn functional programming applied to Scala
* To be able to understand the existing online materials
* To be able to use the main FP libraries
* To have an intuition of the theory behind FP
* To become fluent in the majority of FP concepts, tools and techniques

## Different way of thinking

* Put aside all other paradigms, you'll need to learn afresh
* Paradigm shift: tons of new concepts, structures, techniques, abstractions
* Declarative programming, cleaner way of thinking
  * https://bartoszmilewski.com/2015/04/15/category-theory-and-declarative-programming/
* Abstract over implementation details
* Ease of reasoning with black-box approach
* FP referential transparency makes things cumbersome. Requirements of new tools
* The unfortunate way of learning FP in Scala: Haskell, few books, blog posts

## Scala language feature: Case classes

* What they add to normal classes
* Pattern matching with case classes
* How to define values with types
* Build data types with case classes
* Best practices (like sealed traits, ...)
  * https://nrinaudo.github.io/scala-best-practices/tricky_behaviours/
  * https://medium.com/@cachiama/demystifying-scala-case-classes-b4d756959dcd
  * http://www.alessandrolacava.com/blog/scala-case-classes-in-depth/

## Definition of pure functions and referential transparency

* Simple examples of both concepts and counter examples
* Show that loops are not good, they require mutability
* Local mutabilty can be used
* Importance of pure function signatures
* Working with expressions
* Generating random numbers
* Memoization, or values as functions
* References:
  * https://www.manning.com/books/functional-programming-in-scala Chapter 1
  * https://www.wikiwand.com/en/Referential_transparency
  * https://alvinalexander.com/scala/fp-book/benefits-of-pure-functions
  * https://sidburn.github.io/blog/2016/03/14/immutability-and-pure-functions

## Recursion

* Solves the lack of loops
  * https://sidburn.github.io/blog/2016/04/05/mutable-loops-to-immutability
* Recursion is the equivalent of induction reasoning
* Examples: list length, take nth element, flatten list, map, flatMap
* Tail recursion. But it doesn't solve all problems!
* Recursion from two different functions
  * https://medium.com/@olxc/trampolining-and-stack-safety-in-scala-d8e86474ddfa
* References:
  * http://learnyouahaskell.com/recursion
  * http://aperiodic.net/phil/scala/s-99/
  * https://www.scala-exercises.org/fp_in_scala/getting_started_with_functional_programming
  * https://alvinalexander.com/scala/fp-book/section-scala-recursion-recursive-programming

## Scala language feature: Lazyness

* Lazy val
* By-name parameters
  * https://alvinalexander.com/scala/fp-book/how-to-use-by-name-parameters-scala-functions
* Streams as infinite lists
  * https://www.youtube.com/watch?v=cahvyadYfX8&t=741s
* Lazyness is like functions as values
* References:
  * https://www.manning.com/books/functional-programming-in-scala Chapter 5
  * https://www.scala-exercises.org/scala_tutorial/lazy_evaluation

## Purely functional data structures

* FP as functions acting on data, no objects around
* Sharing structure
  * https://vimeo.com/20262239
* Techniques to work with immutable data structures
* Example: Linked list, simple tree
* Spatial hash map
  * https://github.com/ColOfAbRiX/tankwar/blob/master/src/main/scala/com/colofabrix/scala/geometry/collections/SpatialHash.scala
* References:
  * https://sidburn.github.io/blog/2016/03/14/immutability-and-pure-functions
  * https://www.cs.cmu.edu/~rwh/theses/okasaki.pdf

## Scala language feature: Type classes (or classes of types)

* Type constructors
* Ad-hoc polymorphism
* Simple examples
* Proof that a kind exists
* Recursive resolution of type classes (with implicits scopes)
* References:
  * http://learnyouahaskell.com/types-and-typeclasses
  * https://gigiigig.github.io/tlp-step-by-step/implicits-resolution.html
  * https://danielwestheide.com/blog/2013/02/06/the-neophytes-guide-to-scala-part-12-type-classes.html
  * http://learnyouahaskell.com/making-our-own-types-and-typeclasses
  * https://alvinalexander.com/scala/fp-book/type-classes-signpost

## Scala language feature: Higher order functions

* Polymorphic functions
* The type keyword and fixing one type parameter
* Functions as objects that can be passed around
  * https://alvinalexander.com/scala/fp-book/how-write-functions-take-function-input-parameters
* Composition
* Currying shift the way we interpret function parameters
  * https://alvinalexander.com/scala/fp-book/partially-applied-functions-currying-in-scala
* References:
  * https://www.manning.com/books/functional-programming-in-scala Chapter 2
  * http://learnyouahaskell.com/higher-order-functions

## Functors and the world of containers

* Mapping of values in the container
  * http://learnyouahaskell.com/making-our-own-types-and-typeclasses#the-functor-typeclass
  * https://fsharpforfunandprofit.com/posts/elevated-world/#map
* Why do I want a container?
* Mapping doesn't change the shape of the container
* Mapping as lifting of functions
* Simple example with List+map as functor
* Exceptions and null are evil. Option+map as functor
  * https://sidburn.github.io/blog/2016/03/20/null-is-evil
* References:
  * http://learnyouahaskell.com/functors-applicative-functors-and-monoids
  * https://fsharpforfunandprofit.com/posts/elevated-world/

## Monoids

* What they are, mempty and mappend
* Examples that generalize some operations like log addition
  * https://typelevel.org/cats/typeclasses/monoid.html
* Create new instances of monoid with type classes
* Collapse a list with fold and reduce
  * https://typelevel.org/cats/typeclasses/monoid.html
* Show another example like Int and summing prices of items
* References:
  * http://learnyouahaskell.com/functors-applicative-functors-and-monoids

## Applicative functors

* What are applicatives
  * http://learnyouahaskell.com/functors-applicative-functors-and-monoids#applicative-functors
* Apply as lifting funtions and pipelining
  * https://fsharpforfunandprofit.com/posts/elevated-world/#apply
* Validating errors in a pipeline
* Traverse and sequence
  * https://fsharpforfunandprofit.com/posts/elevated-world-4/
  * https://sidburn.github.io/blog/2016/04/14/sequence-and-traverse
* References:
  * http://learnyouahaskell.com/functors-applicative-functors-and-monoids
  * http://adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html

## Optics

* Updating data structures by creating copies
* Lenses
  * https://scalac.io/scala-optics-lenses-with-monocle/
  * http://adit.io/posts/2013-07-22-lenses-in-pictures.html
* Prisms
* References:
  * https://medium.com/zyseme-technology/functional-references-lens-and-other-optics-in-scala-e5f7e2fdafe
  * https://www.schoolofhaskell.com/school/to-infinity-and-beyond/pick-of-the-week/a-little-lens-starter-tutorial
  * https://en.wikibooks.org/wiki/Haskell/Lenses_and_functional_references
  * https://www.scala-exercises.org/monocle/lens
  * http://adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html

## Introducing monads

* The need for sequencing elaborations
  * https://stackoverflow.com/questions/28139259/why-do-we-need-monads?rq=1
* Keep a log of function calls
* Generate a sequence of random numbers
* Operate on multiple values at the same time
* Generalizing the composition of operations
  * https://fsharpforfunandprofit.com/posts/elevated-world-2/#bind
* Railway oriented programming
  * https://fsharpforfunandprofit.com/rop/
* Scala for-comprehensions and syntactic sugar for flatMap, map and filter
  * https://alvinalexander.com/scala/fp-book/quick-review-scala-for-expressions
* Writing your own LeMonad
* References:
  * http://blog.sigfpe.com/2006/08/you-could-have-invented-monads-and.html
  * https://stackoverflow.com/a/13388966/1215156
  * http://learnyouahaskell.com/a-fistful-of-monads
  * http://learnyouahaskell.com/for-a-few-monads-more
  * http://adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html

## Most common monads

* Basic monads and examples: List, Option, Either, Writer, Try
  * https://www.scala-exercises.org/fp_in_scala/handling_error_without_exceptions
* Evaluated-values monads and examples: Reader, Future, Eval/Task
* Advanced monads and examples: IO, Trampoline, Cont
  * https://medium.com/@olxc/trampolining-and-stack-safety-in-scala-d8e86474ddfa
* Real world examples of using these monads, writing a more complex application
* Applicatives vs Monads
  * https://fsharpforfunandprofit.com/posts/elevated-world-3/#validation
* References:
  * http://learnyouahaskell.com/for-a-few-monads-more
  * http://adit.io/posts/2013-06-10-three-useful-monads.html

## Monad transformers

* What problems they solve
  * https://blog.buildo.io/monad-transformers-for-the-working-programmer-aa7e981190e7?gi=e7395015ecf9
* They work by unpacking and repacking containers
* Examples with OptionT and ListT
  * https://typelevel.org/cats/datatypes/optiont.html
  * https://github.com/ColOfAbRiX/monad-study/blob/master/src/main/scala/com/colofabrix/scala/transformers/Examples.scala
* References
  * http://eed3si9n.com/learning-scalaz/Monad+transformers.html
  * http://book.realworldhaskell.org/read/monad-transformers.html

## Free applicatives and free monads

* Representing algorithms with structured data
* References
  * https://stackoverflow.com/a/13388966/1215156
  * http://eed3si9n.com/herding-cats/stackless-scala-with-free-monads.html
  * http://functionaltalks.org/2013/06/17/runar-oli-bjarnason-dead-simple-dependency-injection/
  * https://skillsmatter.com/skillscasts/3244-stackless-scala-free-monads

## Comonads

* Non-Empty-List comonad
* Store comonad
* Product comonad
* Stream comonad
* Explaining lenses as comonads
  * https://r6research.livejournal.com/23705.html
* Conway's game of life using Store
  * https://eli-jordan.github.io/2018/02/16/life-is-a-comonad/
  * https://chrispenner.ca/posts/conways-game-of-life
* References:
  * http://blog.higher-order.com/blog/2015/06/23/a-scala-comonad-tutorial/
  * https://bartoszmilewski.com/2017/01/02/comonads/

## Introduction to Category Theory

* Types and mathematics
  * https://bartoszmilewski.com/2014/11/24/types-and-functions/
* Definition of a category
* Mapping to types and other "things"
* What "structure" means in mathematics

## Basic concepts of Category Theory

* Product and Coproduct (union and sum types)
* Algebraic Data Types
* Functors in categories and relation to Scala objects. Examples of Functors
* Natural transformations and relation to Scala objects.
  * https://milessabin.com/blog/2012/05/10/shapeless-polymorphic-function-values-2/
  * https://typelevel.org/cats/datatypes/functionk.html
  * http://eed3si9n.com/learning-scalaz/Natural-Transformation.html
* Parametric polymorphism
  * https://milessabin.com/blog/2012/05/10/shapeless-polymorphic-function-values-2/
  * https://bartoszmilewski.com/2014/09/22/parametricity-money-for-nothing-and-theorems-for-free/
  * https://stackoverflow.com/questions/3071136/what-does-the-forall-keyword-in-haskell-ghc-do
* References:
  * http://danshiebler.com/2018-11-10-category-solutions/
  * https://medium.com/free-code-camp/demistifying-the-monad-in-scala-part-2-a-category-theory-approach-2f0a6d370eff

## Revisiting structures and tools using Category Theory

* How to understand Scala type system using category theory
* Representable functors
* Monads, Comonads and Adjuctions
* Lenses ad Comonad Coalgebras
  * https://r6research.livejournal.com/23705.html

## Few concepts in Type theory

* Kinds (as in values <-> types <-> kinds)
  * https://vimeo.com/channels/videosyo/28793245
  * https://stackoverflow.com/questions/6246719/what-is-a-higher-kinded-type-in-scala
  * http://ropas.snu.ac.kr/~bruno/papers/ScalaGeneric.pdf
* Ranks in Scala
  * https://stackoverflow.com/questions/13317768/kind-vs-rank-in-type-theory
  * https://apocalisp.wordpress.com/2010/07/02/higher-rank-polymorphism-in-scala/
  * https://www.stephanboyer.com/post/115/higher-rank-and-higher-kinded-types
* Kind projector
  * https://github.com/typelevel/kind-projector

## Type Level Programming

* The flexibility of dynamic types with the checkes of static types
* Type resolution in scala: implicits, type, aux pattern
* Example with Boolean/If
* Example with natural numbers
* HLists
* Map on HLists
  * https://milessabin.com/blog/2012/05/10/shapeless-polymorphic-function-values-2/
* References:
  * https://gigiigig.github.io/tlp-step-by-step/introduction.html
  * https://apocalisp.wordpress.com/2010/06/08/type-level-programming-in-scala/
  * https://www.youtube.com/watch?v=_-J4YRI1rAw7
  * https://www.youtube.com/watch?v=GKIfu1WtSz4

# The road to FP

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
  * <https://bartoszmilewski.com/2015/04/15/category-theory-and-declarative-programming/>
* Abstract over implementation details
* Ease of reasoning with black-box approach
* FP referential transparency makes things cumbersome. Requirements of new tools
* The unfortunate way of learning FP in Scala: Haskell, few books, blog posts
* Composability
  * Design and analysis approaches: blackbox, top-down, bottom-up
  * Composition of blocks
  * Programming languages and modularity

## Scala language feature: Case classes

* What they add to normal classes
  * <https://stackoverflow.com/questions/2312881/what-is-the-difference-between-scalas-case-class-and-class>
* Pattern matching with case classes
  * <https://docs.scala-lang.org/tutorials/tour/pattern-matching.html.html>
  * <https://alvinalexander.com/scala/how-to-use-pattern-matching-scala-match-case-expressions>
* How to define values with types
* Build data types with case classes
* Best practices (like sealed traits, ...)
  * <https://nrinaudo.github.io/scala-best-practices/tricky_behaviours/>
  * <https://medium.com/@cachiama/demystifying-scala-case-classes-b4d756959dcd>
  * <http://www.alessandrolacava.com/blog/scala-case-classes-in-depth/>

## Definition of pure functions and referential transparency

* Simple examples of both concepts and counter examples
* Show that loops are not good, they require mutability
* Immutability as values that cannot change
* Local mutabilty can be used
* Importance of pure function signatures
* Working with expressions
* Generating random numbers
* Memoization, or values as functions
* References:
  * <https://www.manning.com/books/functional-programming-in-scala> Chapter 1
  * <https://www.wikiwand.com/en/Referential_transparency>
  * <https://alvinalexander.com/scala/fp-book/benefits-of-pure-functions>
  * <https://sidburn.github.io/blog/2016/03/14/immutability-and-pure-functions>

## Recursion

* Solves the lack of loops for primitives
  * <https://sidburn.github.io/blog/2016/04/05/mutable-loops-to-immutability>
* Recursion is the equivalent of induction reasoning
  * Rules of induction and how to translate them to programmi
* Examples: list length, take nth element, flatten list, map, flatMap
* Tail recursion. But it doesn't solve all problems!
* Recursion to iterators, important for safety:
  * <http://blog.moertel.com/tags/recursion-to-iteration%20series.html>
* Recursion from two different functions
  * <https://medium.com/@olxc/trampolining-and-stack-safety-in-scala-d8e86474ddfa>
* References:
  * <http://learnyouahaskell.com/recursion>
  * <http://aperiodic.net/phil/scala/s-99/>
  * <https://www.scala-exercises.org/fp_in_scala/getting_started_with_functional_programming>
  * <https://alvinalexander.com/scala/fp-book/section-scala-recursion-recursive-programming>

## Scala language feature: Lazyness

* Lazy val
* By-name parameters
  * <https://alvinalexander.com/scala/fp-book/how-to-use-by-name-parameters-scala-functions>
* Streams as infinite lists
  * <https://www.youtube.com/watch?v=cahvyadYfX8&t=741s>
* Lazy collections and how they can improve performance
* Lazyness is like functions as values
* References:
  * <https://www.manning.com/books/functional-programming-in-scala> Chapter 5
  * <https://www.scala-exercises.org/scala_tutorial/lazy_evaluation>

## Purely functional data structures

* FP as functions acting on data, no objects around
* Sharing structure
  * <https://vimeo.com/20262239>
* Techniques to work with immutable data structures
* Example: Linked list, simple tree
* Spatial hash map
  * <https://github.com/ColOfAbRiX/tankwar/blob/master/src/main/scala/com/colofabrix/scala/geometry/collections/SpatialHash.scala>
* References:
  * <https://sidburn.github.io/blog/2016/03/14/immutability-and-pure-functions>
  * <https://www.cs.cmu.edu/~rwh/theses/okasaki.pdf>

## Scala language feature: Higher order functions

* Polymorphic functions
* The type keyword and fixing one type parameter
* Functions as objects that can be passed around
  * <https://alvinalexander.com/scala/fp-book/how-write-functions-take-function-input-parameters>
* Composition of function (andThen, orElse)
* Currying shift the way we interpret function parameters
  * <https://alvinalexander.com/scala/fp-book/partially-applied-functions-currying-in-scala>
* References:
  * <https://www.manning.com/books/functional-programming-in-scala> Chapter 2
  * <http://learnyouahaskell.com/higher-order-functions>

## Functors and the world of containers

* Mapping of values in the container
  * <http://learnyouahaskell.com/making-our-own-types-and-typeclasses#the-functor-typeclass>
  * <https://fsharpforfunandprofit.com/posts/elevated-world/#map>
* Why do I want a container?
* Mapping doesn't change the shape of the container
* Mapping as lifting of functions
* Simple example with List+map as functor
* Covariant and contravariant functors
  * <https://ocharles.org.uk/blog/guest-posts/2013-12-21-24-days-of-hackage-contravariant.html>
  * <https://kubuszok.com/2018/the-f-words-functors-and-friends/#functor>
* Exceptions and null are evil. Option+map as functor
  * <https://sidburn.github.io/blog/2016/03/20/null-is-evil>
* References:
  * <https://medium.com/@lettier/your-easy-guide-to-monads-applicatives-functors-862048d61610>
  * <http://learnyouahaskell.com/functors-applicative-functors-and-monoids>
  * <https://fsharpforfunandprofit.com/posts/elevated-world/>

## Scala language feature: Typeclasses

* The expression problem
  * <https://www.wikiwand.com/en/articles/Expression_problem>
* Type constructors
* Forms of polymorphism: type polymorphism, inclusion polymorphism ad-hoc polymorphism
  * <https://slides.com/petrabierleutgeb/polymorphism-in-scala-scaladays19>
* Simple examples of typeclasses
* The materializer function
* View bound and context bounds
* Proof that a kind exists (as in proof that a type has the X typeclass)
* Creating a typeclass for Functors
* Recursive resolution of type classes (with implicits scopes)
* References:
  * <http://learnyouahaskell.com/types-and-typeclasses>
  * <https://gigiigig.github.io/tlp-step-by-step/implicits-resolution.html>
  * <https://danielwestheide.com/blog/2013/02/06/the-neophytes-guide-to-scala-part-12-type-classes.html>
  * <http://learnyouahaskell.com/making-our-own-types-and-typeclasses>
  * <https://alvinalexander.com/scala/fp-book/type-classes-signpost>

## Monoids

* What they are, mempty and mappend
* Examples that generalize some operations like log addition
  * <https://typelevel.org/cats/typeclasses/monoid.html>
* Create new instances of monoid with type classes
* Collapse a list with fold and reduce
  * <https://typelevel.org/cats/typeclasses/monoid.html>
* Show another example like Int and summing prices of items
* Exercise: create a typeclass for Monoids and functions that work with them
* References:
  * <http://learnyouahaskell.com/functors-applicative-functors-and-monoids>

## Applicative functors

* What are applicatives
  * <http://learnyouahaskell.com/functors-applicative-functors-and-monoids#applicative-functors>
  * <https://kubuszok.com/2018/the-f-words-functors-and-friends/#functor>
* Apply as lifting funtions and pipelining
  * <https://fsharpforfunandprofit.com/posts/elevated-world/#apply>
* Validating errors in a pipeline
  * <https://sihil.net/cats-validated.html>
* Indipendent computations and parallel processing
  * <https://alexn.org/blog/2017/01/30/asynchronous-programming-scala.html#h6-2>
* Traverse and sequence
  * <https://fsharpforfunandprofit.com/posts/elevated-world-4/>
  * <https://sidburn.github.io/blog/2016/04/14/sequence-and-traverse>
* References:
  * <http://learnyouahaskell.com/functors-applicative-functors-and-monoids>
  * <http://adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html>

## Optics

* Updating data structures by creating copies
* Lenses
  * <https://github.com/jdegoes/lambdaconf-2014-introgame>
  * <https://scalac.io/scala-optics-lenses-with-monocle/>
  * <http://adit.io/posts/2013-07-22-lenses-in-pictures.html>
* Prisms
* References:
  * <https://medium.com/zyseme-technology/functional-references-lens-and-other-optics-in-scala-e5f7e2fdafe>
  * <https://www.schoolofhaskell.com/school/to-infinity-and-beyond/pick-of-the-week/a-little-lens-starter-tutorial>
  * <https://en.wikibooks.org/wiki/Haskell/Lenses_and_functional_references>
  * <https://www.scala-exercises.org/monocle/lens>
  * <http://adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html>

## Introducing monads

* The need for sequencing elaborations
  * <https://stackoverflow.com/questions/28139259/why-do-we-need-monads?rq=1>
* A brief history of monads: monads defined, haskell without I/O, eugenio moggi uses monads, philip wadler explains them
* How to keep a log of function calls
  * <http://blog.sigfpe.com/2006/08/you-could-have-invented-monads-and.html>
* Generate a sequence of random numbers
  * <http://blog.sigfpe.com/2006/08/you-could-have-invented-monads-and.html>
* Operate on multiple values at the same time
  * <http://blog.sigfpe.com/2006/08/you-could-have-invented-monads-and.html>
* Generalizing the composition of operations
  * <https://fsharpforfunandprofit.com/posts/elevated-world-2/#bind>
* Railway oriented programming
  * <https://fsharpforfunandprofit.com/rop/>
* Scala for-comprehensions and syntactic sugar for flatMap, map and filter
  * <https://alvinalexander.com/scala/fp-book/quick-review-scala-for-expressions>
* Writing your own LeMonad
* References:
  * <https://github.com/jdegoes/lambdaconf-2014-introgame>
  * <https://stackoverflow.com/a/13388966/1215156>
  * <http://learnyouahaskell.com/a-fistful-of-monads>
  * <http://learnyouahaskell.com/for-a-few-monads-more>
  * <http://adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html>

## Most common monads

* Intuitive monads: List, Option
* Error handling monads: Option, Either, Try
  * <https://www.scala-exercises.org/fp_in_scala/handling_error_without_exceptions>
* Configuration and state monads: Reader and State
* Evaluated-values monads and examples: Reader, Future and the IO monad
  * <https://www.beyondthelines.net/programming/cats-effect-an-overview/>
* Advanced monads and examples: Trampoline, Cont
  * <https://medium.com/@olxc/trampolining-and-stack-safety-in-scala-d8e86474ddfa>
* Real world examples of using these monads, writing a more complex application
* Applicatives vs Monads
  * <https://fsharpforfunandprofit.com/posts/elevated-world-3/#validation>
* References:
  * <http://learnyouahaskell.com/for-a-few-monads-more>
  * <http://adit.io/posts/2013-06-10-three-useful-monads.html>

## Handling errors in FP

* Reviewing Option, Either and Validated
* Scala exceptions and the ErrorMonad
* Examples of the ErrorMonad with IO
  * <https://www.youtube.com/watch?v=KQZjOJjnHIE>

## Monad transformers

* What problems they solve
  * <https://blog.buildo.io/monad-transformers-for-the-working-programmer-aa7e981190e7?gi=e7395015ecf9>
* They work by unpacking and repacking containers
* Examples with OptionT and ListT
  * <https://typelevel.org/cats/datatypes/optiont.html>
  * <https://github.com/ColOfAbRiX/monad-study/blob/master/src/main/scala/com/colofabrix/scala/transformers/Examples.scala>
* Examples with with EitherT and ReaderT
* References
  * <http://eed3si9n.com/learning-scalaz/Monad+transformers.html>
  * <http://book.realworldhaskell.org/read/monad-transformers.html>

## Free applicatives and free monads

* Representing algorithms with structured data
* Building a configuration system with free applicatives
  * <https://www.youtube.com/watch?v=H28QqxO7Ihc>
* Composing free monads
  * <https://underscore.io/blog/posts/2017/03/29/free-inject.html>
  * <https://perevillega.com/understanding-free-monads>
* References
  * <https://degoes.net/articles/modern-fp>
  * <https://stackoverflow.com/a/13388966/1215156>
  * <http://eed3si9n.com/herding-cats/stackless-scala-with-free-monads.html>
  * <http://functionaltalks.org/2013/06/17/runar-oli-bjarnason-dead-simple-dependency-injection/>
  * <https://skillsmatter.com/skillscasts/3244-stackless-scala-free-monads>
  * <https://www.youtube.com/watch?v=U0lK0hnbc4U>

## Tagless final

* An alternative to free monads to abstract over dependencies and implementation s
* <https://rockthejvm.com/articles/tagless-final-in-scala>
* <https://nrinaudo.github.io/articles/tagless_final.html>
* <https://www.reddit.com/r/scala/comments/s6ih9p/can_you_give_me_a_brief_understanding_on_tagless/>
* Drawbacks and problems
  * <https://degoes.net/articles/tagless-horror>

## Comonads

* Non-Empty-List comonad
* Store comonad
* Product comonad
* Stream comonad
* Explaining lenses as comonads
  * <https://r6research.livejournal.com/23705.html>
* Conway's game of life using Store
  * <https://eli-jordan.github.io/2018/02/16/life-is-a-comonad/>
  * <https://chrispenner.ca/posts/conways-game-of-life>
* Cofree Comonads
  * <https://stackoverflow.com/questions/38816993/what-are-some-motivating-examples-for-cofree-comonad-in-haskell>
* References:
  * <http://blog.higher-order.com/blog/2015/06/23/a-scala-comonad-tutorial/>
  * <https://bartoszmilewski.com/2017/01/02/comonads/>
  * <https://stackoverflow.com/questions/16551734/can-a-monad-be-a-comonad>

## Recursion Schemes

* Practical introduction
  * <https://www.youtube.com/watch?v=ahX2l4GcOwI&t=1136s>
  * <https://nrinaudo.github.io/recursion-schemes-from-the-ground-up/#1>
  * <https://free.cofree.io/2017/11/13/recursion/>

## Introduction to Category Theory

* Types and mathematics
  * <https://bartoszmilewski.com/2014/11/24/types-and-functions/>
  * <https://corecursive.com/category-theory-is-how-our-minds-work-with-bartosz-milewski/>
* Definition of a category
* Mapping to types and other "things"
* What "structure" means in mathematics
* Product and Coproduct (union and sum types)
* Algebraic Data Types
* References
  * <https://www.youtube.com/watch?v=mqCsfYERzzE>

## Basic concepts of Category Theory

* Functors in categories and relation to Scala objects. Examples of Functors
* Natural transformations and relation to Scala objects.
  * <https://milessabin.com/blog/2012/05/10/shapeless-polymorphic-function-values-2/>
  * <https://typelevel.org/cats/datatypes/functionk.html>
  * <http://eed3si9n.com/learning-scalaz/Natural-Transformation.html>
  * <https://lukepalmer.wordpress.com/2008/04/28/whats-a-natural-transformation/>
* Parametric polymorphism
  * <https://milessabin.com/blog/2012/05/10/shapeless-polymorphic-function-values-2/>
  * <https://bartoszmilewski.com/2014/09/22/parametricity-money-for-nothing-and-theorems-for-free/>
  * <https://stackoverflow.com/questions/3071136/what-does-the-forall-keyword-in-haskell-ghc-do>
* Algebras, Coalgebras, Catamorphisms
  * <https://stackoverflow.com/questions/16015020/what-does-coalgebra-mean-in-the-context-of-programming>
  * <https://blog.hablapps.com/2016/10/11/yo-dawg-we-put-an-algebra-in-your-coalgebra/>
  * <https://blog.hablapps.com/2017/02/20/algebras-for-the-masses/>
  * <https://medium.com/@olxc/catamorphisms-and-f-algebras-b4e91380d134>
  * <https://blog.hablapps.com/2016/10/11/yo-dawg-we-put-an-algebra-in-your-coalgebra/>
* References:
  * <http://danshiebler.com/2018-11-10-category-solutions/>
  * <https://medium.com/free-code-camp/demistifying-the-monad-in-scala-part-2-a-category-theory-approach-2f0a6d370eff>

## Revisiting structures and tools using Category Theory

* How to understand Scala type system using category theory
* Representable functors, optimization
* Folding
  * <https://softwaremill.com/beautiful-folds-in-scala/>
* Monoids, definition, generalization, examples
  * <https://apocalisp.wordpress.com/2010/06/14/on-monoids/>
  * <https://apocalisp.wordpress.com/2010/07/21/more-on-monoids-and-monads/>
* Adjunctions and definition of Monads and Comonads
* Free monoids and free monads
  * <http://blog.higher-order.com/blog/2013/08/20/free-monads-and-free-monoids/>
  * <https://bartoszmilewski.com/2015/07/21/free-monoids/>
* Free monads and Yoneda
  * <https://underscore.io/blog/posts/2015/04/14/free-monads-are-simple.html>
  * <https://underscore.io/blog/posts/2017/03/29/free-inject.html>
  * <http://blog.higher-order.com/blog/2013/11/01/free-and-yoneda/>
  * <https://medium.com/@olxc/yoneda-and-coyoneda-trick-f5a0321aeba4>
  * <https://bartoszmilewski.com/2013/05/15/understanding-yoneda/>
  * <https://kubuszok.com/2018/the-f-words-functors-and-friends/#functor>
* Lenses and Comonad Coalgebras
  * <https://r6research.livejournal.com/23705.html>

## Few concepts in Type theory

* Types
  * <https://kubuszok.com/2018/kinds-of-types-in-scala-part-1/>
* Kinds (as in values <-> types <-> kinds)
  * <https://vimeo.com/channels/videosyo/28793245>
  * <https://stackoverflow.com/questions/6246719/what-is-a-higher-kinded-type-in-scala>
  * <http://ropas.snu.ac.kr/~bruno/papers/ScalaGeneric.pdf>
* Ranks in Scala
  * <https://stackoverflow.com/questions/13317768/kind-vs-rank-in-type-theory>
  * <https://apocalisp.wordpress.com/2010/07/02/higher-rank-polymorphism-in-scala/>
  * <https://www.stephanboyer.com/post/115/higher-rank-and-higher-kinded-types>
* Kind projector
  * <https://github.com/typelevel/kind-projector>

## Type Level Programming

* Overcoming type erasure
  * <https://medium.com/@sinisalouc/overcoming-type-erasure-in-scala-8f2422070d20>
* How the compiler does type resolution and infers missing parameters
* Dependent types and the Aux pattern
* Type resolution in scala: implicits, type
  * <https://www.youtube.com/watch?v=R8GksuRw3VI> (great video on dependent types and Aux pattern)
  * <https://karlcode.owtelse.com/blog/2017/04/11/the-rise-and-hopefully-fall-of-the-aux-pattern-2/?mode=doc#slide-0>
* Example of defining Boolean/If with types
* Example with natural numbers
* Shapeless basics: HLists, tuples, generics
* Working with types and HList is working with recursion and induction reasoning
* Map on HLists and polymorphic functions
  * <https://milessabin.com/blog/2012/05/10/shapeless-polymorphic-function-values-2/>
* Encoder/Decoder derivation with shapeless
  * <https://www.beyondthelines.net/programming/reducing-type-class-boilerplate-with-shapeless/>
  * <https://stackoverflow.com/questions/22418309/creating-a-shapeless-polymorphic-function-with-a-naked-type-param>
  * <https://blog.free2move.com/shapeless-hlists-and-how-to-traverse-them/>
  * <https://www.beyondthelines.net/programming/reducing-type-class-boilerplate-with-shapeless/>
  * <https://olegpy.com/traversing-hlists/>
* More on shapeless
  * <https://stackoverflow.com/questions/20912679/what-is-at-in-shapeless-scala>
* References:
  * <https://gigiigig.github.io/tlp-step-by-step/introduction.html>
  * <https://apocalisp.wordpress.com/2010/06/08/type-level-programming-in-scala/>
  * <https://www.youtube.com/watch?v=_-J4YRI1rAw7>
  * <https://www.youtube.com/watch?v=GKIfu1WtSz4>

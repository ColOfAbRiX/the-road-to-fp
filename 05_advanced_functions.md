# Advanced functions concepts

Estimated reading time: 10 minutes

After having introduced pure functions in the previous chapter, there are many more things to know
about them and how Scala works with functions.

## Parametricity and pure function signatures

For now I won't get into details on what parametricity is and I will assume the reader is familiar
with the Scala type parameters and more details can be read in [this excellent article in two
parts][3] that also talk about bounds, variance and subtyping.

If you think about it, functions with no inputs or functions that return `Unit` are always impure
functions because if they return `Unit` without any side effect then they won't be useful and
therefore they must have side effects. If they require no input then their output must come from a
side effect making it again impure pretty much by definition.

This is a very simple example of how the signature of a function can be of great interest in
functional programming, because they can tell us a lot and we can reason about them in a very
general way.

Function signatures are a very powerful and general tool that can be used to prove general
properties about the function you're analysing. This concept is much more general and it was
introduced by Philip Wadler in his paper [Theorems for free][4] and Tony Morris has [an excellent
and comprehensive set of slides][2] on why parametricity is an important very useful and how it can
be used. Amongst other things he argues that polymorphic functions can prove things about what a
function can or cannot do.

Let's see some examples to get an intuition of what I mean. Given a function with this signature:

```scala
def mickeymouse(n: Int): Int
```

how many implementations can you think of? There is a vast amount of functions that fit this
signature and we cannot say much about its content. The function could simply return always the same
number, it can do any kind of operation to the input number before spitting it out, it can contain
logic like `if` statements and so on. Let's pick one implementation like this other function, which
has the same signature as the previous one:

```scala
def doubleInput(n: Int): Int = if (n % 2 == 0) n * 2 else n * 3
```

Well, clearly this is a pure function and has a well defined body. But its name can't be trusted as
documentation! In fact trying to unit test this function might result in a lot of frustration!

Time to use parametricity to see what we gain. But also what we loose. Given this function
definition:

```scala
def donalduck[A](a: A): A
```

find its complete implementation.

When we start writing the body of the function we see that the function must return a value of type
`A`. How many ways we have to obtain a value of type `A` to return? Can we make it up? We can't
because we don't know what the actual type `A` is. In one call it might be an `Int` in another it
might be an instance of an object. Because we're creating a pure function we can't magically find
this value in a upper scope. From inside the function there is only one place where we can obtain an
`A` and that is in the variable `a` of type `A`. There's no other way.

So given the signature above we can be confident that the function's complete and correct
implementation is:

```scala
def identity[A](a: A): A = a
```

Let's try again with a different example but similar reasoning:

```scala
def unclescrooge[A, B](a: A, f: A => B): B
```

We can analyse this function again by starting from the output value and working only with what we
have available inside the function. The function has to return a `B` and we can obtain that value
from the result of the function `f`. `f` returns a value of type `B` but it requires a value of type
`A` to operate. Where can we get an `A`? From the value `a` that is also given as argument and the
final implementation becomes:

```scala
def apply[A, B](a: A, f: A => B): B = f(a)
```

These are simple examples where there is only one possible implementations while in the real word we
can have more possible implementations. Nonetheless the concept remains the same and this technique
is very powerful. I strongly encourage you to read the slides linked in the references and to work
on the following exercises.

## Exercises 5.1

### 5.1.1

Implements the functions with the following definitions. Note that there might be any number of
implementation, none, one or more (in this last case implement only a few):

```scala
def f[A](a: A, b: A): A
def g[A, B](a: A, b: B): A
def h[A, B](a: A, b: B): B
```

### 5.1.2

Implement the functions with the following definitions:

```scala
def f[A, B](as: Option[A]): Option[B]
def g[A, B](as: List[A]): List[B]
def h[A](as: List[A]): List[A]
def i[A, B](as: List[A], f: A => B): List[B]
```

How many implementations did you find? Why is that?

## Type inhabitants



## Scala encoding of functions

Functions as objects
Methods are different than functions
Eta expansion/lifting. Tupled functions
Scala PartialFunction and concatenation. Similarities with the chain of responsibility pattern

## Currying

## Closures

**WIP**
What are closures
How to use them
Examples and exercises
https://www.learningjournal.guru/article/scala/functional-programming/closures/

## Memoization, or values as functions

There is another way we can understand pure functions different from what we have talked so far. We
can think of pure functions are replacement machines that for every input value substitute an output
value (see later on the discussion about mathematical functions).

Using this point of view we can perform a neat trick: we can tabulate each result value of running a
function in a table that for every input it associates the relative output and/or we can store this
correspondence in a data structure.

Let's make an example and create a function to convert hexadecimal digits into decimal. The input
type of this function is the set of hexadecimal digits from the number `0` to the number `9` and
from letter `a` to letter `f`. The function we want to write converts each single digit into its
corresponding integer value. Such a function can be easily written as:

```scala
def hex2dec(s: String): Int = {
  Integer.parseInt(s, 16)
}

hex2dec("2")
hex2dec("b")

// Output:
//   Int = 2
//   Int = 11
```

We can think of a version where we tabulate all the possible input values and for each one we return
the corresponding value. In this way we are establishing a relation which is a function:

```scala
def hex2dec(s: String): Int = s.toLowerCase match {
  case "0" => 0
  case "1" => 1
  case "2" => 2
  case "3" => 3
  case "4" => 4
  case "5" => 5
  case "6" => 6
  case "7" => 7
  case "8" => 8
  case "9" => 9
  case "a" => 10
  case "b" => 11
  case "c" => 12
  case "d" => 13
  case "e" => 14
  case "f" => 15
}

hex2dec("a")

// Output:
//   Int = 10
```

Of course this is a useless example but in real world application we can have all sort of relations
and what I want to highlight here is that pure functions can actually be represented by simple data
and a relation.

This might seem a waste of time but this technique can be exploited to speed up the calculation of
expensive functions by building a dynamic table of the results as we go.

Let's say we have an expensive function, `expensiveOperation`, that takes a long time to perform its
job. We can wrap it inside another function, we then define a mutable dictionary that will map the
relation between inputs and output values and we place this dictionary inside an `object` to hide it
from the user. The resulting function will be pure because we make use of the concept of local
mutability so that our version of `expensiveOperation` is referentially transparent.

Every time we call the function with a given input `x` we look inside this dictionary and if there
is a key defined for `x`. If there is not we call the expensive function and we save the result in
the dictionary otherwise we return the result immediately:

```scala
object Calculations {
  import collection.mutable._

  private var calculateTable: HashMap[Int, Int] = HashMap.empty

  def calculate(n: Int): Int = {
    def expensiveOperation(x: Int): Int = {
      Thread.sleep(3000)
      x * x
    }
    calculateTable.getOrElseUpdate(n, expensiveOperation(n))
  }
}

Calculations.calculate(3)    // The first time you run this it will take 3 seconds
Calculations.calculate(3)    // This uses the memoized value and returns immediately

// Output:
//   Int = 9
//   Int = 9
```

This technique is called memoization and it's a powerful tool made possible by the use of pure
functions. Many times it's not as trivial as here to memoize a function because the values might be
infinite but this is not the place to get into more details.

## Pure functions and mathematics

With pure functions you have at your disposal mathematics that can help you talk about functions and
visualize what is happening and what the functions is doing because we can use this new tool to draw
diagrams of the functions.

In mathematics a function is a relation that given an input set and an output set (that can also be
the same) it associates element of the input set to elements of the output set and it never
associates elements in the input to more than one element in the output. We can **colloquially**
call this operation mapping and say that *a function maps elements of the input set into elements of
the output set*.

The input set is called **domain** of the function and the output set is called **codomain** of the
function.

If the function maps every element of the domain then the functions is called **total** otherwise it
is called **partial**.

If different input elements of a function are always associated with different output elements then
the function is called **injective**. Intuitively injective functions don't collapse input elements
in the output but they keep them separate.

If a function, after it has done its job of associating inputs elements into outputs elements, has
used all the elements of the codomain then the function is called **surjective**. Surjective
functions are different than injective so different input elements can be mapped to the same output
element.

If a function is both injective and surjective then it is called **bijective** or **isomorphic**.
This is a really important concept in mathematics because it tells us that that to every input
element there is one and only one output element associated to it and that all output elements are
associated with one and only one input element. In turn this gives us the power to go back and forth
from any input element to an output element and vice-versa. In other words it exists a functions
that reverses the original functions. Such a function is called **inverse**.

To summarize, this is the list of concepts we just talked about: domain, codomain, total, partial,
injective, surjective, isomorphic, inverse.

We don't need to go deeper than this and we'll use these concepts to visualize and to understand
what happens. We'll see similar things when we will talk about categories.

Pure functions allow us to use the same terminology in programming because they correspond to
mathematical functions. In programming we talk of input and output *types* instead of sets.

## Lambda Calculus and pure functions

**WIP**
What is lambda calculus and why it's important. Brief historical overview
Explain turing machines and equivalence to lambda calculus
Everything is a function so we can use function to build everything
Closures as free variables in lambda calculus

## Exercises 5.2

### 5.2.1

For each of the following questions tell the domain and codomain of the function, if the function is
total or partial and if it is surjective, injective or bijective:

1. a function `String => String` that associate each string with their reverse (e.g. "Scala" ->
  "alacS");

2. a function `Int => Int` that associates negative numbers with their positive value;

3. a function `Int => Int` that associates even numbers into their double;

### 5.2.2

Given a case class `case class Person(name: String, surname: String)` create a function `Person =>
(String, String)` that associates an instance of `Person` to a tuple that contains name and surname.
Is this function a bijection? If yes write also its inverse.

## References

* [What is a Closure?][1]
* [Parametricity - Types are documentation][2]
* [Scala type system: Parametrized types and variance, Part 1][3]
* [P.Wadler - Theorems for free][4]
* [Why is Function[-A1,…,+B] not about allowing any supertypes as parameters?][5]

[1]: https://www.learningjournal.guru/article/scala/functional-programming/closures/
[2]: http://data.tmorris.net/talks/parametricity/4985cb8e6d8d9a24e32d98204526c8e3b9319e33/parametricity.pdf
[3]: https://blog.codecentric.de/en/2015/03/scala-type-system-parameterized-types-variances-part-1/
[4]: https://people.mpi-sws.org/~dreyer/tor/papers/wadler.pdf
[5]: https://stackoverflow.com/questions/10603982/why-is-function-a1-b-not-about-allowing-any-supertypes-as-parameters

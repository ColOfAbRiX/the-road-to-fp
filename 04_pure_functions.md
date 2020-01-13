# Pure functions and referential transparency

## Pure functions

Pure functions are are the essence of functional programming.

Adapting a quote from the book "Functional Programming Simplified" from Alvin Alexandre, functional
programming are the techniques and methods that result from restricting ourselves to pure functions.

So, what is a pure function? It is a very simple machine as seen from the outside world because it's
a type of machine that need some stuff to work on and that spits out the result. We can imagine it
as closed box with three simple parts:

* the inputs, where you put the things the machine needs to work on;
* the engine (the body of the function), which plays with the input and the inputs only to produce
  an output;
* the output, which is some elaboration of the things given in the and the input only.

Note that "the engine" is not allowed to modify its inputs.

Here's a simple example in Scala:

```scala
def double(input: Int): Int = input * 2
def length(input: String): Int = input.length

double(3)
length("FP")

// Output:
//   Int = 6
//   Int = 2
```

It's very important to stress that the engine **works only on the things given as inputs and
produces a result that is only presented at its output**. There is another way to rephrase this same
concept that I see quote more often on the web which says that **the output of a function depends
only on its inputs**.

There is nothing else that the engine can do:

```Code
Inputs -> elaboration of the inputs into output -> Output
```

Why? What other things can a normal function do beside giving a result? Plenty.

Can you think of a function that does something other than returning a result? Let's see together
some examples.

```scala
// def println(x: Any): Unit = Console.println(x)
println("Hello, World!")

// Output:
//   Hello, World!
```

A function can print on the screen. That's not a value that it returns, actually `println` doesn't
return a value but just `Unit`. A function that returns `Unit` has always side effects or it won't
be a useful function.

```scala
// def readLine(): String = in.readLine()
val line = scala.io.StdIn.readLine()
```

A function that can read some text from the keyboard. This time the function returns something (what
has been read) but there is no input! The internal engine of the functions has no input to work with
and the output is produced in some other way... side effects! A function that produces an output
with an empty argument list is always an impure function or it won't do real work.

```scala
// public final Date getTime()
// NOTE: This is defined in java. But still no arguments.
import java.util.Calendar
Calendar.getInstance().getTime()

// Output:
//   java.util.Date = Sun Dec 22 00:54:55 CET 2019
```

A function that returns the current time is similar to the previous previous example where the
output doesn't depend only on the input. The function's engine didn't really work on the inputs to
produce an output.

```scala
// The list is empty, there is no head of the list
List().head

// Output:
//   java.util.NoSuchElementException: head of empty list
```

A sample function can work on a list to get the first element but then it throws an exception before
it can return the head. Yes, an exception is not a return value of a function, it's something else
that the function can do and that you can handle in some specific ways.

```scala
var state: Int = 0

def increment(amount: Int): Int = {
    state += 1
    state
}

increment(1)
increment(1)
increment(1)

// Output:
//   Int = 1
//   Int = 2
//   Int = 3
```

A function that works on a shared counter and increments this counter by a number and returns the
current counter value. This is not pure because several calls of the same method with the same
arguments return different values. Or in other words the output doesn't depend only on the inputs of
the functions but also on an internal state.

To summarize, some easy way to quickly understand if a function is impure is to check if:

* it returns `Unit` (it must do side effects to be useful);
* it requires no arguments (the output is produced );
* it can throw exceptions;
* there is an internal or shared state that the function uses.

This is not an exhaustive list but it's a starting point.

It's more difficult to reason with impure functions because they can do things that are not
contained in their box and there are plenty of bad things that can happen that you need to handle.
You need to handle exceptions. You need to reason about the interactions that the function calls
you're making do with other components of your system, you need to plan for network delays, you need
to check a file is present or your functions, you need to provide thread lock mechanisms if your
functions shares a state.

Alvin Alexander has a [great article about the benefits of pure functions][4] and I want to quote
here just the summary of his points:

> * They’re easier to reason about
> * They’re easier to combine
> * They’re easier to test
> * They’re easier to debug
> * They’re easier to parallelise
> * They are idempotent
> * They offer referential transparency
> * They are memoizable
> * They can be lazy

The article goes in much more details and I advise you to read it till the end because.

Pure functions are as close as we can get to mathematical functions in programming.

In mathematics the input and output are sets while in programming they are types. Using [the
description of Bartosz of types][8] "the simplest intuition for types is that they are sets of
values.". Scala `Boolean` is a set with two elements called `true` and `false`, `Int` is a set that
contains the integer that your JVM can represent, `String` is a set with infinite elements (limited
by the memory you assign you the JVM).

Then we use our programming language of choice to define how the output relates to the input, using
keywords, variables, calls to other functions and so on, in other terms we program the engine of the
function.

## Exercises 1

* Create a pure function `String => String` that associates each input string into its uppercase
  string.

* Create a pure function `Int => Boolean` that associates even numbers with the value `true` and odd
  numbers with the value `false`.

* Using your result of the exercise of animals in the case classes chapter, create a pure function
  with signature `Animal => String` that given an animal it returns the type of animal.

## On side effects

So we don't like side effects. Or do we?

Have you noticed that all the side effects mentioned above are somewhat related to interacting with
the world? That's because what side effects are, interactions with the world. So we need them, we
need interactions with the world, that's why we write programs in the first place, to do stuff! To
read and process files, to communicate on the network, to calculate a result and display it. The
reason to run a program is to have side effects!

Pure functions **do** no real-world work.

Yes, this is the catch, pure functions are difficult to work with and make our lives as developers
more unpredictable but we need them. Over the years scientists and programmers all around the world
have developed (and are still actively working on) a lot of techniques and tools to have side
effects that we can control and deal with.

Before these techniques were discovered and developed in Haskell, the researchers that built haskell
were having a very difficult time to just use pure functions and they didn't really know how
interact with the operating system, read files, execute commands! Have a look at the [interesting
story of the development of Haskell][6] to learn more about the development of FP.

I would like to say that functional programming is also about working with the tools and techniques
to work with side effects efficiently.

Right now is too early to introduce and talk about these tools and techniques, we need a bit more
background but you can already start assimilating the facts that:

* side effects are bad;
* pure functions have no side effects;
* we need side effects to interact with the world;
* thus we need tools and techniques to tightly control and handle side effects.

A very good resource to get an all round understanding of pure function is provided by [Chapter
1 of Functional Programming in Scala][2]

## Exercises 2

* We want to work with a counter and increment and decrement its value of a given amount. First
  solve this exercise in imperative style and then try to solve the same problem but using only pure
  functions. What's the problem you're encountering?

* Write a pure function that discovers the maximum integer in a list of **positive integers**
  `List[Int] => Int` and returns `-1` if the list is empty or if there are negative numbers or any
  other sort of issues. Solve the exercise using mutable variables and a for loop. Is it still a
  pure function? Why?

* How can you solve the previous exercise if we want the function to be able to work on a list that
  contains any integer and not just positive integers?

## Referential transparency

Referential transparency is a concept that is very tight to pure functions even it's strictly the
same.

Referential transparency is a property of some code that allows to replace an expression with the
result of evaluating that expression everywhere in the program without changing the result of the
program. Said the other way around, if you can replace a value with a reference to it (an
expression) then that expression is referentially transparent. An expression is a single unit of
code that has a value.

Being able to reason with the resulting value of a function is part of what makes FP simpler because
it takes away the details of how that value is obtained.

Let's see an example:

```scala
val a = 2
a + 3 * a
2 + 3 * 2

// Output:
//   Int = 8
//   Int = 8
```

In this very simple example `a + 3 * a` is referentially transparent because if we substitute to `a`
with is value `2` the result is the same.

All pure functions are necessarily referentially transparent. Since, by definition, they cannot
access anything other than what they are given, their result must be fully determined by their
arguments.

```scala
def stringLength(input: String): Int = {
  input.length
}

def someCalculation(input: Int): Int = {
  input * 2 - 1
}

// Any of this is equivalent even if just the last evaluation is enough to prove it
someCalculation(stringLength("functional")) + someCalculation(3)
someCalculation("functional".length) + someCalculation(3)
someCalculation(10) + someCalculation(3)
(10 * 2 - 1) + (3 * 2 - 1)
24

// Output:
//   Int = 24
//   Int = 24
//   Int = 24
//   Int = 24
//   Int = 24
```

Let's see a counter example where we use an impure function and study the consequences:

```scala
var counter: Int = 0

def incrementCounter(amount: Int): Int = {
  counter += amount
  counter
}

incrementCounter(2) + incrementCounter(3)
(counter + 2) + (counter + 3)
// What should I put as one number example?

// Output:
//   Int = 7
//   Int = 15
```

The two expressions return different values, as expected because `incrementCounter` is not pure as
it makes use of a shared state in the form of the variable `counter`.

Sounds easy to me! Let's see another example that is a more edge case:

```scala
def factorial(n: Int): Int = {
    // In this example I don't want to deal with error cases, so just return 1 when n < 0
    var accumulator: Int = 1

    for (i <- 1 to Math.max(0, n))
        accumulator = i * accumulator

    accumulator
}
```

Is this function pure or not? Is it referentially transparent or not? The for loop uses the state
represented by the variable `accumulator` so the function is not pure because it has impure code.

But even if we use a shared state the function is referentially transparent because we can replace
the call to this function with its output:

```scala
factorial(3)
6

// Output:
//   Int = 7
//   Int = 15
```

The trick here is that we used **local mutability** and from the point of view of user of the user
the output of `factorial` depends only on the inputs and it will never know that the function makes
use of a shared state. This can be acceptable as the function is externally pure but unless you have
a very good reason like deep performance optimization you shouldn't do it because it will become
easier and easier to fall back to imperative style with all its drawbacks.

At this point you know all the things about referential transparency useful for programming but you
can find more details in the [Wikipedia page][3] and a more practical description in the
[HaskellWiki][7].

## Exercises 3

* Write a pure function `List[A] => Int` that counts the number of elements in the given list. Be
  mindful to not use mutable variables.

## Loops and immutable values

From the previous exercise you should have noticed that it's not possible to use a classic for/while
loop without using mutable variables. This is because of the very nature of the loop itself.

An imperative style loop needs to update a shared state (in many cases a variable defined outside
the loop) to perform it job. The instruction it contains are executed a certain amount of time, in
accordance with the condition of the loop and the result of each iteration is stored in this shared
state that ultimately becomes the result of the entire loop.

Taking this reasoning to its logical conclusion we can say that a loop has side effects and mutable
variables allow these side effects. So they should be avoided.

As [this answer on StackOverflow] points out, using pure functions and immutable variables allows us
to prove facts about our system (functions, data) that we can later use to abstract over the details
and understand

This can sound like a big crippling problem for a developer. How can we work without loops? We have
already started to build the tools necessary to understand how to work in a pure manner in FP and in
the next few chapters we will carry this work on and we will analyse and discuss the techniques used
to overcome this apparent limitation and actually gain more power and elegance from our code:
immutable data structures and recursion.

## Usefulness of pure function signatures

Functions with no inputs or functions that return `Unit` are always impure functions because if they
return unit without side effect then they won't be useful so they must have side effect and if they
require no input then their output must come from a side effect.

This is a very simple example of how the function signature can be of great interest in functional
programming, because they can tell us a lot and we can reason about them in a very general way.

## Working with expressions

**WIP**

In functional programming we like that every statement has a value when evaluated. This ma

## Generating random numbers

## Memoization, or values as functions

There is another way we can understand pure functions. Pure functions are replacement machines that
for every input value substitute an output value.

Using this point of view we can perform a trick. We can tabulate each resulting value of a function
in a table that for every input associates an output or we can memorize in a data structure the
output for each given input.

Let's say the input type is the set of hexadecimal digits from the number `0` to the number `9` and
from letter `a` to letter `f`. The function we want to write converts takes a single digit an
converts it into its corresponding integer. Such a function can be easily written as:

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

What I want to highlight here is that pure functions can actually be represented by simple data and
a relation.

This might seem a waste of time but this technique can be exploited to speed up the calculation of
expensive functions by building a dynamic table of results as we go.

Let's say we have an expensive function `expensiveOperation` that takes a long time to perform its
job. We wrap it inside another function and then we make use of the concept of local mutability and
we define a mutable dictionary that will map the relation between inputs and output values.

Every time we call the function with a given input `x` we look in this dictionary if there is a key
defined for `x`. If there is not we call the expensive function and we save the result in the
dictionary otherwise we return the result immediately.

```scala
import collection.mutable._

// This variable is hidden from the user
var calculateTable: HashMap[Int, Int] = HashMap.empty

def calculate(n: Int): Int = {
  def expensiveOperation(x: Int): Int = {
    Thread.sleep(3000)
    x * x
  }
  calculateTable.getOrElseUpdate(n, expensiveOperation(n))
}

calculate(3)    // The first time you run this it will take 3 seconds
calculate(3)    // This uses the memoized value and returns immediately

// Output:
//   Int = 9
//   Int = 9
```

This technique is called memoization and it's a powerful tool made possible by the use of pure
functions. Most of the times it's not as trivial as here to memoize a function because the values
might be infinite and so on but this is not the place to go deeper.

## Composition

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

## Exercises 4

* For each of the following questions tell the domain and codomain of the function, if the function
  is total or partial and if it is surjective, injective or bijective:

  * a function `String => String` that associate each string with their reverse (e.g. "Scala" ->
    "alacS");

  * a function `Int => Boolean` that associates negative numbers with their positive value;

  * a function `Int => Int` that associates even numbers into their double;

* Given a case class `case class Person(name: String, surname: String)` create a function `Person =>
  (String, String)` that associates an instance of `Person` to a tuple that contains name and
  surname. Is this function a bijection? If yes write also its inverse.

## References

* [Functional programming in Scala][2] Chapter 1
* [Referential transparency - Wikipedia][3]
* [Referential transparency - HaskellWiki][7]
* [The Benefits of Pure Functions][4]
* [Understanding Immutability and Pure Functions (for OOP)][5]
* [Escape from the ivory tower: the Haskell journey][6]
* [Types and Functions][8]
* [Why do immutable objects enable functional programming?][9]

[2]: https://www.manning.com/books/functional-programming-in-scala
[3]: https://www.wikiwand.com/en/Referential_transparency
[7]: https://wiki.haskell.org/Referential_transparency
[4]: https://alvinalexander.com/scala/fp-book/benefits-of-pure-functions
[5]: https://sidburn.github.io/blog/2016/03/14/immutability-and-pure-functions
[6]: https://www.youtube.com/watch?v=re96UgMk6GQ
[8]: https://bartoszmilewski.com/2014/11/24/types-and-functions/
[9]: https://stackoverflow.com/a/12208744/1215156

# Pure functions and referential transparency

Estimated reading time: 23 minutes

In this chapter we'll see what are pure functions and why they are at the center of functional
programming. This is a big topic with plenty of concepts and language features that will take me
several sections to cover but it's at the very foundation of FP and complex software so it's very
important you build a strong foundation on pure functions.

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
> * They are memoizable
> * They can be lazy

The article goes in much more details and I advise you to read it till the end because.

Pure functions are as close as we can get to mathematical functions in programming.

In mathematics the input and output are sets while in programming they are types. Using [the
description of Bartosz of types][7] "the simplest intuition for types is that they are sets of
values.". Scala `Boolean` is a set with two elements called `true` and `false`, `Int` is a set that
contains the integer that your JVM can represent, `String` is a set with infinite elements (limited
by the memory you assign you the JVM).

Then we use our programming language of choice to define how the output relates to the input, using
keywords, variables, calls to other functions and so on, in other terms we program the engine of the
function.

## Exercises 4.1

### 4.1.1

Create a pure function `String => String` that associates each input string into its uppercase
string.

### 4.1.2

Create a pure function `Int => Boolean` that associates even numbers with the value `true` and odd
numbers with the value `false`.

### 4.1.3

Using your result of the exercise of animals in the case classes chapter, create a pure function
with signature `Animal => String` that given an animal it returns the type of animal.

## Referential transparency

Referential transparency is a concept that is very tight to pure functions even if it's strictly not
the same.

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
//   Int = 6
//   Int = 6
```

The trick here is that we used **local mutability** and from the point of view of user of the user
the output of `factorial` depends only on the inputs and it will never know that the function makes
use of a shared state. This can be acceptable as the function is externally pure but unless you have
a very good reason like deep performance optimization you shouldn't do it because it will become
easier and easier to fall back to imperative style with all its drawbacks.

At this point you know all the things about referential transparency useful for programming but you
can find more details in the [Wikipedia page][2] and a more practical description in the
[HaskellWiki][3].

## Working with expressions

Any program is composed of statements and expressions where statements are operations, or actions,
that can be performed and expressions are something that produce a value. We already mentioned
expressions when talking about referential transparency. I summarized this sentence from the article
[Statements and Expressions in Scala][9].

In functional programming we like that every statement has a value when evaluated because we can use
the definition of referential transparency and reason better about our code.

Scala adopts this line of reasoning and everything is an expressions so that constructs that you
would normally consider as statements are in fact expressions. Ifs and pattern matching are examples
of this even when it doesn't seem so.

Here we have a simple `if` expression that prints a different text depending on the value of a
condition:

```scala
if (true) println("true") else println("false")

// Output:
//   true
```

and we can prove that this code has in fact a well defined value:

```scala
val result = if (true) println("true") else println("false")
println(result)

// Output:
//   true
//   ()
```

and this value is `Unit` (the double `()`) because the value that the evaluations of the `if`
statement produces is the value returned by the `println` function and that is `Unit`.

When working inside functions is good practice to never use the `return` keyword because it can
introduce parts of the code that are not evaluated or that return values not compatible with the
return type. In this example we see how a missing `else` statement makes the code failing at
compilation:

```scala
def badStatement(cond: Boolean): String = {
  if (cond) return "A"
}

// Output
//   type mismatch;
//    found   : Unit
//    required: String
//     if (cond)
//     ^
//   Compilation Failed
```

In Scala the value of an expression is always the value of the last expression evaluated, this is
why you don't have to use the `return` keyword inside a function.

## Exercise 4.2

### 4.2.1

Given the following code that contains a component that deal with users and another one that deal
with calculating bills:

```scala
object UserComponent {
  private val users = List("Olivia", "Ruby", "Emily", "Grace", "Jack", "Oliver", "Thomas", "Harry")

  var status = "enabled"

  def getUser(i: Int): String = {
    if (i >= users.length && status == "enabled") {
      status = "illegal"
      ""
    } else if (status == "disabled") {
      throw new IllegalArgumentException()
    } else {
      status = "disabled"
      users(i)
    }
  }
}

object BillComponent {
  def getBillForUser(user: String): Double = {
    UserComponent.status = "updating"
    user.length
  }
}
```

without changing the existing code, create a function that prints the bill of all users.

## Modularity and Composition

The last exercise wanted to show how difficult it can be to program with impure functions and to use
together components that are very tight together. Ok, it's a silly example create with the explicit
purpose of making it difficult but in real life you will have tens or hundreds of component,
probably a shared state somewhere in the form of a database or a connection, you'll have thousand of
lines between functions and you or your colleagues won't notice subtle side effects.

This brings us to the next topic. Composition and modularity are the two key properties of software
that enable us to build complex software systems (complex, not complicated!).

Quoting the definition of modularity from Wikipedia:

> Broadly speaking, modularity is the degree to which a system's components may be separated and
> recombined, often with the benefit of flexibility and variety in use.[1] The concept of modularity
> is used primarily to reduce complexity by breaking a system into varying degrees of
> interdependence and independence across and "hide the complexity of each part behind an
> abstraction and interface.

While, quoting [dictionary.com](https://www.dictionary.com/browse/composition) composition, is:

> The act of combining parts or elements to form a whole.

On their own modules are

* reusable
* testable
* parallelisable
* maintainable

It might be obvious why these properties hold and it's because they all derive from the fact that a
module is, or at least it should be, Because they are self-contained unit you can reuse modules by
simply "plugging" the inputs you want and get from them their output. For the same reason you can
"plug" fake values and look at their output to perform thorough and simple testing.

Composition helps us in making complex software

* scalable
* reasonable

Bigger system are created by composing smaller modules and making them work together using the rules
of composition. Each module is easier to understand as a unit of work and you don't need to know its
internals to understand what it does, in fact a module is an abstraction. The laws of composition
ensure that as things get larger they don't get too complex to understand. This process can be done
in different layers because a module itself, instead of the whole system, can be built by
composition other modules.

And [this article][12] expands on software composition in the specific discuss how developers do it
all the time, even without noticing and gives several examples.

## Function composition

The introduction to modularity and composition should make it clear how important they are. Now a
question: what is the smaller module that you, as a developer, can write? That is a pure function!

Pure functions are the smallest modules you can build that have the properties described above. They
are reusable and testable, they are independent and maintainable. Once you define it it can be
called by other modules or by tests. Being a pure function it can be parallelisable and, if you are
a good developer, small enough that is maintainable.

You could argue that expressions, that we've seen at the beginning of this chapter, are more
fundamental units that a developer can write. But I don't think that's correct because they're not
reusable, you can't call a single expression that you wrote multiple times. You have to write it
again.

Good! So the first step to write composable software is to understand pure functions composition.

Intuitively, two functions are composable if the input of the second can accept the output of the
first one. By giving an example, if we have two functions:

```scala
val f: Int => String = ???
val g: String => Boolean = ???
```

then, because the input of `g` coincides with the output of `f`, we can use them together in another
function by composing them:

```scala
val h: Int => Boolean = f(g(a))
```

Side note: Here i've used Scala function instead of methods because Scala treats functions and
methods in different ways. Functions, like the ones defined above, are instances of object while
methods are directly used as their JVM counterparts. This distinction is caused by design choices
that Scala made to work on the JVM. I will cover few basic things here and in the next chapters but
for the rest internet is full of articles that explain the differences in details. I find [this
Stackoverflow question][13] very good at explaining what they are.

```scala
val f: Int => Int = ???     // This is a function
def f(x: Int) => Int = ???  // This is a method
```

Scala, being a functional language and having functions as first class citizens, allows developers
to use an additional notation to express functions called point-free notation. This is away of
expressing functions that doesn't use variables and it works especially well in function composition
because it removes some redundant information like if we define an argument used only once.

For example point-free notation is very nice when printing a list:

```scala
// Traditional notation
List(1, 2, 3).foreach(x => println(x))

// Reduced notation
List(1, 2, 3).foreach(println(_))

// Point-free notation
List(1, 2, 3).foreach(println)
```

and it works because println is a function with one argument `Any => Unit`. The point-free notation
is particularly nice when used for function composition. Let's see it in action, let's create few
function that manipulate some data.

```scala
case class User(firstName: String, lastName: String)

def alphabeth(size: Int): String                = List.tabulate(size)(i => ('a' + i).toChar).mkString
def split(string: String): (String, String)     = string.splitAt(string.length / 2)
def userFromTuple(data: (String, String)): User = User(data._1, data._2)
```

So, the hypothetical problem we want to solve is to build a user using the functions we have and we
would like to create a function that does that by composing the three functions above:

```scala
// Composing just two of the functions
def createUser(fullName: String): User = {
  val s = split(fullName)
  userFromTuple(s)
}

// Composing all the functions and all in one line
def createRandomuser(i: Int): User = userFromTuple(split(alphabeth(i)))
```

Scala gives use two composition operator: `compose` and `andThen` that can be used to perform
function composition in point-free style, which means applying one function after the other without
indicating the variable being used and creating a new function as a result:

```scala
val f: Double => Int = { _.toString.length }
val g: Int => String = { x => "a" * x }
val h: String => Boolean = { _.isEmpty }

val k1 = g andThen h // <==> h(g(x)) <==> { x => ("a" * x).isEmpty }
val k2 = g compose f // <==> g(f(x)) <==> { x => "a" * (x.toString.length) }

k1(0)
k1(1)
k2(1.2345)

// Output
//   Boolean = true
//   Boolean = false
//   String = "aaaaaa"
```

going back to our previous example we can now compose in a much cleaner and understandable way:

```scala
val comp1: Int => User = (userFromTuple _) compose (split _) compose (alphabeth _)
val comp2: Int => User = (alphabeth _) andThen (split _) andThen (userFromTuple _)

comp1(6)
comp2(2)

// Output
//   User = User("abc", "def")
//   User = User("a", "d")
```

Be aware that the `_` is because we defined these functions as methods, nothing more, and Scala
requires what is called eta-expansion by adding the `_`.

## Exercises 4.3

### Exercise 4.3.1

Given the following code:

```scala
case class User(name: String, age: Int)

def createUser(name: String, age: Int): (Boolean, User) = {
  if (age < 0 || name == "") (false, null)
  else (true, User(name, age))
}

def isAdult(user: User): Boolean = user.age > 18

def createAndCheck(name: String, age: Int): Boolean = ???
```

create a function that given a name and an age determines if a user is adult.

## Composition in the large

Now the real big point to understand is that we want this capability, ease of composition, as much
as we can, we don't always work with simple functions with one argument and one output but
potentially more or with more complex types. Or perhaps we are not working with functions but with
lists and we want to somewhat compose all the elements of the list together or we want to perform
aggregation or we are dealing with deferred results.

It would be great if there were ways to have composition in the bigger.

This is one of the key realizations to do when becoming a functional programmer. You want
composition, you look for composition, you build for composition and you leverage libraries for
composition.

**The central point of this tutorial will be on how to build composable software and how to reuse
it**

## Exercises 4.4

### 4.4.1

We want to work with a counter and write two functions to increment/decrement its value of a given
amount. First solve this exercise writing a function in imperative style and then try to solve the
same problem but using only pure functions. What's the problem you're encountering?

### 4.4.2

Write a pure function that discovers the maximum integer in a list of **positive integers**
`List[Int] => Int` and returns `-1` if the list is empty or if there are negative numbers or any
other sort of issues. Solve the exercise without using scala `.max()` function but using mutable
variables and a for loop. Is it still a pure function? Why?

### 4.4.3

How can you solve the previous exercise if we want the function to be able to work on a list that
contains any integer and not just positive integers? You now need to express a situation where you
can't use the type `Int` to represent an "exceptional" situation because `Int` is part of your
normal output.

## On side effects

So we don't like side effects. Or do we?

Have you noticed that all the side effects mentioned above are somewhat related to interacting with
the world? That's because what side effects are, interactions with the world. So we need them, we
need interactions with the world, that's why we write programs in the first place, to do stuff! To
read and process files, to communicate on the network, to calculate a result and display it. The
reason to run a program is to have side effects!

Pure functions **do** no real-world work.

Yes, this is the catch, pure functions are difficult to work with and make our lives as developers
more unpredictable and frustrating but we need side effects and in functional programming we deal
with them by keeping track of them, enveloping them into containers and pushing them at the side of
our program where we accept side effect to happen but, let's say, under supervision.

Functional programming doesn't eliminate or hide side effect, it pushes them into confined and
controlled spaces where it's easier to work with and this handling is explicit and clear to the
developer that then becomes aware of side effects.

Over the years scientists and programmers all around the world have developed (and are still
actively working on) a lot of techniques and tools to have side effects that we can control and deal
with.

Before these techniques were discovered and developed in Haskell, the researchers that built Haskell
were having a very difficult time to just use pure functions and they didn't really know how
interact with the operating system, read files, execute commands until monads were understood to be
the key of sequential computation and to be useful to wrap side effects! Have a look at the
[interesting story of the development of Haskell][6] to learn more about the history and development
of FP.

I want to add that functional programming is also about working with the tools and techniques to
deal with side effects efficiently and cleanly, to understand and build an intuition and fluency
that allows us developers to reason at a different level about side effects.

Right now is too early to introduce and talk about these tools and techniques, we need a bit more
background but you can already start assimilating the facts that:

* side effects are bad;
* pure functions have no side effects;
* we need side effects to interact with the world;
* thus we need tools and techniques to tightly control and handle side effects;
* we usually handle side effects in a controlled environment at the sides of your core logic.

A very good resource to get an all round understanding of pure function is provided by [Chapter
1 of Functional Programming in Scala][1]

## Exercises 4.5

### 4.5.1

Write a pure function `List[A] => Int` that counts the number of elements in the given list. Be
mindful to not use mutable variables or it wouldn't be a pure function. Is it possible at all?

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
and understand. We will cover immutable data structures in a future chapter but if you like you can
understand how immutable variables are essential for FP in this [Stackoverflow question][8].

This can sound like a big crippling problem for a developer. How can we work without loops? We have
already started to build the tools necessary to understand how to work in a pure manner in FP and in
the next few chapters we will carry this work on and we will analyse and discuss the techniques used
to overcome this apparent limitation and actually gain more power and elegance from our code:
immutable data structures and recursion.

## References

* [Functional programming in Scala][1] Chapter 1
* [Referential transparency - Wikipedia][2]
* [Referential transparency - HaskellWiki][3]
* [The Benefits of Pure Functions][4]
* [Understanding Immutability and Pure Functions (for OOP)][5]
* [Escape from the ivory tower: the Haskell journey][6]
* [Types and Functions][7]
* [Why do immutable objects enable functional programming?][8]
* [Statements and Expressions in Scala][9]
* [Why Isn't Functional Programming the Norm? – Richard Feldman][10]
* [Simplicity in Composition—Adelbert Chang][11]
* [Composing Software: An Introduction][12]
* [Functions vs methods in Scala][13]

[1]: https://www.manning.com/books/functional-programming-in-scala
[2]: https://www.wikiwand.com/en/Referential_transparency
[3]: https://wiki.haskell.org/Referential_transparency
[4]: https://alvinalexander.com/scala/fp-book/benefits-of-pure-functions
[5]: https://sidburn.github.io/blog/2016/03/14/immutability-and-pure-functions
[6]: https://www.youtube.com/watch?v=re96UgMk6GQ
[7]: https://bartoszmilewski.com/2014/11/24/types-and-functions/
[8]: https://stackoverflow.com/a/12208744/1215156
[9]: https://www.learningjournal.guru/article/scala/functional-programming/statements-and-expressions-in-scala/
[10]: https://www.youtube.com/watch?v=QyJZzq0v7Z4
[11]: https://www.youtube.com/watch?v=XV0o-hy1WUM
[12]: https://medium.com/javascript-scene/composing-software-an-introduction-27b72500d6ea
[13]: https://stackoverflow.com/questions/4839537/functions-vs-methods-in-scala

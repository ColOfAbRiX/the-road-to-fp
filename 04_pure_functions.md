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

Here's a simple example in Scala:

```Scala
def double(input: Int): Int = input * 2
def length(input: String): Int = input.length

double(3)
// Output:
//   Int = 6

length("FP")
// Output:
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

```Scala
// def println(x: Any): Unit = Console.println(x)
println("Hello, World!")
// Output:
//   Hello, World!
```

A function can print on the screen. That's not a value that it returns, actually `println` doesn't
return a value but just `Unit`. A function that returns `Unit` has always side effects or it won't
be a useful function.

```Scala
// def readLine(): String = in.readLine()
val line = scala.io.StdIn.readLine()
```

A function that can read some text from the keyboard. This time the function returns something (what
has been read) but there is no input! The internal engine of the functions has no input to work with
and the output is produced in some other way... side effects! A function that produces an output
with an empty argument list is always an impure function or it won't do real work.

```Scala
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

```Scala
// The list is empty, there is no head of the list
List().head
// Output:
//   java.util.NoSuchElementException: head of empty list
```

A sample function can work on a list to get the first element but then it throws an exception before
it can return the head. Yes, an exception is not a return value of a function, it's something else
that the function can do and that you can handle in some specific ways.

```Scala
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

## On side effects

So we don't like side effects. Or do we?

Have you noticed that all the side effects mentioned above are somewhat related to interacting with
the world? That's because what side effects are, interactions with the world. So we need them, we
need interactions with the world, that's why we write programs in the first place, to do stuff! To
read and process files, to communicate on the network, to calculate a result and display it. The
reason to run a program is to have side effects!

Pure functions **do** no real world work.

Yes, this is the catch, pure functions are difficult to work with and make our lives as developers
more unpredictable but we need them. Over the years scientists and programmers all around the world
have developed (and are still actively working on) a lot of techniques and tools to have side
effects that we can control and deal with.

Before these techniques were discovered and developed in Haskell, the researchers that built haskell
were having a very difficult time to just use pure functions and they didn't really know how
interact with the operating system, read files, execute commands! Have a look at the [interesting
story of the development of Haskell](6) to learn more about this process.

I would like to say that functional programming is also about working with the tools and techniques
to work with side effects efficiently.

Right now is too early to introduce and talk about these tools and techniques, we need a bit more
background but you can already start assimilating the facts that:

* side effects are bad;
* pure functions have no side effects;
* we need side effects to interact with the world;
* thus we need tools and techniques to tightly control and handle side effects.

A very good resource to get an all round understanding of pure function is provided by [Chapter
1 of Functional Programming in Scala](2)

## Referential transparency

## Simple examples of both concepts and counter examples

## Show that loops are not good, they require mutability

## Immutability as values that cannot change

## Local mutabilty can be used

## Importance of pure function signatures

Functions with no inputs or functions that return `Unit` are always impure functions. Think of it:
if they

## Working with expressions

## Generating random numbers

## Memoization, or values as functions

## References

* [Functional programming in Scala][2] Chapter 1
* [Referential transparency][3]
* [The Benefits of Pure Functions][4]
* [Understanding Immutability and Pure Functions (for OOP)][5]
* [Escape from the ivory tower: the Haskell journey][6]

[2]: https://www.manning.com/books/functional-programming-in-scala
[3]: https://www.wikiwand.com/en/Referential_transparency
[4]: https://alvinalexander.com/scala/fp-book/benefits-of-pure-functions
[5]: https://sidburn.github.io/blog/2016/03/14/immutability-and-pure-functions
[6]: https://www.youtube.com/watch?v=re96UgMk6GQ

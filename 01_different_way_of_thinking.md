# A different way of thinking

Learning functional programming has been one of the best things I could do in programming. It
chenged the way I reason about code and the way I engineer it, it made me see code in a different
way and it's been really beneficial to simplify my programs in terms of logic and internals.

When learning FP there are several paradigm shifts that will you have to go through and they are all
necessay to become proficient:

* Pure functions and Immutability
* New concepts, techniques and tools
* Declarative programming
* Use math to reason on your code (ok, this one is not really necessary...)

These are really big shifts and they will require a lot of practice because they are very different
from traditional imperative programming and OOP and, at least for me, required to give up many of my
established way of coding. You'll almost need to learn programming from scratch (not really!)

But don't be scared it's this that will grow your horizon! That's the big part of the fun!

At the same time, in Scala at least, the new tools and techniques are mostly optional! One could
say that there are "levels" of functional style because you can work with just pure functions
without introducing anything new or you can start using tools developed to make specific tasks
easier.

Let's see in more details what these paradigm shifts are about

## Pure functions and immutability

The first thing that will have to change is to learn to work with values that cannot be directly
modified and with functions that can't share their state. At the beginning it might seems something
odd and not really useful. Actually it might seems and impediment to work!

But it's just a matter of learning how to deal with this new situation and once understood the
tricks it becomes very trivial to apply them over and over. It's not limiting at all!

But on the other hand we gain a lot in clarity and ease of reasoning. And it's outstanding how many
designs will become much simpler when seen with the lens (pun intended) of FP. This happens because
we can reason about pure functions as black boxes with just an input and an output, like simple
machines that do some processing and nothing more.

We'll see pure functions and immutability in full details in a future chapter.

## New concepts, techniques and tools

As I said erlier you could write pure functions and use immutable values all day long without the
need of anything else and you would be writing functional code.

The issue arises when you start to write more complex programs, start to solve real world problems
and the use cases and scenarios grow. Examples can be managing a program configuration, using a
database, receiing data from a network with its possible failures, validate user inputs and so on.

Sure, you can absolutely just write pure functions and you'll get the job done but the resulting
code will be very intricate, an endless forest of calls and compositions. So you'll try clever
tricks to reuse what you wrote, put functions in libraries, make them generic as "The Good Cooder
Guide (TM)" says to be DRY. But why reinventing the wheel? Here is where the next step comes in.

There are a series of tools in FP that will make your life easier, that you'll be able to reuse and
to compose together like, for instance, the concept of monad or lenses or monoids and, more than
this, these new concept will be very generic, once you have the intuition behind them you'll be able
to see existing code in a new light, as a special case of these concepts and you will refactor
everything to use them.

You will have found an underlying "commoness" to something you previously thought unrelated and this
will improve the reasoning about the code as well as its maintanability.

You will have moved a step further.

## Declarative programming

This is another of the beauty of FP: you'll be able to reason about what you want instead of how to
achieve it and at different level.

The simples example of this I can think of is... doing something on a list! Let's say we have a
`List[String]` and we want to convert each string to upper case. In imperative style this code would
do the job (I'm using Scala to write classic imperative style code. It's verbose on purpose):

```Scala
var inputList = List("apple", "orange", "fig")
var outputList = collection.mutable.ListBuffer[String]()

for { i <- 0 until inputList.length } {
  outputList += inputList(i).toUpperCase()
}
```

What happens here is a lot of things that are **not** related to what I want to do, which is
converting the strings to upper case. What is not related? I hear you say... Well, all the control
structures and all the logic that sits around `.toUpperCase()`. That's the **real work** I want to
do!

In imperative style I need a construct, the `for` loop, that scans my list one item at the time and
then a variable that holds the result, that I fill one bit at the time, with the result of the
operation.

Let's see how I would do it in FP style and then comment on it:

```Scala
var inputList = List("apple", "orange", "fig")
val outputList = inputList.map(_.toUpperCase())
```

Leaving aside that it's much shorter (because it's not always the case) the real difference is that
only two things happen:

* I say what operation to perform on my list, and that is the `map` operation
* I say what operation to perform on a single element of the list with `_.toUpperCase()`

The big shift in paradigm here is that I reason locally instead of globally. Here I just say how to
work on one single element and then I say how to do that on the list as a whole.

If you ask me, that's conceptually pretty neat and powerful.

Bartoz Milewski has a [great post about declarative programming](1) and goes much more philosophical
than I do and I strongly recommend you to read it as it is because it's really deep and interesting.

## Use math to reason on your code

Functional programming has a strong foundation in a branch of mathematics called Category Theory
which is essentialy a tool that can be used to

* express relationships between types
* discover new relationships
* generalize concepts

This is probably the most advanced topic and it's not strictly necessary to become a good FP
developer but it's still a tool and its usefulness is in the hand of the person that codes. It won't
have a use if you don't find one! And if you know category theory then you'll have something more in
your toolbox to use for you own advantage!

It's a very interesting topic that will open your mind to a even higher degree.

Quoting again [Bartosz in this video](2) category theory is the abstraction, is the next level up
that functional languages tend to reach. It's the level above and as such it will give you a bigger
world to explore that you can then render with Scala (or the functional language of your choice)

[1]: https://bartoszmilewski.com/2015/04/15/category-theory-and-declarative-programming/
[2]: https://www.youtube.com/watch?v=I8LbkfSSR58

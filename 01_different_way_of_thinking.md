# A different way of thinking

Learning functional programming (FP) has been one of the best things I could do for my programming
skills. It changed the way I reason about code and the way I solve problems, it made me see code in
a different way and it's been really beneficial to simplify my programs in terms of logic and
internal works.

When learning FP there are several paradigm shifts that you will have to go through and they are all
necessary **to become proficient**:

* pure functions and Immutability;
* new concepts, techniques and tools;
* declarative programming;
* use math to reason on your code (ok, this one is not really necessary...).

In my opinion these are quite big conceptual shifts and they will probably require a lot of practice
before you can comfortably reason and work with them. They are very different from the traditional
imperative programming style that the majority of programmers have used and, at least for me,
they require to give up many of the established practices of coding.

These are also the reason why I find FP so fun and interesting, because they forced me to adapt a
new point of view and see problems from a different angle, increasing the tools at my disposal when
I tackle a problem.

I have highlighted that these changes are needed to become proficient and write good code but you
could say that there are "levels of functional style" where at one end you can work with just pure
functions, composing and calling them, without introducing anything new or, on the other end, you
can make use of tools developed to make specific tasks easier. The choice of where to stand is on
you, your experience as an engineer and your level of comfort with these new tools.

Let's see in more details what these paradigm shifts are about.

## Pure functions and immutability

The first thing that will have to change is to learn to work with with functions that can't access a
shared state and values that cannot be directly modified.

At the beginning it might seem something odd and not really useful. Actually it might seem an
impediment to work! But it's just a matter of learning how to deal with this new constraint and,
once "the tricks" are understood, it becomes very trivial to apply them over and over.

On the other hand, what we lose in terms of being forced to code in a certain way we gain in clarity
and ease of reasoning. It's remarkable how many problems will become simpler when seen with the lens
(pun intended) of FP. This happens because we can reason about pure functions as pure black boxes,
"machines" with just an input and an output that perform a specific action on some data, instead of
tracking what happens to some variable that determines how my function works.

We'll see pure functions and immutable data structures in full details in a future chapter.

## New concepts, techniques and tools

As I said earlier you could write pure functions and use immutable values all day long without the
need of anything else and you would be writing functional code.

Issues will start to arise when you begin to write more complex and structured programs, when you
start to solve real world problems and the use cases and scenarios grow in size and complexity.
Examples of what I mean are managing the configuration of a program, using a database, receiving
data from a network with its possible failures, validate user inputs and so on.

Sure, you can absolutely write pure functions and you'll get the job done but the resulting code
will be intricate, an endless forest of calls and compositions. So you'll try clever tricks to reuse
what you wrote, put functions in libraries, make them generic as "The Good Coder Guide (TM)" tells
you to do to be DRY. But why reinvent the wheel? Here is where the next step comes in.

There are a series of tools in FP that will make your life easier, that you'll be able to reuse and
to compose together like, for instance, the concepts of monad, lenses or monoids and these new
concepts will be very generic and powerful. Once you have the intuition behind them you'll be able
to see your own code that you wrote so far in a new light, as a special case of these concepts and
you will refactor everything to use them.

This is about finding an underlying commonness of problems and, in practice, link together things
that you previously thought unrelated and this will improve your reasoning about the code as well as
its maintanability.

At this stage I'm sure this sounds very generic and abstract but we will see in details what this
means and we'll learn to use these tools. For now just take my word for it!

## Declarative programming

This is another of the beauty of FP: you'll be able to reason about what you want instead of how to
achieve it and at higher level.

The most simple example I can think of is... doing something on a list! Let's say we have a
`List[String]` and we want to convert each string to upper case. In imperative style this code would
do the job (I'm using Scala to write classic imperative style code. It's verbose on purpose):

```Scala
var inputList = List("apple", "orange", "fig")
var outputList = collection.mutable.ListBuffer[String]()
for (i <- 0 until inputList.length) {
  outputList += inputList(i).toUpperCase()
}

println(outputList)
// Output:
//   ListBuffer(APPLE, ORANGE, FIG)
```

This piece of code contains a lot of things that are **not** related to what I want to do. Remember,
the goal of my example is to convert the strings inside a list to upper case. And I hear you say
"what is not related to it?" Well, all the control structures and all the logic that sits around
`.toUpperCase()`. That's the **real work** I want to do!

In imperative style I need a construct, the `for` loop, that scans my list one item at the time and
then a variable that holds the result, then I need to fill the result variable one item at the time,
with the result of each the operation. This sounds easy enough, as the classic approach to the
problem, isn't it?

Let's see how I would do it in FP style:

```Scala
var inputList = List("apple", "orange", "fig")
val outputList = inputList.map(_.toUpperCase())
println(outputList)
// Output:
//   List(APPLE, ORANGE, FIG)
```

Leaving aside that it's much shorter (because it's not always the case) the real difference is how I
reason about solving the problem. Here only two things happen:

* I say what operation to perform on my list and that is the `.map()` operation
* I say what operation to perform on a single element of the list with `.toUpperCase()`

The big shift in paradigm here is that I reason first globally (the `.map()`) and then I focus on
how to solve the problem locally on one item only (what I do inside `.map()`).

If you ask me, that's conceptually pretty neat and powerful.

Bartoz Milewski has a [great post about declarative programming][1] and goes much more philosophical
than I do and I strongly recommend you to read it because it's really deep and interesting.

We will see many more examples of this in these pages!

## Use math to reason on your code

FP has a strong foundation in a branch of mathematics called Category Theory
which is essentially a tool that can be used to:

* express relationships between types;
* discover new relationships;
* generalize concepts.

This is probably the most advanced topic and it's not strictly necessary to become a good FP
developer, but it's still a tool and its usefulness is in the hand of the person that codes. It
won't have a use if you don't find one! And if you know category theory then you'll have something
more in your toolbox to use for you own advantage!

It's a very interesting topic that will open your mind to a even higher degree.

Quoting again [Bartosz in this video][2], category theory is the abstraction, it is the next level
up that functional languages tend to reach. It's the level above and as such it will give you a
bigger world to explore that you can then render with Scala (or the functional language of your
choice)

## About naming

Functional programming was born in academy and it borrows a lot of names from mathematics.

This means that many names that you will encounter might sound scary to you. Names like functors,
comonad coalgebras, lifting or semigroups are certainly very unfamiliar to us programmers and I
found them very cryptic and not understandable when I started to learn FP. But now I want to address
this concern that many beginners might have because I believe it's completely unfounded.

These terms are cryptic just because programmers are not familiar with mathematics and for no other
reason. A name is just a label that is assigned to a concept and what is important is the concept
not the word. FP started in mathematics so things got named by mathematicians first and that's the
end of it, there is no relation to how easy or difficult to learn a name is. Just learn the concept
and assign the label you're being told to it. If it makes it easier you can think of how difficult a
mathematician might find to learn programming terms like REST API, subclassing, interface or Liskov
substitution principle. They are just labels.

For the concern that these terms are not understandable I point the finger to low quality
educational material online or to the wrong understanding of people writing articles. Again, not a
problem of the name itself. And this guide is meant to address precisely this issue.

## References

* [Category Theory and Declarative Programming][1]
* [Category Theory 1.1: Motivation and Philosophy][2]

[1]: https://bartoszmilewski.com/2015/04/15/category-theory-and-declarative-programming/
[2]: https://www.youtube.com/watch?v=I8LbkfSSR58

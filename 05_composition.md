# Composition

Estimated reading time: 9 minutes

This chapter will explain the importance of modularity in system designs and why FP is so good at
it. We will also introduce the basic building block of FP modularity and a glimpse on how to do it
for less trivial situations.

## Exercise 5.1

### 5.1.1

In this exercise we have two components that take care of users and of billing. We want to make use
of these existing functionalities and you have to create a new function that displays the bill for
all the users. Changing the existing code is not allowed but you are allowed to use mutable
variables for simplicity.

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

### 5.1.2

Similar to the previous exercise we want to reuse some existing functionalities but this time the
two components behave differently. You are allowed to use mutable variables for simplicity.

```scala
object UserComponent {
  private val users = List("Olivia", "Ruby", "Emily", "Grace", "Jack", "Oliver", "Thomas", "Harry")

  def getUser(i: Int): String = {
    if (i >= users.length) ""
    else users(i)
  }
}

object BillComponent {
  def getBillForUser(user: String): Double = {
    user.length
  }
}
```

## Modularity and Composition

The last exercise wanted to show how difficult it can be to program with impure functions and to use
together components that are coupled together, even if they are only few lines it can still be
difficult to reason about them. Ok, it's a silly example created with the explicit purpose of making
it difficult but in real life you will have tens or hundreds of components, probably a shared state
somewhere in the form of a database or a connection, you'll have thousand of lines between functions
and you or your colleagues won't notice subtle side effects.

This brings us to the next topic. Composition and modularity are the two key properties of software
design that enable us to build complex software systems (complex, not complicated!).

Quoting the definition of modularity from Wikipedia:

> Broadly speaking, modularity is the degree to which a system's components may be separated and
> recombined, often with the benefit of flexibility and variety in use.[1] The concept of modularity
> is used primarily to reduce complexity by breaking a system into varying degrees of
> interdependence and independence across and "hide the complexity of each part behind an
> abstraction and interface.

While, quoting [dictionary.com](https://www.dictionary.com/browse/composition) composition, is:

> The act of combining parts or elements to form a whole.

On their own modules are;

* reusable
* testable
* parallelisable
* maintainable

It might be obvious why these properties hold and it's because they all derive from the fact that a
module is, or at least it should be, independent, self contained, small box. Because they are a
self-contained unit you can reuse modules by simply "plugging" the inputs you want and get from them
their output. For the same reason you can "plug" fake values and look at their output to perform
thorough and simple testing.

Composition helps us in building complex software that is:

* scalable
* reasonable

Bigger system are created by composing smaller modules and making them work together using the rules
of composition. Each module is easier to understand as a unit of work and you don't need to know its
internals to understand what it does, in fact a module is an abstraction. The laws of composition
ensure that as things get larger they don't get too complex to understand. This process can be done
in different layers because a module itself, instead of the whole system, can be built by
composition other modules.

[This article][1] expands on software composition in the specific, it discusses how developers do
it all the time, even without noticing and gives several examples.

## Function composition

The introduction to modularity and composition should make clear how important they are. Now a
question: what is the smaller module that you, as a developer, can write? That is a pure function!

Pure functions are the smallest modules you can build that have the properties described above. They
are reusable and testable, they are independent and maintainable. Once you define a function, that
function can be called by other code or by tests. Being a pure function it can be used in concurrent
computation and, if you are a good developer, it's small enough that is easily maintainable.

You could argue that expressions, that we've seen at the beginning of this chapter, are more
fundamental units that a developer can write. But I don't think that's correct because they're not
reusable, you can't call a single expression multiple times, you just have to write it again.

Good! So the first step to write composable software is to understand pure functions composition.

Intuitively, two functions are composable if the input of the second function can accept the output
of the first one. By giving an example, if we have two functions:

```scala
val f: Int => String = _.toString
val g: String => Boolean = _ == ""

// Output:
//   f: Int => String
//   g: String => Boolean
```

then, because the input of `g` coincides with the output of `f`, we can define a new function that
passes the result of the first function to the second function by composing them:

```scala
val h: Int => Boolean = x => g(f(x))      // Int => String => Boolean

// Output:
//   h: Int => Boolean
```

Scala, being a functional language and having functions as first class citizens, allows developers
to use an additional notation to express functions called **point-free** notation. This is away of
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

// Point-free notation with less symbols
List(1, 2, 3) foreach println
```

Note that `println` is a function of one argument `Any => Unit`. The point-free notation is
particularly nice when used for function composition.

Scala gives use two nice composition operator: `compose` and `andThen` that can be used to perform
function composition in point-free style, which means applying one function after the other without
indicating the variable being used and creating a new function as a result:

```scala
val f: Double => Int = { _.toString.length }
val g: Int => String = { x => "a" * x }
val h: String => Boolean = { _.isEmpty }

// Output
//   f: Double => Int
//   g: Int => String
//   h: String => Boolean

val k1 = g andThen h  //  <==>  val k1 = h(g(x))  <==>  val k1 = { x => ("a" * x).isEmpty }
val k2 = g compose f  //  <==>  val k2 = g(f(x))  <==>  val k2 = { x => "a" * (x.toString.length) }

// Output
//   k1: Int => Boolean
//   k2: Double => String

k1(0)
k1(1)
k2(1.2345)

// Output
//   Boolean = true
//   Boolean = false
//   String = "aaaaaa"
```

Let's see how the point-free notation can make our function composition clearer in a more
articulated example:

```scala
case class User(firstName: String, lastName: String)

val alphabeth: Int => String              = List.tabulate(_)(i => ('a' + i).toChar).mkString
val split: String => (String, String)     = value => value.splitAt(value.length / 2)
val fromTuple: ((String, String)) => User = data => User(data._1, data._2)
```

In this exercise the problem we want to solve is to build a `User` composing the functions we have
and for this purpose we want to create two functions, one that creates a user given a username and
the other that creates a user with a random name:

```scala
// Composing just two of the functions
def createUser(fullName: String): User = {
  val s = split(fullName)
  fromTuple(s)
}

// Composing all the functions and all in one line
def createRandomUser(i: Int): User = fromTuple(split(alphabeth(i)))
```

going back to our previous example we can now compose in a much cleaner and understandable way:

```scala
// Several examples of "compose" and "andThen"
val createRandomUser1: Int => User = fromTuple compose split compose alphabeth
val createUser1: String => User = split andThen fromTuple
val createRandomUser2: Int => User = createUser1 compose alphabeth
val createRandomUser3: Int => User = alphabeth andThen split andThen fromTuple

createRandomUser1(6)
createRandomUser2(2)

// Output
//   User = User("abc", "def")
//   User = User("a", "d")
```

## Exercises 5.2

### Exercise 5.2.1

Starting with the following code create a function that given a name and an age determines if a user
is adult. Why are we returning a tuple and what's that `Boolean` for?

```scala
case class User(name: String, age: Int)

def createUser(name: String, age: Int): (Boolean, User) = {
  if (age < 0 || name == "") (false, null)
  else (true, User(name, age))
}

def isAdult(user: User): Boolean = user.age > 18

def createAndCheck(name: String, age: Int): (Boolean, User) = ???
```

## Stepping up composition

Now the real big point to understand is that we want to have composable pieces of code and we want
this composition to be easy and painless but we don't always work with simple functions like the
ones we've seen so far, very simple, with one argument and one output but potentially we'll have
more arguments or with more complex types. Or perhaps we are not working with functions but with
lists and we want to somewhat compose all the elements of the list together or we want to perform
aggregation or we are dealing with deferred results.

It would be great if there were ways to have composition in the bigger.

This is one of the key realizations when becoming a functional programmer. You want composition, you
look for composition, you build for composition and you leverage libraries for composition.

**Most of the contents we'll see will revolve around how to make composable code**, from higher
order functions, to functor and monads. We just started the journey and there's still lot more to
cover and understand!

There are several great minds out there that believes that [true software scalability lies in
modularity][2] and that FP is the perfect tool to enable modular software so much that [OOP happened
by accident][3] and what developers really need is not object but modules. The two videos I linked
are very opinionated but I can promise you that if you dig deep into FP you'll see by yourself how
much I think OOP is overrated, despite offering some great solution and tools. And also why Scala is
such a great tool, because amongst other reasons it can mix both worlds.

## References

* [Composing Software: An Introduction][1]
* [Functional Scala - The Many Faces of Modularity by Eric Torreborre][2]
* [Why Isn't Functional Programming the Norm? – Richard Feldman][3]

[1]: https://medium.com/javascript-scene/composing-software-an-introduction-27b72500d6ea
[2]: https://www.youtube.com/watch?v=SfW9w-FogeE
[3]: https://www.youtube.com/watch?v=QyJZzq0v7Z4

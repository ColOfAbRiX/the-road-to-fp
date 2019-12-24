# Scala language feature: Case classes

Case classes in Scala are a somewhat simple concept as described very well in [this StackOverflow
answer][1], that I report here:

> Case classes can be seen as plain and immutable data-holding objects that should exclusively
> depend on their constructor arguments.
>
> In combination with inheritance, case classes are used to mimic algebraic datatypes (we will see
> Algebraic Data Types soon).

The importance of case classes is that they are a tool to _simplify and clean_ the way we think
and work with data in FP. Contrary to OOP where we have objects that update their own internal
state, in FP we usually have data structures and a bunch of functions that can be structured
and organized throughout the code, and that perform operations and transformations on this data.

With this definition in mind, case classes allow us to create these data structures with a clean
syntax and to avoid code that is not strictly necessary to the FP way of working.

Probably the most useful use case of case classes are Algebraic Data Types that we will see in a
future chapter.

To begin working with case classes a great introductory and comprehensive examples are [Demystifying
Scala - Case Classes][2] and [Scala case classes in depth][3]. Have a good read and then come back
here for more details and thoughts on case classes.

## What they add to normal classes

Technically, there is no difference between a `class` and a `case class`: they're both the same
thing under the hood. A case class is a normal class where the compiler adds several predefined
functionalities:

* a companion object which contains a predefined `apply()` and `unapply()` methods to set the class
  members;
* getter (but not setter) methods for the constructor arguments so that they become public fields;
* a `copy()` method is generated to be able to clone an object;
* they provide a visually nice `toString()` method formatting all the fields of the class;
* `hashCode()` and `equals()` methods to compare case classes "the right way";
* they are also instances of `Product` and thus inherit the methods `productElement()`,
  `productArity()` and `productIterator()`.

### Public fields

Defining getters for constructor arguments means that we can define public fields without using the
`val` keyword in the constructor. The following example shows the different behaviour of classes
versus case classes for constructor arguments:

```Scala
class Class1(
  privateField: String,    // This is not public
  val publicField: String  // A normal class requires `val` in the constructor argument
)

case class Class2(publicField: String)  // Here val is not required to make the field public
```

### Changing values

The `copy()` method is an interesting one.

Pure case classes are immutable in nature and cannot update their internal status. The `copy()`
method is used to create an **updated** copy of you instance, it simply creates a new instance of
your class passing all the existing fields to the new instance except the ones you specify manually
with the end result of "updating" these fields. It is a partial implementation of the [prototype
pattern][4].

We'll see immutable data structures in the future.

## Pattern matching with case classes

Pattern matching is a really powerful tool, so much that you will ask yourself how did you do when
you were not using it (spoiler: with a ton of bad looking `if` statements and other tricks)!

In its essence it matches a variable with a given pattern and executes some code for that case. This
definition is simple and yet it has many implication. For instance, we can match a variable against:

* a generic catch all pattern;
* a literal value like `true` or `"this is a string"`;
* a simple type;
* a type that has specific values in its fields;
* a type that has values in its fields but no specified ones;
* a type as above but with its fields composed of other types, recursively;

In particular the last two points add a lot of power. And, more than this, we ask the compiler to:

* match a pattern but only if a specific condition is satisfied (guard);
* assign the matched value to a variable of that type so to **safely** enforce the type;
* assign the values of the fields of the variable to an inner variable that we can use later.

This behaviour is useful in a variety of cases. In its simplest form it can replace `if` statements
with literal values, or it allows the developer to perform different tasks depending on the type of
data being passed, it can extract fields from case classes and even match and extract regular
expressions thanks to the powerful Scala compiler.

A very used technique consists of using case classes (and in particular Algebraic Data Types)
together with pattern matching because it makes it possible to carry around data structures in the form
of case classes, extract, and then operate on their content. I'll present a concrete example
below.

Part of the magic of pattern magic comes from the `unapply()` method that is used by the compiler to
extract the fields values from the variable. The article [Scala pattern matching: apply the
unapply][6] has a good description of `unapply()` and how it works together with pattern matching
and, at the bottom of this page, there is a summary about objects.

Again, this is only a brief summary because other articles cover this topic with very good details.
See the references below for more links.

More reading on [this page][5].

## Build data types with case classes

As said above, case classes are used to define data-holding structures. The following example shows
how we can model a simple shopping cart that is made of a list of purchases made by accounts on
items:

```Scala
final case class Account(id: Int, name: String, address: String)
final case class Item(id: Int, name: String, price: Double)
final case class Purchase(date: String, account: Account, items: Seq[Item])
final case class ShoppingCart(purchases: Seq[Purchase])
```

Another example is a data structure that can express the success or failure of a computation. This
example here is taken from the scala standard library [scala.util.Try][7]:

```Scala
// I only reported the main definitions
sealed abstract class Try[+T]{
final case class Failure[+T](exception: Throwable) extends Try[T]
final case class Success[+T](value: T) extends Try[T]
```

## From values to types

Case classes, together with pattern matching, become a really powerful tool because we can use them
to shift the focus from the values to the types and thus strengthening type safety.

Why is this important? It's because the compiler can't know what value a variable will contain (by
the very definition of variable its value is determined at runtime) but the compiler knows the type
of each variable and we can ask it to fail when it sees something that is not supposed to happen.
The compiler will do more work for us even before we start our application!

The key for this is to start using types themselves as carrier of semantic information. This key
insight is also what will enable much more power and expressiveness in the future when we will see
functors and monads.

Bartosz again explains with a master example how we should think of types and what types and a clever
compiler can do for us in his page [Types and Functions][10]:

> The simplest intuition for types is that they are sets of values.

and more on the usefulness of types:

> The usual goal in the typing monkeys thought experiment is the production of the complete works of
> Shakespeare. Having a spell checker and a grammar checker in the loop would drastically increase
> the odds. The analogue of a type checker would go even further by making sure that, once Romeo is
> declared a human being, he doesn’t sprout leaves or trap photons in his powerful gravitational
> field.

The `Try` above is a great example of this. We have defined two types, both rooted in `Try`: one is
used to carry the message _"I am the result of a successful computation and I contain the result of
that computation of type `T`"_ (and of course the actual result), while an instance of the other one
carries the message _"I am the result of a failed computation and I contain that failure"_.

Once we have this result in a variable we can use pattern matching to see what happened to the
computation and take different actions:

```Scala
import scala.util._

val toConvert = "abc"
val tryConversion = Try(toConvert.toInt)

tryConversion match {
  case Success(number) =>
    println(s"Converted $toConvert into the integer $number")
  case Failure(exception) =>
    println(s"ERROR! $exception")
}

// Output:
//   ERROR! java.lang.NumberFormatException: For input string: "abc"
```

But... we'll see ways to avoid this tedious way of writing and we'll let the libraries manage this.

## Case classes best practices

For this, I refer you straight to [Mark case classes as final][8]

When case classes are used to create algebraic data types you should follow the [Algebraic Data
Types][9] best practices.

## Exercises

* Use case classes to represent a person with its name, surname and age. Create some instances to
  demonstrate the use of your data structures.

* Use case classes to represent a pet with its name and owner. The owner is of type `Person`. Create
  some instances to demonstrate the use of your data structures.

* Use case classes to represent a type of animal. The animals can be `Cat`, `Dog`, `Dolphin`. Is it
  sufficient to use just case classes? Make sure you express that all the case classes are animals.
  Create some instances to demonstrate the use of your data structures.

* Create two instances of `Person` with the same values and then compare them with `==`. Does Scala
  recognise them as different or equals? Why?

## References

* [What is the difference between Scala's case class and class?][1]
* [Demystifying Scala - Case Classes][2]
* [Scala case classes in depth][3]
* [Prototype][4]
* [Pattern Matching][5]
* [Scala pattern matching: apply the unapply][6]
* [Types and Functions][10]
* [scala.util.Try][7]
* [Scala Best Practices - Mark case classes as final][8]
* [Scala Best Practices- Algebraic Data Types][9]

[1]: https://stackoverflow.com/a/2312936/1215156
[2]: https://medium.com/@cachiama/demystifying-scala-case-classes-b4d756959dcd
[3]: http://www.alessandrolacava.com/blog/scala-case-classes-in-depth/
[4]: https://refactoring.guru/design-patterns/prototype
[5]: https://docs.scala-lang.org/tutorials/tour/pattern-matching.html.html
[6]: https://medium.com/wix-engineering/scala-pattern-matching-apply-the-unapply-7237f8c30b41
[7]: https://github.com/scala/scala/blob/2.13.x/src/library/scala/util/Try.scala
[8]: https://nrinaudo.github.io/scala-best-practices/tricky_behaviours/final_case_classes.html
[9]: https://nrinaudo.github.io/scala-best-practices/definitions/adt.html
[10]: https://bartoszmilewski.com/2014/11/24/types-and-functions/

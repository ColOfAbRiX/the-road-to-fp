# Scala language feature: Case classes

Case classes in Scala are a somewhat simple concept as described very well in [this StackOverflow
answer][1], that I report here:

Case classes can be seen as plain and immutable data-holding objects that should exclusively depend
on their constructor arguments.

In combination with inheritance, case classes are used to mimic algebraic datatypes (we will see
Algebraic Data Types soon).

The importance of case classes is that they simplify and clean the way we think and work with data
in FP. Contrary to OOP where we have objects that mutate their own or other object's states, in FP
we usually have data (data structures or similar) and a bunch of functions, structured and organized
throughout the code, that perform some operations and transformations on this data.

In this view case classes allow us to create these data structures and to avoid code that is not
strictly related to this way of working that can pollute cleanliness.

To begin working with case classes great introductory and comprehensive examples are [Demystifying
Scala - Case Classes][2] and [Scala case classes in depth][3]. Have a good read and then come back
here for more details and thoughts on case classes.

## What they add to normal classes

Technically, there is no difference between a class and a case class. A case class is a normal class
where the compiler add some predefined functionalities:

* a companion object which contains predefined `apply()` and `unapply()` methods;
* getter (but no setter) methods for the constructor arguments so that they become public fields;
* a `copy()` method is generated to enable cloning an object;
* they provide a visually nice `toString()` returning all the fields of the class;
* `hashCode()` and `equals()` method to compare case classes "the right way";
* they are also instances of `Product` and thus inherit these methods `productElement()`,
  `productArity()` and `productIterator()`.

At the bottom of this page there's a section that summarizes companion objects, `apply()`, and
`unapply()` methods.

Defining getters for constructor arguments means that we can define public fields without using the
`val` keyword in the constructor:

```Scala
class Class1(
  privateField: String,                 // This is not public
  val publicField: String               // Normal class requires `val` in the constructor argument
)

case class Class2(publicField: String)  // val is not required here to make the field public
```

The `copy()` method is an interesting one. Pure case classes are immutable data and cannot update
their fields. The `copy()` method is used to create an **updated** copy of you instance; it simply
creates a new instance of your class passing all the existing fields to the new instance except the
ones you specify manually with the end result of "updating" these fields. It is a partial
implementation of the [prototype pattern][4].

We'll see immutable data structures in the future.

## Pattern matching with case classes

Pattern matching is a really powerfool tool, so much that you will ask yourself how you did when
you were not using it!

In its essence it matches a variable with a given pattern. This definition is simple and yet it has
many implication. For instance, we can match a variable against:

* a literal value like `true` or `"functional_programming"`;
* a generic catch all pattern.
* a simple type;
* a type that has specific values in its fields;
* a type that has values in its fields but without specified them;
* a type as above but with its fields composed of other types, recursively;

In particular the last two points add a lot of power. And, more than this, we ask the compiler to:

* match a pattern but only if a specific condition is satisfied (guard);
* assign the matched value to a variable of that type so to **safely** enforce the type;
* assign the values of the fields of the variable to an inner variable that we can use later.

Part of the magic of pattern magic comes from the `unapply()` method that is used by the compiler to
extract the field values from the variable. The article [Scala pattern matching: apply the
unapply][6] has a good description of `unapply()` and how it works together with pattern matching
and at the bottom of this page there is a summary about objects.

Again, this is only a brief summary because other articles cover this topic with very good details.
See the references below for more links.

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

Another example is a data structure that can express the success or failure of a computation using
the `scala.util` data structure [Try][8]:

```Scala
sealed abstract class Try[+T]{
final case class Failure[+T](exception: Throwable) extends Try[T]
final case class Success[+T](value: T) extends Try[T]
```

where I only reported the main definitions.

## From values to types

Case classes, together with pattern matching, become a really powerful tool because we can use them
to shift the focus from the values to the types and thus strengthening type safety.

Why is this important? It's because the compiler can't know what value a variable will contain but
it can know what type a variable is and we can ask it to fail when it sees something that is not
supposed to happen. The compiler will do more work for us even before we start our application!

The key for this is to start using types themselves as carrier of semantic information. This key
insight is also what will enable much more power and expressiveness in the future when we will see
functors and monads.

The `Try` above is a great example of this. We have defined two types, both rooted in `Try`: one is
used to carry the message "I am the result of a successful computation and I contain the result of
that computation of type `T`" (and of course the actual result) while an instance of the other one
carries the message "I am the result of a failed computation and I contain that failure".

Once we have this we can use pattern matching to see what happened to the computation and take
different actions:

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

For this, I refer you straight to [Mark case classes as final][6]

When case classes are used to create algebraic data types you should follow the [Algebraic Data
Types][9] best practices.

## References

* [What is the difference between Scala's case class and class?][1]
* [Demystifying Scala - Case Classes][2]
* [Scala case classes in depth][3]
* [Pattern Matching][4]
* [How to use pattern matching in Scala match/case expressions][5]
* [Try.scala][8]
* [Scala Best Practices - Mark case classes as final][6]
* [Scala Best Practices - Algebraic Data Types][9]
* [Scala pattern matching: apply the unapply][7]

[1]: https://stackoverflow.com/a/2312936/1215156
[2]: https://medium.com/@cachiama/demystifying-scala-case-classes-b4d756959dcd
[3]: http://www.alessandrolacava.com/blog/scala-case-classes-in-depth/
[4]: https://docs.scala-lang.org/tutorials/tour/pattern-matching.html.html
[5]: https://alvinalexander.com/scala/how-to-use-pattern-matching-scala-match-case-expressions
[6]: https://nrinaudo.github.io/scala-best-practices/tricky_behaviours/final_case_classes.html
[7]: https://medium.com/wix-engineering/scala-pattern-matching-apply-the-unapply-7237f8c30b41
[8]: https://github.com/scala/scala/blob/2.13.x/src/library/scala/util/Try.scala
[9]: https://nrinaudo.github.io/scala-best-practices/definitions/adt.html

# A quick tour of the Scala object keyword

Estimated reading time: 4 minutes

This chapter is a little refresher on what the Scala `object` keyword does. You can complement this
page with a well written [full article here][1].

## Singleton objects

Scala classes cannot have static variables or methods like other languages, like Java, do. Instead a
Scala class can have what is called a _singleton object_ which is a special instance of a class that
the compiler guarantees to be unique and that makes available in scope without an explicit
instantiation.

A singleton object is declared using the `object` keyword:

```scala
object MyComponent {
  val enabled: Boolean = false
  private val otherConfig: String = "Some config"
}

MyComponent.enabled

// Output:
//   Boolean = false
```

By all means, this is a language-level implementation of the well known [Singleton pattern][2].

## Modules

Another use case for objects is to use them as containers to modularize the application. In this
scenario you stop thinking of `object` as related to OOP and FP and you create an object when you
want to organize your code and place things in a separate place from others. You can also next
`object` one inside the other and give a hierarchical structure to your code. It's a bit like
packages but it gives the developer different features that packages don't. I won't go through these
differences as they're not that important but the point is that you can use `object` to organise
your code. Here is an example of how you can use the keyword:

```scala
object UserModule {
  def createUser(name: String, surname: String): String = s"$name $surname"
  // ... more methods, variables, classes and so on here ...
}

object PetModule {
  object PetOperations {
    def cleanPet(pet: String): String =  s"$pet, cleaned"
    // ... more methods, variables, classes and so on here ...
  }
  def createPet(name: String, age: Int): String =  s"$name, age: $age"
  // ... more methods, variables, classes and so on here ...
}

// Importing from an object
import UserModule._
createUser("Matt", "Smith")

// Accessing the object's members
val pet = PetModule.createPet("fido", 5)
val cleanedPet = PetModule.PetOperations.cleanPet(pet)

// Output:
//   pet: String = "fido, age: 5"
//   cleanedPet: String = "fido, age: 5, cleaned"
```

## Companion Objects

When a singleton object is named the same as a class and it is defined inside the same source file,
it is called a companion object.

A companion object and its class can access each other’s private members (fields and methods) and
this becomes useful whenever you want them to interact. For example you can create a class with a
private constructor but because the companion object can access the private constructor you could
mediate the creation of an instance. Viceversa you could set private constants in the companion
object that only the class will be able to use. These are only two examples of the features of
companion objects.

```scala
object Container { // Container is used to make the example work on REPL

  object MyComponent {
    val enabled: Boolean = false
    private val otherConfig: String = "Some config"
  }

  class MyComponent(name: String) {
    val companionOtherConfig = MyComponent.otherConfig
  }

}
import Container._

new MyComponent("My name").companionOtherConfig

// Output:
//   String = "Some config"
```

When you define a special method named `apply()` in an object, the compiler can call it without
explicitly naming this method which means that instead of writing `MyObject.apply(...)` you simply
write `MyObject(...)`. This is useful in a few cases:

* to call methods with a shorter syntax;
* to shorten the instantiation of new classes (omitting `new`);
* to provide several simplified constructors.

Here is a short example:

```scala
class Loader(val name: String)

object Loader {
  def normal(): Int = 123
  def apply(): Int = 5
  def apply(name: String): Loader = new Loader(name)
  def apply(first: String, second: String): Loader = new Loader(s"$first $second")
}

Loader.normal()             // As usual
Loader()                    // Equivalent to Loader.apply()
Loader("Matt Smith").name
Loader("Matt", "Smith").name

// Output:
//   Int = 123
//   Int = 5
//   String = "Matt Smith"
//   String = "Matt Smith"
```

## Pattern matching

There is another special method that can be defined in a object called `unapply()` and it is known
as extractor. This method is used to "deconstruct" objects into its components, or "extract" the
components, that are returned by the method and it's used by pattern matching to perform its duty.
We'll explain morea about pattern matching in the next chapter. In the following example we can see
how both `apply()` and `unapply()` are used to construct and deconstruct a sample object:

```scala
class Person(val name: String, val age: Int)

object Person {
  def apply(name: String, age: Int): Person = new Person(name, age)
  def unapply(p: Person): Tuple2[String, Int] = (p.name, p.age)
}

val matt = Person("Matt Smith", 30)
Person.unapply(matt)

// Output:
//   (String, Int) = ("Matt Smith", 30)
```

If we want to use extractors into pattern matching the `unapply` method must return a type that has
a `isEmpty` and a `get` method as [explained here][3]. For example we can make `unapply()` return an
`Option` because `Option` has the two methods `isEmpty` and `get`:

```scala
class Person(val name: String, val age: Int)

object Person {
  def apply(name: String, age: Int): Person = new Person(name, age)
  def unapply(p: Person): Option[(String, Int)] = Some(p.name, p.age)
}

val matt = Person("Matt Smith", 30)

matt match {
  case Person(name, age) => s"Name: $name, Age: $age"
}

// Output:
//   String = "Name: Matt Smith, Age: 30"
```

## References

* [Companion Objects][1]
* [Singleton Pattern][2]
* [Why I have to return Some in unapply method][3]

[1]: https://hello-scala.com/409-scala-companion-objects.html
[2]: https://refactoring.guru/design-patterns/singleton
[3]: https://stackoverflow.com/a/46897645/1215156

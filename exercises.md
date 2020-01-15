# Proposed solutions for exercises

## Chapter 3 - Case Classes

### Exercises 1

Use case classes to represent a person with its name, surname and age. Create some instances to
demonstrate the use of your data structures.

Use case classes to represent a pet with its name and owner. The owner is of type `Person`. Create
some instances to demonstrate the use of your data structures.

Create a new instance of a person and associate with it one of the existing instances of pet created
in the previous exercise.

```scala
case class Person(name: String, surname: String, age: Int)

val matt = Person("Matt", "Smith", 34)
val robert = Person("Robert", "Wilson", 21)

// Output:
//   author: Person = Person("Matt", "Smith", 34)
//   robert: Person = Person("Robert", "Wilson", 21)

case class Pet(name: String, owner: Person)

val lucy = Pet("Lucy", matt)
val fido = Pet("Fido", matt)
val shadow = Pet("Shadow", robert)

// Output:
//   lucy: Pet = Pet("Lucy", Person("Matt", "Smith", 34))
//   fido: Pet = Pet("Fido", Person("Matt", "Smith", 34))
//   shadow: Pet = Pet("Shadow", Person("Robert", "Wilson", 34))

val mike = Person("Michael", "Harris", 50)
val fido2 = fido.copy(owner = mike)

// Output:
//   mike: Person = Person("Michael", "Harris", 50)
//   fido2: Pet = Pet("Fido", Person("Michael", "Harris", 50))
```

### Exercises 2

Use case classes to represent a type of animal. The animals can be `Cat`, `Dog`, `Dolphin`, cats
have a name and age, dogs just a name and dolphins a name and a weight. Is it sufficient to use just
case classes? Make sure you express the fact that cats, dogs and dolphins are animals. Create some
instances to demonstrate the use of your data structures.

Once you've created the instances for the previous exercise, use pattern matching so that you can
print a different description of each type of animal and then you print the name of the animal.

```scala
trait Animal
case class Cat(name: String, age: Int) extends Animal
case class Dog(name: String) extends Animal
case class Dolphin(name: String, weight: Double) extends Animal

val lucy = Cat("Lucy", 3)
val fido = Dog("Fido")
val shadow = Dolphin("Shadow", 62.4)

// Output:
//   lucy: Cat = Cat("Lucy", 3)
//   fido: Dolphin = Dolphin("Fido", 6.0)
//   shadow: Dog = Dog("Shadow")

def greet(animal: Animal): Unit = {
  animal match {
    case Cat(name, _)     => println(s"Hi, I'm a cat and my name is $name")
    case Dog(name)        => println(s"Woof! I'm $name")
    case Dolphin(name, _) => println(s"I'm $name and I like to swim. I'm a dolphin!")
  }
}

greet(lucy)
greet(fido)
greet(shadow)

// Output:
//   "Hi, I'm a cat and my name is Lucy"
//   "Woof! I'm Fido"
//   "I'm Shadow and I like to swim. I'm a dolphin!"
```

### Exercises 3

Create two instances of `Person` with the same values and then compare them with `==`. Does Scala
recognise them as different or equals? Why? Now _modify_ one of the two instances (using `.copy()`)
and try to compare it again with the unmodified one. Are they still equals?

Use case classes to represent a package that can contain other packages or objects. To give some
more details, one package can either contain one or more objects (you can use a `String` to
represent "objects") or one or more package that in turn can contain what we just described. This is
a recursive structure. Create some instances to see how it works.

```scala
case class Person(name: String, surname: String, age: Int)

val matt = Person("Matt", "Smith", 34)
val otherMatt = Person("Matt", "Smith", 34)
val robert = otherMatt.copy(name = "Robert", surname = "Harris")

matt == otherMatt
matt == robert
otherMatt == robert

// Output:
//   Boolean = true
//   Boolean = false
//   Boolean = false

trait Things
case class Package(items: List[Things]) extends Things
case object Object extends Things

val myDrawer = Package(List(
  Package(List(
    Package(List.empty)
  )),
  Package(List(Object, Object)),
  Package(List(
    Package(List(Object)),
    Object
  )),
))

// Output:
//   myDrawer: Package = Package(List(Package(List(Package(List()))), Package(List(Object, Object)), Package(List(Package(List(Object)), Object))))
```

## Chapter 4 - Pure functions and referential transparency

### Exercises 1

Create a pure function `String => String` that associates each input string into its uppercase
string.

Create a pure function `Int => Boolean` that associates even numbers with the value `true` and odd
numbers with the value `false`.

Using your result of the exercise of animals in the case classes chapter, create a pure function
with signature `Animal => String` that given an animal it returns the type of animal.

```scala
def toUpperCase(input: String): String = input.toUpperCase()

// Returning a function
def isEven: Int => Boolean = _ % 2 == 0

// Same as Chapter 3, Exercises 2
trait Animal
case class Cat(name: String, age: Int) extends Animal
case class Dog(name: String) extends Animal
case class Dolphin(name: String, weight: Double) extends Animal

def animalToString(animal: Animal): String = animal match {
  case Cat(_, _)     => "Cat"
  case Dog(_)        => "Dog"
  case Dolphin(_, _) => "Dolphin"
}
```

### Exercises 2

We want to work with a counter and increment and decrement its value of a given amount. First solve
this exercise in imperative style and then try to solve the same problem but using only pure
functions. What's the problem you're encountering?

Write a pure function that discovers the maximum integer in a list of **positive integers**
`List[Int] => Int` and returns `-1` if the list is empty or if there are negative numbers or any
other sort of issues. Solve the exercise using mutable variables and a for loop. Is it still a pure
function? Why?

How can you solve the previous exercise if we want the function to be able to work on a list that
contains any integer and not just positive integers?

```scala
```

### Exercises 3

* Write a pure function `List[A] => Int` that counts the number of elements in the given list. Be
  mindful to not use mutable variables.

```scala
// It's not possible!
```

### Exercises 4

For each of the following questions tell the domain and codomain of the function, if the function is
total or partial and if it is surjective, injective or bijective:

* a function `String => String` that associate each string with their reverse (e.g. "Scala" ->
  "alacS");

* a function `Int => Boolean` that associates negative numbers with their positive value;

* a function `Int => Int` that associates even numbers into their double;

Given a case class `case class Person(name: String, surname: String)` create a function `Person =>
(String, String)` that associates an instance of `Person` to a tuple that contains name and surname.
Is this function a bijection? If yes write also its inverse.

```scala
```

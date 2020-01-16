# Proposed solutions for exercises

## Chapter 0 - Introduction

None

## Chapter 1 - A different way of thinking

None

## Chapter 2 - Objects

None

## Chapter 3 - Case Classes

### Exercises 3.1

#### 3.1.1, 3.1.2, 3.1.3

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

### Exercises 3.2

#### 3.2.1, 3.2.2

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

### Exercises 3.3

#### 3.3.1

Create two instances of `Person` with the same values and then compare them with `==`. Does Scala
recognise them as different or equals? Why? Now _modify_ one of the two instances (using `.copy()`)
and try to compare it again with the unmodified one. Are they still equals?

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
```

#### 3.3.2

Use case classes to represent a package that can contain other packages or objects. To give some
more details, one package can either contain one or more objects (you can use a `String` to
represent "objects") or one or more package that in turn can contain what we just described. This is
a recursive structure, in particular it's a tree. Create some instances to see how it works.

```scala
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

### Exercises 4.1

#### 4.1.1, 4.1.2, 4.1.3

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

### Exercises 4.2

#### 4.2.1

We want to work with a counter and write two functions to increment/decrement its value of a given
amount. First solve this exercise writing a function in imperative style and then try to solve the
same problem but using only pure functions. What's the problem you're encountering?

```scala
var counter: Int = 0

def incrementImperative(amount: Int): Unit = {
  counter += amount
}

def incrementPure(counter: Int, amount: Int): Int = {
  counter + amount
}

// Checking initial value
counter
// Output:
//   Int = 0

// Incrementing and display counter
incrementImperative(5)
counter
// Output:
//   Int = 5

// Incrementing a displaying counter
incrementPure(counter, 3)
counter
// Output:
//   Int = 8
//   Int = 5
```

#### 4.2.2

Write a pure function that discovers the maximum integer in a list of **positive integers**
`List[Int] => Int` and returns `-1` if the list is empty or if there are negative numbers or any
other sort of issues. Solve the exercise without using scala `.max()` function but using mutable
variables and a for loop. Is it still a pure function? Why?

```scala
def maxPositive(input: List[Int]): Int = {
  if (input.isEmpty || input.exists(_ < 0)) -1
  else {
    var max = 0
    for (x <- input)
      max = Math.max(max, x)
    max
  }
}

maxPositive(List(0, 6, 12, 7))
maxPositive(List())
maxPositive(List(0, -3, 12, 7))

// Output:
//    Int = 12
//    Int = -1
//    Int = -1
```

#### 4.2.3

How can you solve the previous exercise if we want the function to be able to work on a list that
contains any integer and not just positive integers? You now need to express a situation where you
can't use the type `Int` to represent an "exceptional" situation because `Int` is part of your
normal output.

```scala

// Using a tuple to represent if a result is valid or not
type ValidatedResult = (Boolean, Int)
def max1(input: List[Int]): ValidatedResult = {
  if (input.isEmpty) (false, 0)
  else {
    var max = 0
    for (x <- input)
      max = Math.max(max, x)
    (true, max)
  }
}

// Using Option to represent the same thing
def max2(input: List[Int]): Option[Int] = {
  if (input.isEmpty) None
  else {
    var max = 0
    for (x <- input)
      max = Math.max(max, x)
    Some(max)
  }
}

max1(List(0, 6, 12, 7))
max1(List())
max1(List(0, -3, 12, 7))

// Output:
//   (Boolean, Int) = (true, 12)
//   (Boolean, Int) = (false, 0)
//   (Boolean, Int) = (true, 12)

max2(List(0, 6, 12, 7))
max2(List())
max2(List(0, -3, 12, 7))

// Output:
//   Option[Int] = Some(12)
//   Option[Int] = None
//   Option[Int] = Some(12)
```

### Exercises 4.3

#### 4.3.1

Write a pure function `List[A] => Int` that counts the number of elements in the given list. Be
mindful to not use mutable variables. Is it possible at all?

```scala
// It's not possible!
```

### Exercises 4.4

#### 4.4.1

For each of the following questions tell the domain and codomain of the function, if the function is
total or partial and if it is surjective, injective or bijective:

##### 4.4.1.1

A function `String => String` that associate each string with their reverse (e.g. "Scala"->"alacS")

```scala
def reverse: String => String = { _.reverse }
def reverseInverse: String => String = reverse

reverse("ABC")
reverseInverse("CBA")
reverseInverse(reverse("ABC")) == "ABC"

// Output:
//   String = "CBA"
//   String = "ABC"
//   Boolean = true
```

The function domain is the `String` set and its codomain is the entire `String` set.

It is total because it works for any string.

It is surjective because it uses all elements of its codomain.

It is injective because for each input string it always produce a different output string.

Because it's surjective and injective it is also bijective. It's inverse function is itself.

##### 4.4.1.2

A function `Int => Int` that associates negative numbers with their positive value;

```scala
def abs: Int => Int = { input =>
  if (input < 0) -input else input
}

abs(3)
abs(0)
abs(-2)

// Output:
//   Int = 3
//   Int = 0
//   Int = 2
```

The function domain is the `Int` set and its codomain is the positive subset of the `Int` set.

It is total because it works for any number.

It is **not** surjective because it uses only positive numbers.

It is **not** injective because there are two input elements associated to each output element (e.g.
both -4 and 4 in the input are associated with 4 in the output)

The function is not bijective.

##### 4.4.1.3

A function `Int => Int` that associates even numbers into their double;

```scala
def doubleEven: PartialFunction[Int, Int] = {
  case even if even % 2 == 0 => even * 2
}

doubleEven(0)
doubleEven(2)
doubleEven(1)

// Output:
//   res10: Int = 0
//   res11: Int = 4
//   scala.MatchError: 1 (of class java.lang.Integer)
```

The function domain is the even subset of the `Int` set and is a subset of the `Int` set.

It is partial because it is not defined for odd inputs.

It is **not** surjective because it uses only even numbers.

It is injective because each distinct input is associated with a different output value.

The function is not bijective.

#### 4.4.2

Given a case class `case class Person(name: String, surname: String)` create a function `Person =>
(String, String)` that associates an instance of `Person` to a tuple that contains name and surname.
Is this function a bijection? If yes write also its inverse.

```scala
case class Person(name: String, surname: String)

def toTuple(person: Person): (String, String) = {
  (person.name, person.surname)
}

def fromTuple(input: (String, String)): Person = {
  Person(input._1, input._2)
}

val author = Person("Matt", "Smith")
val tupled = toTuple(author)
var backAgain = fromTuple(tupled)

backAgain == author

// Output
//   author: Person = Person("Matt", "Smith")
//   tupled: (String, String) = ("Matt", "Smith")
//   backAgain: Person = Person("Matt", "Smith")
//   Boolean = true
```

Yes, the function is bijective and you can write its inverse.

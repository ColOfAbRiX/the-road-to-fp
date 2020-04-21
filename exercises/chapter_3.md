# Proposed solutions for exercises

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
//   fido: Dog = Dog("Fido")
//   shadow: Dolphin = Dolphin("Shadow", 62.4)

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

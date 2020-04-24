# Proposed solutions for exercises

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

### Exercise 4.2

#### 4.2.1

Given the following code that contains a component that deal with users and another one that deal
with calculating bills:

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

solution

```scala
var i = 0
var condition = true
while (condition) {
  UserComponent.status = "enabled"
  val user = UserComponent.getUser(i)
  if (user == "") condition = false
  else {
    val bill = BillComponent.getBillForUser(user)
    println(bill)
    i += 1
  }
}
```

### Exercise 4.3

#### 4.3.1

Given the following code create a function that given a name and an age determines if a user is
adult.

```scala
case class User(name: String, age: Int)

def createUser(name: String, age: Int): (Boolean, User) = {
  if (age < 0 || name == "") (false, null)
  else (true, User(name, age))
}

def isAdult(user: User): Boolean = user.age > 18

def createAndCheck(name: String, age: Int): Boolean = {
  val (isValid, user) = createUser(name, age)
  if (isValid) isAdult(user)
  else false
}
```

### Exercises 4.4

#### 4.4.1

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

#### 4.4.2

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

#### 4.4.3

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

// Throwing an exception, but that would disrupt the normal
// execution flow
def max3(input: List[Int]): Int = {
  if (input.isEmpty) throw new IllegalArgumentException()
  else {
    var max = 0
    for (x <- input)
      max = Math.max(max, x)
    max
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

max3(List(0, 6, 12, 7))
max3(List())
max3(List(0, -3, 12, 7))

// Output:
//   Int = 12
//   java.lang.IllegalArgumentException
//   Int = 12
```

### Exercises 4.5

#### 4.5.1

Write a pure function `List[A] => Int` that counts the number of elements in the given list. Be
mindful to not use mutable variables. Is it possible at all?

```scala
// It's not possible!
```

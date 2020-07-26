# Proposed solutions for exercises

## Chapter 5 - Pure functions and referential transparency

### Exercise 5.1

#### 5.1.1

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

solution:

```scala
def displayAllBills() = {
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
}
```

#### 5.1.2

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

solution:

```scala
var i = 0
var user = UserComponent.getUser(i)
while (user != "") {
  println(user)
  i += 1
  user = UserComponent.getUser(i)
}
```

### Exercise 5.2

#### 5.2.1

Starting with the following code create a function that given a name and an age determines if a user
is adult. Why are we returning a tuple and what's that `Boolean` for?

```scala
case class User(name: String, age: Int)

def validateName(name: String): (Boolean, String) =
  if (name != null && name.nonEmpty) (true, name) else (false, null)

def validateAge(age: Int): (Boolean, Int) =
  if (age > 0) (true, age) else (false, 0)

def createUser(name: String, age: Int): (Boolean, User) = {
  val (nameValid, validName) = validateName(name)
  val (ageValid, validAge) = validateAge(age)
  (nameValid && ageValid, User(validName, validAge))
}

createUser("Matt Smith", 34)
createUser("", -1)

// Output:
//   (Boolean, User) = (true, User("Matt Smith", 34))
//   (Boolean, User) = (false, User(null, 0))
```

We use a tuple to indicate if the functions were able to compute a meaningful output. If the boolean
component of the tuple is `true` then the second component is a valid result, in case it's `false`
the second component has no meaning.

#### 5.2.2

In a similar fashion to the above exercise we now want to play with composition of numerical
functions. Mathematical operations are not always defined on all the real numbers so we take care of
their domain by checking the input value.

```scala
def sqrt(value: Double): (Boolean, Double) =
  if (value >=0 ) (true, Math.sqrt(value)) else (false, 0)

def ln(value: Double): (Boolean, Double) =
  if (value >=0 ) (true, Math.log(value)) else (false, 0)

def div(num: Double, den: Double): (Boolean, Double) =
  if (den != 0) (true, num / den) else (false, 0)
```

Given a number x, create the following calculation functions:

* `f1(x: Double): (Boolean, Double)` such that the output is `sqrt(2.0 * x)`
* `f2(x: Double): (Boolean, Double)` such that the output is `div(sqrt(x), x - 3.0)`
* `f3(x: Double): (Boolean, Double)` such that the output is `div(sqrt(2.0 * x), ln(x - 1.0) - 1.0)`

```scala
def f1(x: Double): (Boolean, Double) = sqrt(2.0 * x)

def f2(x: Double): (Boolean, Double) = sqrt(x) match {
  case (true, sqrtResult) => div(sqrtResult, x - 3.0)
  case (false, _)         => (false, 0)
}

def f3(x: Double): (Boolean, Double) = {
  val (sqrtValid, sqrtResult) = sqrt(2.0 * x)
  val (lnValid, lnResult) = ln(x - 1.0)

  (sqrtValid, lnValid) match {
    case (true, true) => div(sqrtResult, lnResult - 1)
    case (_, _)       => (false, 0)
  }
}

f1(2.0)
f1(-4.0)
// Output:
//   (Boolean, Double) = (true, 2.0)
//   (Boolean, Double) = (false, 0.0)

f2(5.0)
f2(2.0)
// Output:
//   (Boolean, Double) = (true, 1.118033988749895)
//   (Boolean, Double) = (true, -1.4142135623730951)

f3(Math.exp(1.0) + 1.0)
f3(10.0)
// Output:
//   (Boolean, Double) = (false, 0.0)
//   (Boolean, Double) = (true, 3.7354194356333017)
```

### Exercises 5.3

#### 5.3.1

Looking at exercise 5.2.1, can the function `createUser` call the validation functions in parallel?
What about the functions three solution functions (f1, f2 and f3) of exercise 5.2.2, can they call
the mathematical functions in parallel? What's the difference?

The `createUser` function can indeed execute the validation function in parallel because there's no
dependency between the two validation steps. The functions of exercise 5.2.2, on the other hand,
cannot because several calls need to wait for the previous function to have produced a result.

This is the difference between the two exercises. In 5.2.1 we have a composition of independent
computations while in 5.2.2 we have composition of sequential computations.

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

Given the following code create a function that given a name and an age determines if a user is
adult. Why are we returning a tuple and what's that Boolean for?

```scala
case class User(name: String, age: Int)

def createUser(name: String, age: Int): (Boolean, User) = {
  if (age < 0 || name == "") (false, null)
  else (true, User(name, age))
}

def isAdult(user: User): Boolean = user.age > 18

def createAndCheck(name: String, age: Int): (Boolean, User) = ???
  val (isValid, user) = createUser(name, age)
  if (isValid) (true, isAdult(user))
  else (false, null)
}
```

We use a tuple to indicate if the functions were able to compute a meaningful output. If the boolean
component of the tuple is `true` then the second component is a valid result, in case it's `false`
the second component has no meaning.

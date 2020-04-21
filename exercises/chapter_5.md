# Proposed solutions for exercises

## Chapter 5 - Advanced functions concepts

### Exercises 5.1

#### 5.1.1

Implements the functions with the following definitions. Note that there might be any number of
implementation, none, one or more (though not too many):

```scala
// f
def f1[A](a: A, b: A): A = a
def f1[A](a: A, b: A): A = b

// g
def g[A, B](a: A, b: B): A = a

// h
def h[A, B](a: A, b: B): B = b
```

#### 5.1.2

Implements the functions with the following definitions:

```scala
// f
def f[A, B](as: List[A]): List[B] = Nil

// g
def g1[A, B](as: Option[A]): Option[B] = None
def g2[A, B](as: Option[A]): Option[B] = as

// h
def h1[A](as: List[A]): List[A] = Nil
def h2[A](as: List[A]): List[A] = as
def h3[A](as: List[A]): List[A] = {
  if (as.isEmpty) Nil else as.head :: Nil
}
def h4[A](as: List[A]): List[A] = {
  if (as.isEmpty) Nil else as.tail
}

// i
def i1[A, B](as: List[A], f: A => B): List[B] = Nil
def i2[A, B](as: List[A], f: A => B): List[B] = {
  if (as.isEmpty) Nil else f(as.head) :: Nil
}
def i3[A, B](as: List[A], f: A => B): List[B] = {
  if (as.isEmpty) Nil else f(as.head) :: h3(as.tail, f)
}
```

How many implementations did you find? Why is that?

The signatures `g`, `h` and `i` have multiple implementations and this number is bounded by the
number of different input values we have. For `Option` we only have 2 implementations and for `List`
the number is bounded by the elements of the list.

This happens because we have lost parametricity in favour of using `Option` and `List`.

### Exercises 5.2

#### 5.2.1

For each of the following questions tell the domain and codomain of the function, if the function is
total or partial and if it is surjective, injective or bijective:

##### 5.2.1.1

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

##### 5.2.1.2

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

##### 5.2.1.3

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

#### 5.2.2

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

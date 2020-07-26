# Scala language feature: Lazyness

## Overview of definition keywords

## Lazy val

```scala
lazy val a: Int = {
    println("set!")
    3
}
```

## By-name parameters

```scala
def test(value: => Int): Int = {
    if (value > 0) 0 else value * 2
}
test {
    println("test")
    scala.util.Random.nextInt
}
```

<https://stackoverflow.com/questions/4543228/whats-the-difference-between-and-unit>

## Lazyness is like functions as values

## Streams as infinite lists

## Simple example of DSL

## References

* [How to Use By-Name Parameters in Scala][1]
* [Intro to Functional Streams for Scala (FS2)][2]
* [Functional programming in Scala][3] Chapter 5
* [Lazy Evaluation][4]

[1]: https://alvinalexander.com/scala/fp-book/how-to-use-by-name-parameters-scala-functions
[2]: https://www.youtube.com/watch?v=cahvyadYfX8&t=741s
[3]: https://www.manning.com/books/functional-programming-in-scala
[4]: https://www.scala-exercises.org/scala_tutorial/lazy_evaluation

## Functions as Object Members (Methods)

```scala
object Padding:
  def padLines(text: String, minWidth: Int): String =
    val paddedLines =
      for line <- text.linesIterator yield
        padLine(line, midWidth)
    paddedLines.mkString("\n")

  private def padLine(line: String, minWidth: Int): String =
    if line.length >= minWidth then line
    else line + " " * (minWidth - line.length)
```

## Inner Functions

```scala
object Padding:
  def padLines(text: String, minWidth: Int): String =
    def padLine(line: String): String =
      if line.length >= minWidth then line
      else line + " " * (minWidth - line.length)
    val paddedLines =
      for line <- text.linesIterator yield
        padLine(line, midWidth)
    paddedLines.mkString("\n")
```

## Function Literals

```scala
val increment = (x: Int) => x + 1
// or

val incrementBy2 = (x: Int) =>
  val increment = 2
  x + increment
// or

val increment = (x) => x + 1
// or

val increment = x => x + 1
// or

val increment = _ + 1
```

## Partially Applied Functions

```scala
def sum(a: Int, b: Int, c: Int): Int = a + b + c

val add2 = sum(2, _, _)

val add2And5 = sum(2, 5, _) // OR add2(5, _)
```

## Closures

```scala
val more = 1
val addMore = (x: Int) => x + more
addMore(10) // 11

// but

var more = 5
more = 10
val addMore (x: Int) => x + more
more = 7
addMore(10) // 20
```

Closures **close** the function literal by capturing the bindings of free variables. A function literal with no free variables is a **closed term**.

The closure will bind the free variables as they are at the time of definition. If a closure closes over a `var` variable, the value of the variable at that moment will be the value closed into the function literal, even if it changes afterwards.

## Repeated Parameters

```scala
def echo(args: String*) =
  for arg <- args do println(arg)
```

The type of a repeated parameter `T*` is `Seq[T]`.

```scala
echo(Seq("Hello", "World")) // this will fail
echo(Seq("Hello", "World")*) // this will compile
echo("Hello", "World") // this will also compile
```

## Named Arguments

```scala
def speed(distance: Float, time: Float) = distance / time
speed(100, 10)
// alternatively
speed(distance = 100, time = 10)
// or even
speed(time = 10, distance = 100)
```

## Default Parameter Values

```scala
def point(x: Int = 0, y: Int = 0) = (x, y)
point(2, 4)
point(x = 7)
point(y = 19)
```

## 'SAM' (single abstract method) Types

```scala
trait Increaser:
  def increase(i: Int): Int

def increaseOne(increaser: Increaser): Int =
  increaser.increase(1)

// then...

increaseOne(
  new Increaser:
    def increase(i: Int): Int = i + 7
)

// or

increaseOne(i => i + 7)
```

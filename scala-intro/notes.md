# Scala Introduction

> Learning objectives
> - Understand Scala basic syntax
> - Understand fp in Scala
> - Understand lists in Scala

Boolean literals: ` false || true `
Numeric literals: ` 1 + 2 `
String literals: ` ("hello" + " " + "world").length `
Use of Java libraries
```
val dir = java.io.File("/tmp")
dir.listFiles.filter(f => f.isDirectory && f.getName.startsWith("c"))
```

### Everything in Scala is an object
` 5:Int ` is an object of type ` Int ` with methods: ` 5.toDouble  `
Methods can have symbolic names (see scala.Int): ` 5.+ (6) `
scala.runtime.RichInt adds more methods: ` 5.max (6) `
Any unary function e1.f(e2) can be written as ` e1 f e2 `
` 5 + 6 ` is ` 5.+(6) `
`5 max 6 ` is ` 5.max(6) `

### Scala type checking

- Scala performs static type checks
    ` def f() = 5 - "hello" //rejected by type checker `
- REPL prints types of expressions
- Java type hierarchy is embedded in Scala
    - Java primitive types are Scala value types
    - Java reference types are Scala ref types
    - ` java.lang.Object ` is ` scala.AnyRef `


### Mutable and Immutable Variables

#### Mutable

- ##### Java
```
int x = 10;        // declare and initialize x
x = 11;            // assignment to x OK
```

- ##### C
```
int x = 10;        // declare and initialize x
x = 11;            // assignment to x OK
```

- ##### Scala
```
var x = 10         // declare and initialize x
x = 11             // assignment to x OK
```

#### Immutable

- ##### Java
```
final int x = 10;  // declare and initialize x
x = 11;            // assignment to x fails
// error: cannot assign a value to final variable x
```

- ##### C
```
const int x = 10;  // declare and initialize x
x = 11;            // assignment to x fails
// error: assignment of read-only variable ‘x’
```

- ##### Scala
```
val x = 10         // declare and initialize x
x = 11             // assignment to x fails
// error: reassignment to val
```


### Methods

- Parameters **require** type annotations

```
def plus (x:Int, y:Int) : Int = x + y
def times (x: Int, y:Int)     = x * y
```

- Return types can often be inferred but are **required for recursive methods**
- Body of a method is an expression; its value is returned

- Conditional expresssions

```
def fact (n:Int) : Int = if n <= 1 then 1 else n * fact (n - 1)
```

- Compound expressions for side-effects
```
def fact(n:Int) : Int = 
  println("called with n=%d".format(n))
  if n <= 1 then 
    println("no recursive call")
    1 
  else
    println("making recursive call")
    n * fact(n - 1)
```


### Methods vs. Fields

- `def` can be used non-parameterized: ` def x = 5 `; non-strict
- `val` declares a variables: ` val x = 5 `; strict, initialized only once
- `lazy val` : memoized ` def `, initialized on demand

#### Scala
```
class C:
  val x = 1
  lazy val y = 1 + 2
  def z = 1
```

#### Expressed in Java
```
public class C {
  private final int x = 1;
  private Integer y = null;
  public int x() { return x; }
  public int y() {
    if (y == null) y = 1 + 2;
    return y;
  }
  public int z() { return 1; }  
}
```


### Mutable fields

#### Scala

```
class C:
    val x = 1
    var z = 1
```

#### Java
```
public class C {
  private final int x = 1;
  private int z = 1;
  public int x() { return x; }
  public int z() { return z; }
  public void z_$eq(int z) { this.z = z; }
}
```

### Structured Data

- Tuples: fix # of heterogeneous items ` (1, "hello") `
- Lists: variable number of homogeneous items
    ` List(1,2,3) ` or ` 1::2::3::Nil `
- Immutable and mutable variants
- Pattern matching to decompose structured data into its components

### Mutability: Fields vs. Data
- Field mutability is different from data mutability

- #### Java has *mutable* linked lists by default
```
List<Integer> xs = new List<> ();
final List<Integer> ys = xs; // aliasing
xs.add (4); ys.add (5); // list is mutable through both references
xs = new List<> ();     // reference is mutable
ys = new List<> ();     // fails; reference is immutable
```
- #### Scala has *immutable* linked lists by default

```
var xs = List (4, 5, 6)
val ys = xs
xs (1) = 7; ys (1) = 3  // fails; list is immutable
xs = List (0)           // reference is mutable
ys = List ()            // fails; reference is immutable
```


### Tuples

#### Scala tuples

```
val p : (Int, String) = (5, "hello")
val x : Int = p(0)
```

#### Expressed in Java

```
public class Pair<X,Y> {
  final X x;
  final Y y;
  public Pair (X x, Y y) { this.x = x; this.y = y; }
}
Pair<Integer, String> p = new Pair<> (5, "hello");
int x = p.x;
```

### Linked Lists

- Scala's ` :: ` is an infix operator to construct lists

```
val xs = 11 :: (21 :: (31 :: (41 :: Nil))) // List(11, 21, 31, 41)
val xs = 11 :: 21 :: 31 :: 41 :: Nil       // right associative
// method-call style, not encouraged!
val xs = Nil.::(41).::(31).::(21).::(11)
```

- List constructors
```
List (1, 2, 1 + 2)
1 :: 2 :: (1+2) :: Nil
```

### List projections

- Projections extract components of a list: ` head ` and ` tail `
```
xs.head
xs.tail
```

### Summary
- Scala combines functional programming and object-oriented programming
- built in support for tuples

# Functions in Go

**← [Back to INDEX](INDEX.md)**

A **function** is a named block of code that does one job. You can **call** it from other places and **return** results. Functions help you avoid repeating code and keep programs organized.

---

## A simple function

**Define** a function with **`func`**, then its **name**, then **parameters** in parentheses, then the **body** in braces.

```go
package main

import "fmt"

func sayHello() {
    fmt.Println("Hello!")
}

func main() {
    sayHello()  // prints: Hello!
}
```

Here **`sayHello`** has no parameters and no return value. You just call it with **`sayHello()`**.

---

## Functions with parameters

**Parameters** are inputs you pass into the function. You give each one a **name** and a **type**.

```go
package main

import "fmt"

func greet(name string) {
    fmt.Println("Hello,", name)
}

func main() {
    greet("Alice")   // Hello, Alice
    greet("Bob")     // Hello, Bob
}
```

**Pattern:** `func functionName(parameterName type) { ... }`

---

## Multiple parameters

You list them one after another, each with its type.

```go
package main

import "fmt"

func add(a int, b int) int {
    return a + b
}

func main() {
    sum := add(3, 5)
    fmt.Println(sum)  // 8
}
```

If several parameters share the same type, you can write: `a, b int` instead of `a int, b int`.

---

## Return values

Use **`return`** to send a value back. The **return type** is written after the closing `)` of the parameters.

```go
package main

import "fmt"

func multiply(x int, y int) int {
    return x * y
}

func main() {
    result := multiply(4, 5)
    fmt.Println(result)  // 20
}
```

**Pattern:** `func name(params) returnType { ... return value }`

---

## Multiple return values

Go allows **multiple return values**. This is often used to return a result and an error.

```go
package main

import "fmt"

func divide(a, b int) (int, bool) {
    if b == 0 {
        return 0, false  // cannot divide by zero
    }
    return a / b, true
}

func main() {
    result, ok := divide(10, 2)
    if ok {
        fmt.Println("Result:", result)  // Result: 5
    } else {
        fmt.Println("Error: division by zero")
    }
}
```

---

## Named return values

You can **name** the return values. Then you can use **`return`** without listing the values (they are returned automatically).

```go
package main

import "fmt"

func getCoordinates() (x int, y int) {
    x = 10
    y = 20
    return  // same as: return x, y
}

func main() {
    a, b := getCoordinates()
    fmt.Println(a, b)  // 10 20
}
```

Named returns are initialized to zero, so you can assign to them and then just write **`return`**.

---

## Variadic functions

A **variadic function** accepts **zero or more** values of the same type for one parameter. You write **`...Type`** for that parameter. Inside the function it behaves like a **slice**.

```go
package main

import "fmt"

// sum takes zero or more ints and returns their sum
func sum(nums ...int) int {
    total := 0
    for _, n := range nums {
        total += n
    }
    return total
}

func main() {
    fmt.Println(sum())           // 0
    fmt.Println(sum(1, 2, 3))    // 6
    fmt.Println(sum(10, 20))     // 30

    // You can spread a slice with ...
    nums := []int{1, 2, 3, 4}
    fmt.Println(sum(nums...))    // 10
}
```

**Pattern:** `func name(prefix Type, rest ...Type)`. Only the **last** parameter can be variadic. To pass a slice, use **`slice...`**.

---

## Summary

| Idea | Example |
|------|---------|
| No params, no return | `func sayHi() { }` |
| With params | `func greet(name string) { }` |
| With return | `func add(a, b int) int { return a + b }` |
| Multiple returns | `func div(a, b int) (int, bool) { return a/b, true }` |
| Named returns | `func get() (x int, y int) { x=1; y=2; return }` |
| Variadic | `func sum(nums ...int) int` – zero or more `int`s; use `slice...` to pass a slice |

**← [Back to INDEX](INDEX.md)** | Next: [09-arrays-and-slices.md](09-arrays-and-slices.md) – **Arrays and Slices**: lists of data.

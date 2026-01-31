# Generics in Go

**← [Back to INDEX](INDEX.md)**

**Generics** (introduced in Go 1.18) let you write **functions** and **types** that work with **multiple types** without repeating code. You use **type parameters** so the same logic can apply to **int**, **float64**, **string**, or your own types.

---

## Why use generics?

- **Reuse code** – one function for many types (e.g. “sum numbers” for both **int** and **float64**).
- **Type safety** – the compiler still checks types; no **any** or interface{} for this.
- **Less duplication** – no need to write **SumInt**, **SumFloat**, **SumString** separately.

---

## A simple generic function

You declare **type parameters** in square brackets before the normal parameters. Here **T** is a type parameter; the **constraint** **any** means “any type.”

```go
package main

import "fmt"

func Print[T any](s []T) {
    for _, v := range s {
        fmt.Println(v)
    }
}

func main() {
    Print([]int{1, 2, 3})
    Print([]string{"a", "b", "c"})
}
```

**`[T any]`** means: “**T** can be any type.” So **Print** works for **[]int**, **[]string**, or any other slice type.

---

## Constraint: limit which types are allowed

Instead of **any**, you can use a **constraint** so **T** is only certain types (e.g. numbers). You define a constraint as an **interface** that lists allowed types.

```go
package main

import "fmt"

type Number interface {
    int | int64 | float64
}

func Sum[T Number](nums []T) T {
    var sum T
    for _, n := range nums {
        sum += n
    }
    return sum
}

func main() {
    ints := []int{1, 2, 3}
    fmt.Println(Sum(ints))  // 6
    floats := []float64{1.1, 2.2, 3.3}
    fmt.Println(Sum(floats))  // 6.6
}
```

**`int | int64 | float64`** means: **T** must be one of these types. So **Sum** works only for number types, and the compiler still checks everything.

---

## Multiple type parameters

You can have more than one type parameter.

```go
func Pair[T, U any](first T, second U) (T, U) {
    return first, second
}

func main() {
    a, b := Pair("hello", 42)
    fmt.Println(a, b)  // hello 42
}
```

---

## Generic types (e.g. a container)

You can define **types** with type parameters too.

```go
type Box[T any] struct {
    Value T
}

func main() {
    intBox := Box[int]{Value: 42}
    strBox := Box[string]{Value: "hello"}
    fmt.Println(intBox.Value, strBox.Value)  // 42 hello
}
```

**Box[int]** and **Box[string]** are different types, but both use the same **Box** definition.

---

## Summary

| Idea | Example |
|------|---------|
| Type parameter | `func F[T any](x T) { }` |
| Constraint | `type Number interface { int \| float64 }` then `func Sum[T Number](...)` |
| Multiple params | `func Pair[T, U any](a T, b U)` |
| Generic type | `type Box[T any] struct { Value T }` |

Generics help you write **one** piece of code that works with **many** types in a type-safe way.

---

**You’ve reached the end of this Go guide.**

**← [Back to INDEX](INDEX.md)** to see all topics. Keep practicing with small programs; try changing the examples and see what happens.

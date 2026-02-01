# Basic Types in Go

**← [Back to INDEX](INDEX.md)**

Go has built-in **types** for common data: numbers, text, and true/false values. Here they are in a simple way.

---

## What is a type?

A **type** tells Go what kind of data you have: a whole number, a decimal number, a line of text, or true/false. That way Go knows how to store it and what you can do with it.

---

## Integer types (whole numbers)

Used for counting and whole numbers.

| Type | Meaning | Example |
|------|----------|--------|
| `int` | Whole number (size depends on system) | `var n int = 42` |
| `int8`, `int16`, `int32`, `int64` | Fixed-size integers | `var x int32 = 1000` |
| `uint` | Whole number, no sign (only ≥ 0) | `var u uint = 10` |

**Simple example:**

```go
package main

import "fmt"

func main() {
    var age int = 25
    var count int32 = 100
    fmt.Println(age, count)  // 25 100
}
```

---

## Float types (decimal numbers)

Used for numbers with a decimal point.

| Type | Meaning |
|------|---------|
| `float32` | About 7 decimal digits |
| `float64` | More precise (Go uses this by default for decimals) |

```go
package main

import "fmt"

func main() {
    var price float64 = 19.99
    temperature := 36.6
    fmt.Println(price, temperature)
}
```

---

## String type (text)

A **string** is a piece of text. In Go, strings are in **double quotes** `"..."`.

```go
package main

import "fmt"

func main() {
    var name string = "Alice"
    greeting := "Hello, World!"
    fmt.Println(name, greeting)
}
```

**Important:** Strings are **immutable** – once created, you cannot change the characters inside. You can only create new strings.

---

## Bool type (true or false)

A **bool** can only be **`true`** or **`false`**. Useful for conditions and flags.

```go
package main

import "fmt"

func main() {
    var isActive bool = true
    isEmpty := false
    fmt.Println(isActive, isEmpty)  // true false
}
```

---

## Zero values

If you declare a variable and **don’t give it a value**, Go gives it a default called the **zero value**:

| Type | Zero value |
|------|------------|
| Numbers (`int`, `float64`, etc.) | `0` |
| `string` | `""` (empty string) |
| `bool` | `false` |

```go
package main

import "fmt"

func main() {
    var x int
    var s string
    var b bool
    fmt.Println(x, s, b)  // 0  false
}
```

---

## Summary

| Type | Purpose | Example |
|------|----------|--------|
| `int` | Whole numbers | `var n int = 42` |
| `float64` | Decimal numbers | `var f float64 = 3.14` |
| `string` | Text | `var s string = "hi"` |
| `bool` | True/false | `var ok bool = true` |

**← [Back to INDEX](INDEX.md)** | Next: [05-operators.md](05-operators.md) – **Operators**: math, comparison, and logic.

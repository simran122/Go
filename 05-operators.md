# Operators in Go

**← [Back to INDEX](INDEX.md)**

**Operators** are symbols that do something with values: add, compare, combine conditions, etc. Here are the main ones in simple form.

---

## Arithmetic operators (math)

Used to do basic math on numbers.

| Operator | Meaning | Example |
|----------|--------|--------|
| `+` | Add | `3 + 5` → `8` |
| `-` | Subtract | `10 - 4` → `6` |
| `*` | Multiply | `6 * 7` → `42` |
| `/` | Divide | `15 / 4` → `3` (integers) |
| `%` | Remainder | `15 % 4` → `3` |

**Example:**

```go
package main

import "fmt"

func main() {
    a, b := 10, 3
    fmt.Println(a + b)   // 13
    fmt.Println(a - b)   // 7
    fmt.Println(a * b)   // 30
    fmt.Println(a / b)   // 3 (integer division)
    fmt.Println(a % b)   // 1 (remainder)
}
```

**Note:** `+` is also used to **join strings**: `"Hello" + " " + "World"` → `"Hello World"`.

---

## Comparison operators (is it equal? bigger?)

These give a **true/false** result. Used a lot in `if` and loops.

| Operator | Meaning | Example |
|----------|--------|--------|
| `==` | Equal | `5 == 5` → `true` |
| `!=` | Not equal | `5 != 3` → `true` |
| `<` | Less than | `3 < 5` → `true` |
| `<=` | Less than or equal | `5 <= 5` → `true` |
| `>` | Greater than | `10 > 5` → `true` |
| `>=` | Greater than or equal | `5 >= 5` → `true` |

```go
package main

import "fmt"

func main() {
    x, y := 10, 20
    fmt.Println(x == y)  // false
    fmt.Println(x < y)   // true
    fmt.Println(x != y)  // true
}
```

---

## Logical operators (combine true/false)

Used to combine conditions.

| Operator | Meaning | Example |
|----------|--------|--------|
| `&&` | AND – both must be true | `true && false` → `false` |
| `||` | OR – at least one true | `true || false` → `true` |
| `!` | NOT – flip true/false | `!true` → `false` |

```go
package main

import "fmt"

func main() {
    age := 25
    hasTicket := true
    canEnter := age >= 18 && hasTicket
    fmt.Println(canEnter)  // true
}
```

---

## Operator precedence (order of operations)

Go does **multiplication and division** before **addition and subtraction**, like in math.

```go
result := 2 + 3 * 4  // 3*4 first, then +2 → 14
```

Use **parentheses** when you want to change the order:

```go
result := (2 + 3) * 4  // 5 * 4 → 20
```

---

## Summary

| Kind | Operators | Use |
|------|-----------|-----|
| Arithmetic | `+ - * / %` | Math on numbers (and `+` for strings) |
| Comparison | `== != < <= > >=` | Compare two values → true/false |
| Logical | `&&` `||` `!` | Combine or negate true/false |

**← [Back to INDEX](INDEX.md)** | Next: [06-conditionals.md](06-conditionals.md) – **Conditionals**: `if` and `switch` to make decisions.

# Variables in Go

**← [Back to INDEX](INDEX.md)**

A **variable** is a name for a place in memory where you store a value. In Go you can declare variables in a few ways.

---

## Why use variables?

- So you can **reuse** a value (e.g. a number or a name) without typing it again.
- So you can **change** the value later if needed.
- So your code is easier to read (e.g. `age` instead of `25` everywhere).

---

## Way 1: Full declaration with `var`

You say: “I want a variable named `name`, and it will hold text (a string).”

```go
package main

import "fmt"

func main() {
    var name string = "Alice"
    fmt.Println(name)  // prints: Alice
}
```

**Pattern:** `var variableName type = value`

- **`var`** – keyword for declaring a variable
- **`name`** – the name you give the variable
- **`string`** – the type (text)
- **`"Alice"`** – the value

---

## Way 2: Short declaration with `:=`

Go can **guess the type** from the value. You use **`:=`** (colon equals) inside a function.

```go
package main

import "fmt"

func main() {
    age := 25
    city := "New York"
    fmt.Println(age, city)  // prints: 25 New York
}
```

**Rule:** Short declaration **only inside functions** (e.g. inside `main`).

---

## Way 3: Declare first, assign later

You can declare a variable and give it a value in a later line.

```go
package main

import "fmt"

func main() {
    var score int
    score = 100
    fmt.Println(score)  // prints: 100
}
```

If you don’t assign a value, Go gives it a **zero value** (e.g. `0` for numbers, `""` for strings).

---

## Multiple variables at once

You can declare several variables in one line.

```go
package main

import "fmt"

func main() {
    var a, b int = 10, 20
    x, y := 1, 2
    fmt.Println(a, b, x, y)  // 10 20 1 2
}
```

---

## Ignoring a value with `_`

Sometimes a function returns more than one value, but you only care about one. Use **`_`** to ignore the rest.

```go
package main

import "fmt"

func main() {
    r, w, _ := getPipe()  // we ignore the third value
    fmt.Println(r, w)
}

func getPipe() (int, int, int) {
    return 1, 2, 3
}
```

---

## Summary

| Style | Example | When to use |
|-------|---------|-------------|
| Full | `var name string = "Alice"` | When you want to be explicit about type |
| Short | `name := "Alice"` | Inside functions, when type is obvious |
| Declare then assign | `var x int` then `x = 5` | When you assign value later |

**Remember:** Short declaration (`:=`) is only for **inside functions**.

**← [Back to INDEX](INDEX.md)** | Next: [03-constants.md](03-constants.md) – Values that **never change**: **constants**.

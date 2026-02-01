# Conditionals in Go

**← [Back to INDEX](INDEX.md)**

**Conditionals** let your program **choose what to do** based on a condition (true or false). In Go we use **`if`** and **`switch`**.

---

## The `if` statement

If a condition is **true**, the code inside the braces runs. Otherwise it is skipped.

```go
package main

import "fmt"

func main() {
    age := 20
    if age >= 18 {
        fmt.Println("You are an adult.")
    }
}
```

**Pattern:** `if condition { ... }`

---

## `if` with `else`

When the condition is **false**, you can run something else with **`else`**.

```go
package main

import "fmt"

func main() {
    score := 45
    if score >= 60 {
        fmt.Println("You passed.")
    } else {
        fmt.Println("You did not pass.")
    }
}
```

---

## `if` with `else if`

You can check **several conditions** one after another.

```go
package main

import "fmt"

func main() {
    grade := 85
    if grade >= 90 {
        fmt.Println("A")
    } else if grade >= 80 {
        fmt.Println("B")
    } else if grade >= 70 {
        fmt.Println("C")
    } else {
        fmt.Println("Need to improve")
    }
}
```

---

## Short statement before the condition

You can put a **short statement** (like a variable declaration) before the condition, separated by a semicolon. The variable exists only inside the `if` (and its `else` blocks).

```go
package main

import "fmt"

func main() {
    if x := 10; x > 5 {
        fmt.Println("x is greater than 5")
    }
}
```

---

## The `switch` statement

**`switch`** is a clean way to choose one of many options based on a value.

```go
package main

import "fmt"

func main() {
    day := "Monday"
    switch day {
    case "Monday":
        fmt.Println("Start of the week")
    case "Friday":
        fmt.Println("Almost weekend")
    case "Saturday", "Sunday":
        fmt.Println("Weekend!")
    default:
        fmt.Println("Midweek")
    }
}
```

- **`switch day`** – we look at the value of `day`
- **`case "Monday":`** – if `day` is `"Monday"`, run this block
- **`case "Saturday", "Sunday":`** – one case can have several values
- **`default:`** – if no case matches, run this (like `else`)

**Important:** In Go, **only one case runs** – there is no “fall through” to the next case like in some other languages.

---

## `switch` without a value (like if-else-if)

If you write **`switch`** with **no value**, it’s like a chain of **if/else if**. Each **`case`** is a condition (true/false).

```go
package main

import "fmt"

func main() {
    score := 75
    switch {
    case score >= 90:
        fmt.Println("Excellent")
    case score >= 70:
        fmt.Println("Good")
    case score >= 50:
        fmt.Println("OK")
    default:
        fmt.Println("Keep trying")
    }
}
```

---

## Summary

| Statement | Use |
|-----------|-----|
| `if condition { }` | Run code when condition is true |
| `if ... else { }` | Choose between two branches |
| `if ... else if ... else { }` | Choose among several branches |
| `switch value { case ... }` | Choose by value (one of many options) |
| `switch { case condition: }` | Choose by conditions (like if-else-if) |

**← [Back to INDEX](INDEX.md)** | Next: [07-loops.md](07-loops.md) – **Loops**: repeat actions with **`for`** and **`range`**.
